<properties
   pageTitle="Uruchamianie dowolnej aplikacji dla systemu Windows na dowolnym urządzeniu z Azure RemoteApp | Microsoft Azure"
   description="Dowiedz się, jak udostępniać użytkownikom dowolnej aplikacji dla systemu Windows przy użyciu Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Uruchamianie dowolnej aplikacji dla systemu Windows na dowolnym urządzeniu z Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Można uruchomić aplikacji systemu Windows w dowolnym miejscu na dowolnym urządzeniu obecnie poważnie — tylko przy użyciu Azure RemoteApp. Czy jest niestandardowa aplikacja napisane 10 lat temu lub aplikacji pakietu Office, użytkownicy już być związana określonego systemu operacyjnego (na przykład Windows XP) tych kilka aplikacji.

Z Azure RemoteApp użytkowników można używać własnych urządzeń Android lub firmy Apple i się tym samym środowisko użytkownika, które były masz w systemie Windows (lub na telefonach Windows). W tym celu hostingu aplikacji systemu Windows w zbiorze maszyn wirtualnych systemu Windows Azure — użytkownicy mogą uzyskiwać do nich dostęp z dowolnego miejsca mają połączenie z Internetem. 

Przeczytaj na przykład dokładnie tak, jak to zrobić.

W tym artykule firma Microsoft zacząć korzystać ze wszystkimi naszych użytkowników. Jednak można użyć dowolnej aplikacji. Jak długo aplikacji można zainstalować na komputerze z systemem Windows Server 2012 R2, możesz udostępnić go, wykonując poniższe kroki. [Wymagania dotyczące aplikacji](remoteapp-appreqs.md) , aby upewnić się, że działa aplikacji można przeglądać.

Należy zauważyć, że ponieważ jest bazą danych programu Access, a chcemy tej bazy danych, aby być przydatne, będzie można robimy kilka dodatkowych kroków, aby umożliwić użytkownikom udostępnianie danych programu Access. Jeśli aplikacji nie ma bazy danych lub nie ma potrzeby użytkowników, aby mieć możliwość dostępu do udziału plików, możesz pominąć tych kroków w tym samouczku

> [AZURE.NOTE] <a name="note"></a>Potrzebne jest konto Azure do użycia tego samouczka:
> - Możesz [otworzyć konto Azure bezpłatnie](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): uzyskiwanie środków Umożliwia wypróbowanie płatnych usług Azure, a nawet w przypadku, gdy są używane nawet przechowujesz konta i użyj wolny Azure usług, takich jak witryny sieci Web. Karta kredytowa nigdy nie zostanie obciążona, chyba że jawnie Zmienianie ustawień i poproś o naliczane.
> - Można [aktywować korzyści subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): subskrypcji MSDN i zapewnia środków co miesiąc, używanej usługi Azure płatnej.


## <a name="create-a-collection-in-remoteapp"></a>Tworzenie zbioru w RemoteApp

Zacznij od tworzenie kolekcji. Kolekcja służy jako kontenera dla użytkowników i aplikacjami. Każdego zbioru jest oparty na obrazie — możesz utworzyć własny lub dostarczany z subskrypcją. Ten samouczek użyto programu obraz wersji próbnej pakietu Office 2013 — zawiera aplikacji, którą chcemy udostępniać.

1. W portalu usługi Azure przewiń w dół drzewa nawigacji po lewej do momentu wyświetlenia RemoteApp. Otwórz tę stronę.
2. Kliknij przycisk **Utwórz zbiór RemoteApp**.
3. Kliknij przycisk **Szybkie tworzenie** i wprowadź nazwę kolekcji.
4. Wybierz region, który ma być używany do tworzenia kolekcji. Aby uzyskać najlepsze wyniki wybierz żądany region najbliżej geograficznie do lokalizacji, w której użytkownicy będą uzyskiwać dostęp aplikacji. Na przykład w tym samouczku użytkownicy będą znajdować się w Redmond, Washington. Najbliższy region Azure jest **Zachód USA**.
5. Wybierz plan rozliczeń, do którego chcesz użyć. Podstawowy plan rozliczeń umieszcza 16 użytkowników dużych Głosowa Azure, podczas standardowej plan rozliczeń ma 10 użytkowników w dużych maszyny Azure. Jako przykład ogólne podstawowy plan działa świetnie przepływu pracy typ wpisu danych. Dla aplikacji wydajności, na przykład pakiet Office warto plan standardowy.
6. Na koniec wybierz obraz pakietu Office Professional 2013. Ten obraz zawiera aplikacje pakietu Office 2013. Po prostu przypomnienia — ten obraz jest tylko dobre dla wersji próbnej zbiorów i POCs. Możesz "nie można używać tego obrazu w zbiorze produkcji.
7. Teraz kliknij pozycję **Utwórz RemoteApp zbiór**.

![Tworzenie zbioru cloud w RemoteApp](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Zostanie uruchomiony, tworzenie kolekcji, ale może potrwać do godziny.

Teraz możesz przystąpić do dodawania użytkowników.

## <a name="share-the-app-with-users"></a>Udostępnianie aplikacji użytkownikom

Po utworzeniu pomyślnie kolekcji, nadszedł czas na publikowanie dostęp do użytkowników i dodać użytkowników, którzy powinni mieć do nich dostęp.

Po przejściu poza węzeł Azure RemoteApp podczas tworzenia kolekcji, zacznij od upewnienia jak do niego z powrotem na stronie głównej Azure.

2. Kliknij pozycję zbiór została utworzona wcześniejszych, aby uzyskać dostęp do dodatkowych opcji i skonfigurować kolekcji.
![Nowy zbiór chmury RemoteApp](./media/remoteapp-anyapp/ra-anyappcollection.png)
3. Na karcie **Publikowanie** kliknij pozycję **Publikuj** w dolnej części ekranu, a następnie kliknij **Programy menu Start publikowanie**.
![Publikowanie programu trybu RemoteApp](./media/remoteapp-anyapp/ra-anyapppublish.png)
4. Wybierz aplikacje, które chcesz opublikować z listy. W celu naszych Wybraliśmy programu Access. Kliknij pozycję **pełne**. Poczekaj na zakończenie publikowania aplikacji.
![Publikowania programu Access w RemoteApp](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)


1. Po aplikacji zakończył publikowanie, głowy na kartę **Dostępu użytkowników** , aby dodać wszystkich użytkowników, którzy muszą mieć dostęp do aplikacji. Wprowadź nazwy użytkownika (adres e-mail) dla użytkowników, a następnie kliknij przycisk **Zapisz**.

![Dodawanie użytkowników do programów RemoteApp](./media/remoteapp-anyapp/ra-anyappaddusers.png)


1. Teraz nadszedł czas na informowanie użytkowników o tych nowych aplikacji i sposobu uzyskiwać do nich dostęp. W tym celu należy wysyłać użytkownikom wiadomości e-mail wskazaniu adres URL plik do pobrania klient pulpitu zdalnego.
![Klient pobrać adresu URL dla RemoteApp](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-to-access"></a>Konfigurowanie dostępu do programu Access

Niektóre aplikacje wymagana jest dodatkowa konfiguracja po wdrożeniu przy użyciu funkcji RemoteApp. W szczególności dostępu, możemy zacząć tworzyć Azure, że każdy użytkownik może uzyskać dostęp do udziału plików. (Jeśli nie chcesz, aby to zrobić, możesz utworzyć [zbioru hybrydowych](remoteapp-create-hybrid-deployment.md) [zamiast naszych zbioru chmury] umożliwia użytkownikom dostęp do plików i informacji w sieci lokalnej.) Następnie możemy musisz określić użytkowników do zamapować dysk lokalny na komputerze w systemie plików Azure.

Pierwsza część wykonywane jako administrator. Następnie mamy pewne czynności dla użytkowników.

1. Zacznij od publikowania interfejsu wiersza polecenia (cmd.exe). Na karcie **publikowania** , zaznacz **cmd**, a następnie kliknij **Publikuj > Publikuj programu przy użyciu ścieżki**.
2. Wprowadź nazwę aplikacji i ścieżkę. W celu naszych za pomocą "Eksplorator plików", a nazwa "% SYSTEMDRIVE%\windows\explorer.exe" jako ścieżkę katalogu.
![Publikowanie pliku cmd.exe.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Następnie należy utworzyć Azure [konta miejsca do magazynowania](../storage/storage-create-storage-account.md). Firma Microsoft o nazwie nasza "accessstorage", aby wybrać nazwę opisową. (Do misquote Highlander, może istnieć tylko jeden "accessstorage.") ![Nasze Azure miejsca do magazynowania konta](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Teraz wróć do pulpitu nawigacyjnego aby ścieżkę uzyskiwać dostęp do Twojej przestrzeni dyskowej (lokalizacja punktu końcowego). Będzie to za pomocą w nieco, dlatego upewnij się, że możesz skopiować innym.
![Ścieżka konta miejsca do magazynowania](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Następnie po utworzeniu konta miejsca do magazynowania, należy klucz podstawowy dostęp. Kliknij pozycję **Zarządzaj klawisze dostępu**, a następnie skopiuj klucz podstawowy dostęp.
6. Teraz Ustaw kontekście konta miejsca do magazynowania i tworzenie nowego pliku w udziale plików dla programu Access. Uruchom następujące polecenia cmdlet w pełnych okna programu Windows PowerShell:

        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx

    Aby dla naszych udziału są polecenia cmdlet, które możemy uruchomić:

        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx


Teraz jest włączanie użytkownika. Przede wszystkim Poproś użytkowników instalowanie [klienta RemoteApp](remoteapp-clients.md). Następnie użytkownicy potrzebują do zamapować dysk z kont do tego udziału pliku Azure, utworzony i Dodaj swoje pliki programu Access. Oto jak wykonaniu tej czynności:

1. W kliencie RemoteApp programu access opublikowanych aplikacji. Uruchom cmd.exe program.
2. Uruchom następujące polecenie, aby zamapować dysk z komputera do udziału plików:

        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>

    Jeśli parametr **/ persistent** tak, zamapowany dysk będzie umieszczony między sesjami.
1. Teraz uruchom aplikację Eksploratora plików z RemoteApp. Skopiuj wszystkie pliki programu Access, który ma być używany w aplikacji udostępnionych do udziału plików.
![Wysyłanie plików programu Access w udziale Azure](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
1. Na koniec Otwórz program Access, a następnie otwórz bazę danych, którą udostępnionego. Powinien zostać wyświetlony danych w programie Access uruchomionym z chmury.
![Liczba rzeczywista bazy danych programu Access uruchomione z chmury](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Teraz możesz wykorzystać dostęp na wszystkich urządzeniach — po prostu upewnij się, że zainstalować klienta RemoteApp.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

Teraz, gdy już format zarządzany tworzenie kolekcji, spróbuj tworzenie [kolekcji, która korzysta z usługi Office 365](remoteapp-tutorial-o365anywhere.md). Lub można utworzyć [zbiór hybrydowe ](remoteapp-create-hybrid-deployment.md), który można uzyskać dostęp do sieci lokalnej.

<!--Image references-->
 
