<properties
   pageTitle="Omówienie kontroli dostępu w magazynie Lake danych | Microsoft Azure"
   description="Zrozumieć, jak uzyskać dostęp do formantu w magazynie Lake danych Azure"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="access-control-in-azure-data-lake-store"></a>Kontrola dostępu w magazynie Lake danych Azure

Magazyn Lake danych zawiera model kontroli dostępu, który pochodzi z HDFS oraz z kolei model kontroli dostępu POSIX. W tym artykule przedstawiono podstawowe informacje o model kontroli dostępu dla magazynu Lake danych. Aby uzyskać więcej informacji na temat HDFS kontrola dostępu do modelu zobacz [Przewodnik po uprawnienia HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Listy kontroli dostępu do plików i folderów

Istnieją dwa rodzaje dostęp control list (ACL) - **List ACL dostępu** i **ACL domyślne**.

* **ACL dostęp** — tych kontroli dostępu do obiektu. Pliki i foldery mają dostęp ACL.

* **Domyślne ACL** — "" list ACL skojarzony szablon w folderze, który Określanie dostępu do list ACL wszystkie elementy podrzędne utworzone w tym folderze. Pliki nie mają domyślnie ACL.

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Zarówno ACL programu Access, jak i domyślne ACL mieć taką samą strukturę.

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

>[AZURE.NOTE] Zmienianie domyślnej ACL element nadrzędny nie wpływa na dostępu lub domyślne ACL elementów podrzędnych, które już istnieją.

## <a name="users-and-identities"></a>Użytkownicy i tożsamości

Każdy plik i folder ma unikatowych uprawnień dla tych tożsamości:

* Posiadanie użytkownika pliku
* Właścicielem grupy
* Nazwana użytkowników
* Nazwanych grup
* Wszyscy inni użytkownicy

Tożsamości użytkowników i grup są tożsamości Azure Active Directory (AAD), więc chyba że inaczej "użytkownika", w kontekście magazynu Lake danych, może albo oznaczać AAD użytkownika lub grupę zabezpieczeń AAD.

## <a name="permissions"></a>Uprawnienia

Uprawnień do obiektu systemu plików są **Odczyt**, **Zapis**, i **Wykonywanie** i ich mogą być używane do plików i folderów, jak pokazano w poniższej tabeli.

|            |    Plik     |   Folder |
|------------|-------------|----------|
| **Read (R)** | Odczytanie zawartości pliku | Wymaga **Odczyt** i **Wykonywanie** wyświetlić zawartość folderu.|
| **Zapis (W)** | Można napisać lub dołączać do pliku | Wymaga **Zapis i wykonanie** do tworzenia podrzędne elementów w folderze. |
| **Wykonywanie (X)** | Nie oznacza niczego w kontekście magazynu Lake danych | Wymagany do przechodzenia elementów podrzędnych folderu. |

### <a name="short-forms-for-permissions"></a>Krótki formularzy uprawnień

**RWX**służy do wskazywania **odczytu + pisanie i wykonywanie**. Zmniejszoną formularza liczbowe istnieje w którym **odczytu = 4**, **Pisanie = 2**, i **Wykonywanie = 1** i ich sumy reprezentuje uprawnienia. Poniżej przedstawiono kilka przykładów.

| Formularz liczbowe | Forma krótka |      Znaczenie     |
|--------------|------------|------------------------|
| 7            | RWX        | Przeczytaj + pisanie + wykonywanie |
| 5            | R-X        | Odczytu i wykonywanie         |
| 4            | R-        | Odczyt                   |
| 0            | ---        | Brak uprawnień         |


### <a name="permissions-do-not-inherit"></a>Nie dziedziczą uprawnienia

Używane przez magazynu Lake danych modelu POSIX stylu uprawnienia dla elementu są przechowywane w samym elemencie. Innymi słowy uprawnienia dla elementu nie mogą być dziedziczone z elementów nadrzędnych.

## <a name="common-scenarios-related-to-permissions"></a>Typowe scenariusze dotyczące uprawnień

Poniżej przedstawiono kilka typowych scenariuszy, aby dowiedzieć się, jakie uprawnienia są potrzebne do wykonania określonych operacji na koncie magazynu Lake danych.

### <a name="permissions-needed-to-read-a-file"></a>Wymagane są uprawnienia do odczytu pliku

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Plik do odczytu - wywołujący musi uprawnienia do **odczytu**
* Dla wszystkich folderów w strukturze folderów, które zawierają pliku — wywołujący musi uprawnienia **wykonywania**

### <a name="permissions-needed-to-append-to-a-file"></a>Wymagane uprawnienia dołączyć do pliku

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* Plik do dołączenia - rozmówcy wymaga uprawnienia do **zapisu**
* Dla wszystkich folderów, które zawierają pliku — wywołujący musi uprawnienia **wykonywania**

### <a name="permissions-needed-to-delete-a-file"></a>Uprawnienia wymagane do usunięcia pliku

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Folder nadrzędny - wywołujący musi **napisać + wykonywanie** uprawnień
* Dla wszystkich folderów w polu Ścieżka pliku — wywołujący musi uprawnienia **wykonywania**

>[AZURE.NOTE] Pisanie, uprawnienia do pliku nie jest wymagana, aby usunąć plik, jak powyżej dwa warunki są spełnione.

### <a name="permissions-needed-to-enumerate-a-folder"></a>Uprawnienia wymagane do wyliczania folderu

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Aby wyliczyć - folderu wywołujący musi uprawnień **odczytu + Execute**
* Dla wszystkich folderów nadrzędny - wywołujący musi uprawnienia **wykonywania**

## <a name="viewing-permissions-in-the-azure-portal"></a>Wyświetlanie uprawnień w portalu Azure

Karta **Eksplorator danych** konta magazynu Lake danych kliknij **dostęp** do Zobacz list ACL pliku lub folderu. Poniżej ekranu kliknij pozycję dostęp do list ACL **wykazu** folder w obszarze konta **mydatastore** Zobacz.

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Po wykonaniu tej z karta **dostęp** , kliknij **Widok prosty** możesz wyświetlić podgląd prostsze.

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Kliknij przycisk **Widok zaawansowany** , aby wyświetlić podgląd bardziej zaawansowane.

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>Jesteś administratorem.

Jesteś administratorem uprawnieniami najbardziej wszystkich użytkowników w magazynie Lake danych. Administratorów:

* uprawnieniami RWX do **wszystkich** plików i folderów
* można zmienić uprawnienia do dowolnego pliku lub folderu.
* można zmienić właścicielem użytkownikowi lub grupie właścicielem dowolnego pliku lub folderu.

Azure konto magazynu Lake danych jest kilka Azure ról:

* Właściciele
* Współautorzy
* Czytniki
* Itd.

Każda osoba w roli **Właściciele** konta magazynu Lake danych jest automatycznie administratora dla tego konta. Aby dowiedzieć się więcej o Azure roli podstawie Access Control (RBAC) zobacz [kontroli dostępu oparta na rolach](../active-directory/role-based-access-control-configure.md).

## <a name="the-owning-user"></a>Posiadanie użytkownika

Użytkownik, który utworzył element jest automatycznie użytkownika określonego elementu. Posiadanie użytkownika wykonywać następujące czynności:

* Zmienianie uprawnień pliku, która jest właścicielem
* Zmienianie właścicielem grupy pliku, która jest właścicielem, jak właścicielem użytkownik znajduje się również członkiem grupy docelowej.

>[AZURE.NOTE] Posiadanie użytkownika **nie można** zmienić właścicielem użytkownika innego pliku własnością. Tylko użytkownicy dysponujący można zmienić określonego użytkownika, pliku lub folderu.

## <a name="the-owning-group"></a>Właścicielem grupy

W ACL POSIX każdy użytkownik jest skojarzony z "Grupa podstawowa". Na przykład użytkownika "Alicja" mogą należeć do grupy "not". Alicja mogą należeć do wielu grup, ale jedną grupę zawsze jest oznaczony jako swojej grupy podstawowej. W POSIX gdy Alicja tworzy plik właścicielem grupy ten plik jest równa swojej grupy podstawowej, czyli w tym przypadku "not".
 
Po utworzeniu nowego elementu plików magazynu Lake danych przypisuje wartość do właścicielem grupy. 

* **Przypadek 1** - folderu głównego "/". Po utworzeniu konta magazynu Lake danych jest tworzony ten folder. W tym przypadku właścicielem grupy jest ustawiona na użytkownika, który utworzył konto.
* **Przypadek 2** (co innym przypadku) - po utworzeniu nowego elementu z folderu nadrzędnego jest kopiowana właścicielem grupy.

Właścicielem grupy mogą być zmieniane przez:
* Wszyscy użytkownicy dysponujący
* Posiadanie użytkownik, jeśli właścicielem użytkownik znajduje się również członek grupy docelowej.

## <a name="access-check-algorithm"></a>Algorytm wyboru programu Access

Poniższa ilustracja przedstawia algorytmu wyboru dostępu dla kont magazynu Lake danych.

![Algorytm ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>Maski i "obowiązujących uprawnień"

**Maska** jest to wartość RWX, która jest używana ograniczyć dostęp do **nazwanych użytkowników**, **będący właścicielem grupy**i **o nazwie grupy** podczas wykonywania algorytmu kontrola dostępu do. Poniżej przedstawiono podstawowe pojęcia dla maski. 

* Maskę tworzy "obowiązujących uprawnień", oznacza to, że modyfikuje uprawnienia w momencie kontrola dostępu do.
* Maska może być edytowany bezpośrednio przez właściciela pliku i dysponujący użytkowników.
* Maski ma możliwość usuwania uprawnień do tworzenia skutecznych uprawnień. Uprawnienia maski, **nie można** dodać do aktywne uprawnienia. 

Pozwól nam Przyjrzyj się kilka przykładów. Poniżej maskę ustawiono **RWX**, co oznacza, że maskę nie powoduje usunięcia żadnych uprawnień. Zwróć uwagę, że obowiązujących uprawnień dla nazwanego użytkownika, właścicielem grupy i nazwanych grupy nie zostały zmienione podczas kontroli dostępu.

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

W poniższym przykładzie maskę ustawiono **R-X**. Tak jego sprawdzanie **powoduje wyłączenie uprawnienia do zapisu** **o nazwie użytkownika**, **będący właścicielem grupy**i **o nazwie** podczas uruchamiania programu access.

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Informacje w tym miejscu jest miejsce, w którym maskę dla pliku lub folderu jest wyświetlana w portalu Azure.

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

>[AZURE.NOTE] Utworzenie nowego konta magazynu Lake danych maskę dostępu i domyślne ACL folderu głównego ("/") są ustawiane domyślnie do RWX.

## <a name="permissions-on-new-files-and-folders"></a>Uprawnienia do nowych plików i folderów

Podczas tworzenia nowego pliku lub folderu w istniejącym folderze, określa ACL domyślny folder nadrzędny:

* Domyślne ACL folderu podrzędne i dostępu
* Plik podrzędne dostępu ACL (pliki nie mają list ACL domyślny)

### <a name="a-child-file-or-folders-access-acl"></a>Podrzędne pliku lub folderu dostępu ACL

Po utworzeniu podrzędne pliku lub folderu nadrzędnego domyślne ACL jest kopiowana jako plik podrzędny lub folderu dostępu ACL. Ponadto jeśli **inny** użytkownik nie ma uprawnień RWX domyślnej nadrzędnego ACL, jego zostanie całkowicie usunięta ze element podrzędny dostępu ACL.

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

W większości scenariuszy powyższych informacji jest potrzebny powinien wiedzieć o jak określony element podrzędny dostępu ACL. Jeśli zaznajomieni z systemami POSIX i zapoznać szczegółowo, jak osiągnąć tego transformacji, zobacz sekcję [roli osoby Umask w tworzeniu dostępu ACL dla nowych plików i folderów](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) w dalszej części tego artykułu.
 

### <a name="a-child-folders-default-acl"></a>ACL domyślny folder podrzędny

Po utworzeniu folderu podrzędnego w folderze nadrzędnej ACL domyślny folder nadrzędny jest kopiowana, ponieważ jest, aby dodać domyślny folder podrzędny do.

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Zagadnienia zaawansowane opis ACL w magazynie Lake danych

Poniżej przedstawiono kilka zagadnienia zaawansowane, aby lepiej zrozumieć, jak ACL są określone dla danych Lake magazyn plików lub folderów.

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>Rola w Umask w tworzenie dostępu ACL dla nowych plików i folderów

W układzie zgodne z POSIX ogólnej koncepcji jest tego umask 9-bitową wartością folderu nadrzędnego umożliwia przekształcanie uprawnienia dla **użytkownik będący właścicielem**, **będący właścicielem grupy**i **innych** na nowy plik podrzędny lub folderu dostępu ACL. Bity umask identyfikowanie które bity, aby wyłączyć funkcję w element podrzędny dostępu ACL. Dlatego jest stosowane w celu zapobiegnięcia selektywne propagowanie uprawnień użytkownik będący właścicielem grupy, będący właścicielem i inne.
  
W systemie plików HDFS umask zazwyczaj jest opcja konfiguracji całej witryny, która jest kontrolowane przez administratorów. Magazyn Lake danych użyto **umask całej konta** , którego nie można zmieniać. W poniższej tabeli przedstawiono umask magazynu Lake danych.

| Grupy użytkowników  | Ustawienie | Wpływ na nowy element podrzędny dostępu ACL |
|------------ |---------|---------------------------------------|
| Użytkownik będący właścicielem | ---     | Nie działa                             |
| Posiadanie grupy| ---     | Nie działa                             |
| Inne       | RWX     | Usuwanie odczytu + pisanie + wykonywanie         | 

Na poniższej ilustracji przedstawiono ten umask w działaniu. Efekt netto jest usunięcie **odczytu + pisanie i wykonywanie** przez **innego** użytkownika. Ponieważ umask nie określi bitów **użytkownik będący właścicielem** i **właścicielem grupy**, te uprawnienia nie są przekształcane.

![ACL magazynu Lake danych](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### <a name="the-sticky-bit"></a>Trwałe bitowa

Bit lepkich jest bardziej zaawansowanych funkcji systemu plików POSIX. W kontekście magazynu Lake danych prawdopodobnie będą potrzebne lepkich bitową.

W poniższej tabeli pokazano, jak lepkich bitowej działa w magazynie Lake danych.

| Grupy użytkowników         | Plik    | Folder |
|--------------------|---------|-------------------------|
| Trwałe bitowa **Wył.** | Nie działa   | Nie działa           |
| Trwałe bitowa **Dalej**  | Nie działa   | Każda osoba z wyjątkiem **dysponujący użytkowników** i **użytkownik będący właścicielem** elementu podrzędnego z usunięcie lub zmianę nazwy tego elementu podrzędnego zapobiega.               |

Trwałe bitowa nie jest wyświetlana w portalu Azure.

## <a name="common-questions-for-acls-in-data-lake-store"></a>Często zadawane pytania dla list ACL w magazynie Lake danych

Poniżej przedstawiono niektóre pytania, które pojawiły się często w odniesieniu do list ACL w magazynie Lake danych.

### <a name="do-i-have-to-enable-support-for-acls"></a>Czy muszę włączyć obsługę ACL?

Wartość nie. Kontrola dostępu za pomocą list ACL jest zawsze włączone dla konta magazynu Lake danych.

### <a name="what-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Jakie uprawnienia są wymagane do lokalizacji Usuń folder i jego zawartość?

* Folder nadrzędny musi mieć **Pisanie + wykonywanie**.
* Folder zostanie usunięta i każdy folder, to wymaga **więcej + pisanie + wykonywanie**.
>[AZURE.NOTE] Usuwanie plików w folderach nie wymaga zapisu do tych plików. Ponadto folderu głównego "/" **nigdy nie** można usunąć.

### <a name="who-is-set-as-the-owner-of-a-file-or-folder"></a>Kto jest ustawiona jako właściciel pliku lub folderu?

Kreator pliku lub folderu staje się właścicielem.

### <a name="who-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Kto jest ustawiona jako grupa właścicielem pliku lub folderu podczas tworzenia?

Jest ona kopiowana z właścicielem grupy folder nadrzędny, w którym jest tworzony nowy plik lub folder.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Jestem właścicielem użytkownika pliku, ale nie mam potrzebnych uprawnień RWX. Co należy zrobić?

Posiadanie użytkownik może po prostu zmienić uprawnienia do tego pliku do udzielenia sobie żadnych RWX uprawnień, które są potrzebne.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Magazyn Lake danych obsługuje dziedziczenie ACL?

Wartość nie.

### <a name="what-is-the-difference-between-mask-and-umask"></a>Jaka jest różnica między maski i umask?

| Maska | umask|
|------|------|
| Właściwość **Maska** jest dostępna na wszystkich plików i folderów. | **Umask** jest właściwością konta magazynu Lake danych. Tak istnieje tylko jednego umask w magazynie Lake danych.    |
| Właściwość maska pliku lub folderu może się zmienić w posiadanie użytkownikowi lub grupie właścicielem pliku lub administratora. | Nie można zmodyfikować właściwości umask przez dowolnego użytkownika, a nawet administratorów. Jest zmieniać, stałą wartość.|
| Właściwość maska służy do podczas kontrola dostępu do algorytmu w czasie rzeczywistym, aby określić, czy użytkownik ma prawo do wykonywania operacji na pliku lub folderu. Roli maski jest utworzenie "obowiązujących uprawnień" w czasie kontroli dostępu. | Umask nie jest używana podczas kontrola dostępu do aplikacji. Umask służy do określania dostępu ACL nowych elementów podrzędnych folderu. |
| Maska jest wartość RWX 3-bitową, która ma zastosowanie do nazwanych użytkowników i grup nazwanych oraz określonego użytkownika w czasie kontroli dostępu.| Umask jest wartością 9 bitowej, która ma zastosowanie do określonego użytkownika, właścicielem grupy i inne nowy element podrzędny.| 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Gdzie można dowiedzieć się więcej na temat POSIX model kontroli dostępu?

* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.HTML](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [HDFS podręczniku uprawnień](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 

* [POSIX — CZĘSTO ZADAWANE PYTANIA](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1e 1997](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)

* [POSIX ACL Linux](http://users.suse.com/~agruen/acl/linux-acls/online/)

* [List ACL przy użyciu listy kontroli dostępu w systemie Linux](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Zobacz też

* [Omówienie magazynu Lake danych Azure](data-lake-store-overview.md)

* [Wprowadzenie do analizy Lake Azure danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)





