<properties
    pageTitle="Praca z zaufanego kolekcjami | Microsoft Azure"
    description="Dowiedz się, najważniejsze wskazówki dotyczące pracy z zaufanego zbiorów."
    services="service-fabric"
    documentationCenter=".net"
    authors="JeffreyRichter"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="03/28/2016"
    ms.author="jeffreyr" />

# <a name="working-with-reliable-collections"></a>Praca z kolekcjami niezawodne

Usługa tkaninie oferuje stanowe model programowania dostępny .NET deweloperzy za pośrednictwem zaufanego zbiorów. W szczególności tkaninie Usługa udostępnia zaufanego słownika i klas zaufanego kolejki. Korzystając z tych klas swój stan jest partycją (w przypadku skalowalność), replikowane (dostępności) i wykonany w partycją (w przypadku kwasu znaczeń właściwych). Załóżmy Spójrz na typowe zastosowania obiektu zaufanego słownika i zobacz, jakie jego faktycznie wykonywanie.

~~~
retry:
try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to  
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) { 
   await Task.Delay(100, cancellationToken); goto retry; 
}
~~~

Wszystkie operacje na niezawodne słownik obiektów (z wyjątkiem ClearAsync, czyli nie można cofnąć), wymagają obiektu ITransaction. Ten obiekt jest skojarzony z nią dowolną i wszystkie zmiany, które próbujesz wprowadzić do dowolnego zaufanego słownika i/lub zaufanego w kolejce obiektów w jedną partycją. Uzyskiwanie ITransaction obiektu, dzwoniąc partycją jest metoda CreateTransaction i StateManager.
 
W kodzie powyżej obiekt ITransaction jest przekazywany do metody AddAsync zaufanego słownika. Wewnętrznie metody słownika, które akceptuje klucz potrwać czytnika-blokadę skojarzonego z kluczem. Jeśli metodę zmienia wartość klucza, metoda wymaga blokady zapisu w kluczu, a jeśli metodę tylko z wartości klucza, następnie blokada odczytu, jest przyjmowana przy użyciu klucza. Ponieważ AddAsync zmienia wartość klucza na wartość nowy, przekazany, jest przyjmowana blokady zapisu klucza. Tak Jeśli wątków 2 (lub więcej) dodanie wartości z tym samym kluczem w tym samym czasie, jeden wątek uzyskiwanie blokady zapisu i blokuje innych wątków. Domyślnie blok metod dla maksymalnie 4 sekundy na uzyskanie blokady; po 4 sekundy metody Zgłoś TimeoutException. Metoda overloads istnieje pozwala przekazywać jawne limit czasu, jeśli chcesz.
 
Zazwyczaj użytkownik wpisuje swój kod reakcji na TimeoutException im i ponawianie próby cała operacja (jak pokazano w kodzie powyżej). W prostych kod I jest po prostu nawiązywania połączeń z Task.Delay przechodzące 100 milisekund zawsze. Jednak w rzeczywistości mogą być lepiej zamiast niektórych rodzajów wykładniczego wycofywania opóźnienie.
 
Gdy zostanie pobrana blokady, AddAsync dodaje odwołania do obiektów klucz i wartość do wewnętrznego słownika tymczasowe skojarzone z obiektem ITransaction. Można to zrobić dostarczanie znaczeń właściwych odczytu i właścicielem zapisu. Oznacza to, że po skontaktowaniu się z AddAsync później połączenia do TryGetValueAsync (przy użyciu tego samego obiektu ITransaction) zwróci wartość, nawet jeśli nie masz jeszcze zatwierdzeniu transakcji. Następnie AddAsync serializes klucz i wartość obiekty tablice bajtów i dołącza macierzach bajt w pliku dziennika w węźle lokalnym. Na koniec AddAsync wysyła tablice bajtów do pomocniczej replik, więc te same informacje klucza/wartości. Mimo że informacje klucz i wartość zostały zapisane w pliku dziennika, informacje nie są uwzględniane część słownik przed przekazaniem transakcji skojarzonych z nimi. 

W kodzie powyżej połączenie CommitAsync zatwierdza wszystkie operacje transakcji. W szczególności dołącza Zatwierdź informacje do pliku dziennika w węźle lokalnym i wysyła również rekordu Zatwierdź do wszystkich replik pomocniczą. Gdy odpowiedział kworum (większość) repliki, wszystkie zmiany danych są traktowane jako stałe i wszystkie blokady skojarzone z klawiszami, które zostały manipulować przy użyciu obiektu ITransaction wydawanych tak wątków transakcje można modyfikować tych samych klawiszy i ich wartości.

Jeśli CommitAsync nie jest wywoływana (zwykle z powodu wyjątek jest), obiekt ITransaction otrzymuje usunięty. Podczas usuwania obiektu ITransaction niezatwierdzonym, tkaninie usługi dołącza przerwanie informacji pliku dziennika węźle lokalnym i nic się nie musi być wysyłana do dowolnego pomocniczej replik. A następnie wydane wszystkie blokady skojarzone z klawiszami, które zostały manipulować za pośrednictwem transakcji.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Jak unikać ich i typowych problemów
Teraz, gdy zostanie sposób działania niezawodne zbiory wewnętrznie, Spójrzmy na kilka typowych niewłaściwym ich. Zobacz poniższy kod:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes, 
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
~~~

Pracując ze słownikiem zwykła .NET, możesz dodać klucz wartość do słownika, a następnie zmień wartość właściwości (na przykład LastLogin). Jednak kod nie będą działać ze słownikiem zaufanego. Pamiętaj z wcześniejszych dyskusji, połączenie AddAsync serializes obiekty klucza/wartości do tablice bajtów zapisze tablic do lokalnego pliku i wysyła je do pomocniczej replik. Jeśli później zmienić właściwość, spowoduje to zmianę wartości właściwości w pamięci. nie ma wpływu lokalnego pliku lub dane wysyłane do repliki. Jeśli ulega awarii procesu, co znajduje się w pamięci jest generowany z dala od komputera. Wraz z procesem nowego lub innego replice staje się podstawowy, stare wartość właściwości jest, jakie opcje są dostępne. 

Nie mogę podkreśla, tak jak łatwo jest nawiązać rodzaju błąd, jak pokazano powyżej. I tylko poznasz błąd jeśli się, że proces awarii. Właściwy sposób wpisz kod jest po prostu odwrócić dwa wiersze:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync(); 
}
~~~

Oto kolejny przykład przedstawiający typowego błędu:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync(); 
   }
}
~~~

Ponownie, przy użyciu zwykłego .NET słowników, kod powyżej działa prawidłowo i czy typowych wzorzec: projektanta wyszukiwanie wartości przy użyciu klucza. Jeśli istnieje wartość, projektanta zmienia wartość właściwości. Jednak z zaufanego kolekcjami kod wykazuje ten sam problem, jak już omówiony: __nie należy zmodyfikować obiektu, gdy masz poświęcić zbioru zaufanego.__
 
Właściwy sposób aktualizacji wartości w zbiorze zaufanego jest odwołać się do istniejącej wartości i należy rozważyć, czy obiekt odwołują się to odwołanie niezmienne. Następnie utwórz nowy obiekt, który jest dokładną kopię oryginalnego obiektu. Teraz można modyfikować stan ten nowy obiekt i wpisz nowy obiekt do kolekcji tak, aby go otrzymuje szeregować do tablice bajtów dołączone do pliku lokalnego i przesyłane do replik. Po zatwierdzeniem zmiany, obiekty w pamięci, plik lokalny i wszystkie repliki były takie same dokładnie. Wszystko to dobre!

Poniższy kod właściwy sposób aktualizacji wartości w zbiorze zaufanego:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire 
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync(); 
   }
}
~~~

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Definiowanie typów danych niezmienne, aby zapobiec programisty błędu

Najlepiej, jeśli chcemy kompilatora, aby raportować błędy podczas przypadkowo utworzysz kod, który mutates stan obiektu, który ma należy rozważyć, czy niezmienne. Jednak kompilatora C# nie ma możliwości wykonaj następujące czynności. Tak, aby uniknąć potencjalne błędy programisty, zaleca się definiowania typów korzystanie z zaufanego zbiory być niezmienne typami. W szczególności oznacza to, że pokazywać podstawowych typów wartości (na przykład liczby [Int32, UInt64 itd.], DateTime, Guid, przedziału czasu i podobne). I oczywiście można też użyć ciągu. Najlepiej unikać właściwości zbioru jako szeregowania i ich deserializacji można często może obniżyć wydajność. Jednak jeśli chcesz używać właściwości zbioru zdecydowanie zalecamy użycie. Biblioteka niezmienne zbiory w sieci (System.Collections.Immutable). Ta biblioteka jest dostępna do pobrania w http://nuget.org. Zalecane jest również zamykania klas i oznaczanie pola tylko do odczytu, o ile to możliwe.

Typ informacji o użytkowniku, poniżej przedstawiono sposób definiowania typu niezmienne skorzystać z powyższych zaleceń.

~~~
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;
 
   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }
 
   [DataMember]
   public readonly String Email;
 
   // Ideally, this would be a readonly field but it can't be because OnDeserialized 
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
~~~

Typ ItemId również jest typu niezmienne, jak pokazano poniżej:

~~~
[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
~~~

## <a name="schema-versioning-upgrades"></a>Przechowywanie wersji schematu (uaktualnienie)

Wewnętrznie niezawodne zbiory szeregować obiekty przy użyciu. DataContractSerializer w sieci. Seryjnych obiekty są zachowywane na dysku lokalnym replice podstawowego i również są przekazywane do pomocniczej replik. Podczas korzystania z usługi, prawdopodobnie można zmienić typu danych (schemat) wymaga usługi. Należy rozwiązać przechowywania wersji danych za pomocą szczególną uwagę. Najpierw i wszystkim musi zawsze można zdeserializować starych danych. W szczególności, oznacza to, że kod deserializacji musi być nieskończenie zapewniającej: 333 wersji kodu usługi muszą mieć możliwość mają dotyczyć dane umieszczone w zbiorze zaufanego w wersji 1 kodu usługi 5 lat temu.

Ponadto kod usługi jest uaktualniony jedną domenę uaktualnienia naraz. Tak podczas uaktualniania, masz dwie wersje kodu usługi działających jednocześnie. Należy uniknąć nową wersję kodu usługi stare wersje kodu usługi może nie być w stanie obsługiwać nowy schemat za pomocą nowego schematu. Jeśli to możliwe, należy zaprojektować w każdej wersji usługi być zgodne z przodu w wersji 1. W szczególności oznacza to, że usługa kodu w wersji 1 powinno być możliwe po prostu zignorować elementów schematu, które wyraźnie nie obsługuje. Jednak muszą mieć możliwość zapisywanie wszelkie dane, które wyraźnie nie rozpozna i po prostu zapisu Wycofaj podczas aktualizowania wartości lub klucza słownika. 

>[AZURE.WARNING] Gdy schemat klucza można modyfikować, należy się upewnić, swój klucz skrótu i algorytmy jest równe są stałe. Jeśli zmienisz, jak działa jeden z tych algorytmów, nie można wyszukać klucz w słowniku zaufanego kiedykolwiek ponownie.
  
Można też kliknąć można wykonywać co zazwyczaj nosi nazwę uaktualnienie 2 fazy. Uaktualnianie 2 fazy, uaktualniania usługi w wersji 1 do wersji 2: w wersji 2 zawiera kod wie, w jaki na zajęcie się nowy schemat zmiany, ale nie jest wykonywana kod. Gdy kod w wersji 2 odczytuje danych w wersji 1, działa on i zapisuje dane w wersji 1. Następnie po zakończeniu uaktualniania domen wszystkie uaktualnienia można można jakiś sposób sygnału uruchomione wystąpienia w wersji 2 zakończeniu uaktualniania. (Jednym ze sposobów sygnału jest wdrożeniem uaktualnienie konfiguracji; jest to, co sprawia, że to uaktualnienie fazy 2). Teraz wystąpienia w wersji 2 może odczytać danych w wersji 1, przekonwertować ją do danych w wersji 2, go i zapisz je jako dane w wersji 2. Inne wystąpienia przeczytaniu danych w wersji 2 niepotrzebne przekonwertować go, po prostu działają nad nim, a także zapisywania danych w wersji 2.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się o tworzeniu Umowy przekazują dane zgodne, zobacz [Nowsze dane zamówień](https://msdn.microsoft.com/library/ms731083.aspx).

Aby uzyskać najlepsze rozwiązania dotyczące zamówień danych przechowywania wersji, zobacz [Przechowywanie wersji zwijania danych](https://msdn.microsoft.com/library/ms731138.aspx). 

Aby dowiedzieć się, jak wdrażać umów na uszkodzenia danych wersji, zobacz [Na uszkodzenia wersji zwrotne szeregowania](https://msdn.microsoft.com/library/ms733734.aspx). 

Aby dowiedzieć się, jak zapewniające strukturę danych, które mogą współpracować w wielu wersjach, zobacz [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
