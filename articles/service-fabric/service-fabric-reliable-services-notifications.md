<properties
   pageTitle="Niezawodne powiadomienia usługi | Microsoft Azure"
   description="Koncepcyjna dokumentacji usługa tkaninie niezawodne usługi powiadomień"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="reliable-services-notifications"></a>Niezawodne powiadomienia usług

Powiadomienia Zezwalaj klientom śledzenia zmian, wprowadzone do obiektu, który Interesujesz się. Dwa typy obiektów obsługuje powiadomienia: *Zaufanego Menedżer stanu* i *Zaufanego słownika*.

Najczęstsze przyczyny za pomocą powiadomień są:

- Budynek materialized widoki, takie jak indeksów pomocniczych lub agregacją filtrowane widoki stan replice. Przykładem jest indeks posortowany klawisze w słowniku zaufanego.
- Wysyłanie monitorowania danych, takich jak liczba użytkowników dodane w ostatnim godziny.

Powiadomienia są uruchamiane w ramach stosowania operacji. Ze względu na to, że powiadomienia powinny być traktowane tak szybko, jak to możliwe i synchroniczne zdarzenia nie zawierają wszystkie operacje drogich.

## <a name="reliable-state-manager-notifications"></a>Stan niezawodny Menedżer powiadomień

Menedżer stanu niezawodny przekazuje powiadomienia dla następujących zdarzeń:

- Transakcji
    - Zatwierdź
- Menedżer stanu
    - Odbudowywanie
    - Dodawanie zaufanego stanów
    - Usuwanie zaufanego stanów

Menedżer stanu niezawodny śledzi bieżące transakcje locie zmiany. Tylko zmiana w stanie transakcji, które powoduje wyświetlanie powiadomień być uruchamiane jest transakcji jest projekt zatwierdzony.

Menedżer stanu niezawodny zapisuje zbiór niezawodne Zjednoczone, takich jak słownik niezawodne i niezawodne kolejki. Menedżer stanu niezawodny uruchamiana powiadomienia o zmianie tego zbioru: stan zaufanego zostanie dodany bądź usunięty lub całą kolekcję zostanie odtworzony.
W zbiorze zaufanego Menedżer stanu zostanie odtworzony w trzech przypadkach:

- Odzyskiwanie: Po uruchomieniu replice odzyskuje poprzedniego stanu z dysku. Na końcu odzyskiwania używa **NotifyStateManagerChangedEventArgs** uruchomienie zdarzenie, które zawiera zestaw odzyskanego Państw zaufanego.
- Pełna kopia: przed replice mogą dołączyć do konfiguracji, musi być wbudowane. Czasami wymaga to pełna kopia stan zaufanego Menedżer stanu z zestawu podstawowego mają być stosowane jako bezczynny replice pomocniczą. Menedżer stanu niezawodny w replice pomocniczej używa **NotifyStateManagerChangedEventArgs** zdarzenia, która zawiera zestaw niezawodnych stanów, które go nabyte z podstawowego zestawu.
- Przywracanie: W scenariuszy odzyskiwania danych, w replice stan można przywrócić z kopii zapasowej za pośrednictwem **RestoreAsync**. W takich przypadkach zaufanego Menedżer stanu, w replice podstawowego używa **NotifyStateManagerChangedEventArgs** zdarzenia, która zawiera zestaw niezawodnych stanów, które go przywrócić z kopii zapasowej.

Aby zarejestrować dla transakcji powiadomienia i/lub powiadomienia Menedżer stanu, musisz zarejestrować z wydarzeniami **TransactionChanged** lub **StateManagerChanged** zaufanego Menedżer stanu. Typowe lokalizację, aby zarejestrować w tych obsługi zdarzeń jest konstruktora akumulującej stan usługi. Podczas rejestrowania w konstruktorze nie przeoczyć powodowany przez zmianę w czasie trwania **IReliableStateManager**powiadomienia.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Program obsługi zdarzeń **TransactionChanged** używa **NotifyTransactionChangedEventArgs** o podanie szczegółowych informacji dotyczących zdarzenia. Zawiera on właściwość akcji (na przykład **NotifyTransactionChangedAction.Commit**), która określa typ zmiany. Zawiera on również właściwość transakcji, która zapewnia odwołanie transakcji zmienione.

>[AZURE.NOTE] Obecnie **TransactionChanged** zdarzenia pojawiają się tylko wtedy, gdy transakcja zostanie zatwierdzony. Akcja jest równa **NotifyTransactionChangedAction.Commit**. Jednak w przyszłości, zdarzeń może wzrosnąć dla pozostałych typów zmian stanu transakcji. Zalecamy sprawdzanie akcji i przetwarzania zdarzenie tylko wtedy, gdy jest to jeden, której oczekujesz.

Oto przykład **TransactionChanged** zdarzeń.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

Program obsługi zdarzeń **StateManagerChanged** używa **NotifyStateManagerChangedEventArgs** o podanie szczegółowych informacji dotyczących zdarzenia.
**NotifyStateManagerChangedEventArgs** występują dwa podklas: **NotifyStateManagerRebuildEventArgs** i **NotifyStateManagerSingleEntityChangedEventArgs**.
Za pomocą właściwości akcji w **NotifyStateManagerChangedEventArgs** rzutowane **NotifyStateManagerChangedEventArgs** na poprawny klasy:

- **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
- **NotifyStateManagerChangedAction.Add** i **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Oto przykład **StateManagerChanged** powiadomień obsługi.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Niezawodne powiadomienia słownika

Słownik niezawodne przekazuje powiadomienia dla następujących zdarzeń:

- Odbuduj: Wywoływane, gdy **ReliableDictionary** ma odzyskać jego stanu z odzyskanego lub skopiowanego lokalne lub wykonywanie kopii zapasowych.
- Wyczyść: Wywoływane, gdy stan **ReliableDictionary** zostało wyczyszczone metodą **ClearAsync** .
- Dodawanie: Wywoływane, gdy element został dodany do **ReliableDictionary**.
- Aktualizacja: Nazywane po zaktualizowaniu elementu **IReliableDictionary** .
- Usuń: Nazywane po usunięciu elementu **IReliableDictionary** .

Aby otrzymywać powiadomienia zaufanego słownika, musisz zarejestrować obsługi zdarzeń **DictionaryChanged** na **IReliableDictionary**. Typowe lokalizację, aby zarejestrować w tych obsługi zdarzeń znajduje się w **ReliableStateManager.StateManagerChanged** Dodawanie powiadomienie.
Rejestrowanie po dodaniu **IReliableDictionary** **IReliableStateManager** gwarantuje, że nie przeoczyć powiadomienia.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] **ProcessStateManagerSingleEntityNotification** jest to metoda przykładowych połączeń z powyższego przykładu **OnStateManagerChangedHandler** .

Poprzedni kod ustawia interfejs **IReliableNotificationAsyncCallback** , wraz z **DictionaryChanged**. Powodujący **NotifyDictionaryRebuildEventArgs** interfejsu **IAsyncEnumerable** — które trzeba można wyliczyć asynchroniczne — odbudowanie powiadomienia są uruchamiane za pośrednictwem **RebuildNotificationAsyncCallback** zamiast **OnDictionaryChangedHandler**.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] W poprzednim kodzie w ramach przetwarzania powiadomień Odbuduj najpierw obsługiwanych stan zagregowane jest wyczyszczone. Ponieważ niezawodne zbioru jest zostanie odbudowany z nowym Państwie, wszystkie poprzedniego powiadomienia są one zbędne.

Program obsługi zdarzeń **DictionaryChanged** używa **NotifyDictionaryChangedEventArgs** o podanie szczegółowych informacji dotyczących zdarzenia.
**NotifyDictionaryChangedEventArgs** zawiera pięć podklas. Aby rzutowane **NotifyDictionaryChangedEventArgs** na poprawny klasy, należy użyć właściwości akcji w **NotifyDictionaryChangedEventArgs** :

- **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
- **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
- **NotifyDictionaryChangedAction.Add** i **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
- **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
- **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Zalecenia

- *Czy* wykonywanie zdarzeń powiadamiania tak szybko, jak to możliwe.
- *Nie* wykonywać wszystkie operacje drogich (na przykład operacji We/Wy) jako część zdarzenia synchroniczne.
- Przed przetwarzanie zdarzenia, *należy* sprawdzić typ akcji. Nowe typy akcji mogą być dodane w przyszłości.

Oto kilka rzeczy, których warto pamiętać:

- Powiadomienia są uruchamiane w ramach wykonywania operacji. Na przykład powiadomienie przywracania jest uruchamiany jako ostatni krok operacji przywracania. Przywracanie nie zostanie zakończony, dopóki nie jest przetwarzana zdarzenia powiadomienia.
- Ponieważ powiadomienia są uruchamiane w ramach operacji stosowania, klienci Zobacz tylko powiadomienia dla operacji lokalnie zatwierdzony. Ponieważ operacje zapewniające tylko lokalnie zapewniane (innymi słowy, rejestrowane), mogą być lub może nie są zapisywane w przyszłości.
- Na ścieżce Powtórz pojedyncze powiadomienia jest uruchamiany dla każdego zastosowanego operacji. Oznacza to, że jeśli transakcji T1 zawiera Create(X), Delete(X) i Create(X), zostanie wyświetlony jeden powiadomienie o utworzenie X, jedną do usunięcia, a drugi do utworzenia ponownie, w tej kolejności.
- Transakcji zawierających wiele operacji operacji są stosowane w kolejności, otrzymano w replice podstawowego przez użytkownika.
- W ramach przetwarzania postępu FAŁSZ niektóre operacje mogą można cofnąć. Powiadomienia jest uruchamiany dla takich działań Cofnij, wycofywanie stanu replice do punktu stabilny. Jedna różnica ważne powiadomień Cofnij jest łączy się zdarzenia, które ma zduplikowane klawiszy. Na przykład jeśli jest Trwa wycofywanie transakcji T1, pojawi się pojedynczego powiadomienie do Delete(X).

## <a name="next-steps"></a>Następne kroki

- [Niezawodne zbiory](service-fabric-work-with-reliable-collections.md)
- [Niezawodne usług szybki start](service-fabric-reliable-services-quick-start.md)
- [Wykonywanie kopii zapasowej niezawodne usług i ich przywracanie (odzyskiwanie)](service-fabric-reliable-services-backup-restore.md)
- [Dokumentacja dewelopera niezawodne zbierania danych](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
