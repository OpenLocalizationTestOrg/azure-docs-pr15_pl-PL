<properties
   pageTitle="Konfigurowanie Azure projektu przy użyciu wielu konfiguracji usługi | Microsoft Azure"
   description="Dowiedz się, jak skonfigurować projekt usługi Azure chmury, zmieniając pliki ServiceDefinition.csdef i ServiceConfiguration.cscfg."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Konfigurowanie Azure projektu przy użyciu wielu konfiguracji usługi

Projekt usługi Azure chmury zawiera dwa pliki konfiguracji: ServiceDefinition.csdef i ServiceConfiguration.cscfg. Te pliki z tej aplikacji usługi Azure chmury i wdrożony Azure.

- Plik **ServiceDefinition.csdef** zawiera metadane, które jest wymagane Azure środowisko na potrzeby aplikacji usługi cloud, tym role, jakie zawiera. Ten plik zawiera również ustawienia konfiguracji, które dotyczą wszystkich wystąpień. Te ustawienia konfiguracji mogą być odczytywane w czasie rzeczywistym przy użyciu usługi Azure hostingu API Runtime. Nie można zaktualizować ten plik jest uruchomiona usługa platformy Azure.

- Plik **ServiceConfiguration.cscfg** ustawia wartości ustawienia konfiguracji zdefiniowane w pliku definicji usługi i określa liczbę wystąpień, aby uruchomić dla poszczególnych ról. Ten plik można aktualizować jest uruchomiona usługa w chmurze platformy Azure.

Narzędzia Azure dla programu Microsoft Visual Studio zapewniają stron właściwości, które można ustawić ustawienia konfiguracji przechowywane w tych plików. Aby uzyskać dostęp do stron właściwości, kliknij dwukrotnie odwołanie roli pod projektu usługi Azure cloud w Eksploratorze rozwiązań lub kliknij prawym przyciskiem myszy odwołanie roli, a następnie wybierz polecenie **Właściwości**, jak pokazano na poniższym rysunku.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Informacje podstawowe schematów dla definicji usługi i pliki konfiguracji usługi zobacz [Schemacie](https://msdn.microsoft.com/library/azure/dd179398.aspx). Aby uzyskać więcej informacji na temat konfiguracji usługi zobacz [jak skonfigurować usług w chmurze](./cloud-services/cloud-services-how-to-configure.md).

## <a name="configuring-role-properties"></a>Konfigurowanie właściwości roli

Strony właściwości dla ról w sieci web i roli Pracownik są podobne, mimo że istnieje kilka różnic wskazano w poniższych sekcjach.

Na stronie **buforowanie** można skonfigurować Azure pamięci podręcznej usług.

### <a name="configuration-page"></a>Strona konfiguracji

Na stronie **Konfiguracja** można ustawić następujące właściwości:

**Wystąpienia**

Ustaw właściwości **wystąpienia** liczba wystąpień, które powinny być uruchomiona usługa dla tej roli.

Ustaw właściwość **rozmiar pamięci Wirtualnej** **Dodatkowe małych**, **mały**, **Średni**, **duży**lub **Bardzo duży**.  Aby uzyskać więcej informacji zobacz [rozmiarów usług w chmurze](./cloud-services/cloud-services-sizes-specs.md).

**Uruchamianie akcji** (Tylko ról w sieci web)

Ustaw dla tej właściwości, aby określić programu Visual Studio należy uruchamianie przeglądarki sieci web dla punktów końcowych HTTP lub HTTPS punkty końcowe lub oba podczas uruchamiania debugowania.

Opcja punktu końcowego HTTPS jest dostępna tylko wtedy, gdy już zdefiniowane punktu końcowego HTTPS dla roli użytkownika. Na stronie właściwości **punkty końcowe** można zdefiniować punktu końcowego HTTPS.

Jeśli zostało już dodane punktu końcowego HTTPS, jest domyślnie włączona opcja punktu końcowego HTTPS i Visual Studio powoduje uruchomienie przeglądarki dla tego punktu końcowego, podczas uruchamiania debugowania, oprócz przeglądarki dla punktu końcowego usługi HTTP. Przy założeniu, że oba opcji uruchamiania są włączone.

**Narzędzia diagnostyczne**

Narzędzia diagnostyczne domyślnie dla ról w sieci Web. Konto programu project i miejsca do magazynowania usługi Azure chmurze są skonfigurowane do korzystania z emulatora magazynu lokalnego. Jeśli możesz przystąpić do wdrożenia Azure, można wybrać przycisk Konstruktor (****...), aby zaktualizować konto przestrzeni dyskowej, aby używać Azure magazynu w chmurze. Możesz przenieść dane diagnostyki na koncie miejsca do magazynowania na żądanie lub odstępach zaplanowane automatycznie. Aby uzyskać więcej informacji o diagnostyce Azure zobacz [Włączanie diagnostyki w środowisku maszyn wirtualnych systemu i usług w chmurze Azure](./cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Ustawienia strony

Na stronie **Ustawienia** możesz dodać ustawienia konfiguracji tej usługi. Ustawienia konfiguracji są par wartości z pola Nazwa. Kod uruchomiony w roli można przeczytać wartości ustawień konfiguracji w czasie rzeczywistym przy użyciu klas dostarczony przez [Azure zarządzane biblioteki](http://go.microsoft.com/fwlink?LinkID=171026). W szczególności metoda [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) zwraca wartość nazwanego ustawienia w czasie rzeczywistym.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Konfigurowanie parametrów połączenia z kontem miejsca do magazynowania

Parametry połączenia to ustawienie konfiguracji, które zawiera informacje o połączenia i uwierzytelniania dla emulatora miejsca do magazynowania lub konta usługi Azure miejsca do magazynowania. Przy każdym kodzie musi uzyskać dostęp do danych usług Azure magazynowania — oznacza to, że obiektów blob, kolejki lub dane w tabeli — z kodu działającego w roli, należy zdefiniować parametry połączenia dla tego konta miejsca do magazynowania.

Parametry połączenia wskazującego na konto Azure miejsca do magazynowania, należy użyć zdefiniowany format. Aby uzyskać informacje dotyczące tworzenia ciągów połączeń, zobacz [Konfigurowanie parametry połączenia miejsca do magazynowania Azure](./storage/storage-configure-connection-string.md).

Gdy możesz przystąpić do testowania usługi dla usług Azure magazynu lub gdy zechcesz wdrażanie usługi w chmurze Azure, możesz zmienić wartość wszystkie ciągi połączenie, wskaż konto Azure miejsca do magazynowania. Wybierz (****...), wybierz **Wprowadź poświadczenia konta miejsca do magazynowania**. Wprowadź informacje o koncie programu zawierający swoją nazwę konta i klucz konta. W oknie dialogowym **Parametry połączenia konta miejsca do magazynowania** można również wskazać, czy chcesz użyć HTTPS domyślne punkty końcowe (opcja domyślna), punkty końcowe HTTP domyślnych lub niestandardowych punktów końcowych. Może zechcesz użyć niestandardowych punktów końcowych, jeśli zgodnie z opisem w [artykule Konfigurowanie niestandardowej nazwy domeny dla obiektów blob danych przy użyciu konta z miejsca do magazynowania Azure](./storage/storage-custom-domain-name.md)zarejestrowano niestandardowej nazwy domeny tej usługi.

>[AZURE.IMPORTANT] Należy zmodyfikować usługi parametry połączenia, wskaż konto Azure miejsca do magazynowania, przed wdrożeniem tej usługi. Nie można to zrobić może spowodować rola nie, aby rozpocząć lub aby przełączać się między stanach inicjowania, zajęty i zatrzymywanie.

## <a name="endpoints-page"></a>Strona punkty końcowe

Rola Pracownik może mieć dowolną liczbę punktów końcowych HTTP, HTTPS lub TCP. Punkty końcowe może być punkty końcowe wprowadzania danych, które są dostępne dla klientów zewnętrznych, lub wewnętrznych punkty końcowe, które są dostępne dla pozostałych ról, które są uruchomione w usłudze.

- Aby udostępnić punktu końcowego HTTP przez klientów zewnętrznych i przeglądarki sieci Web, Zmień typ punktu końcowego do wprowadzania i określ nazwę i numer portu publicznej.

- Aby udostępnić punktu końcowego HTTPS do zewnętrznych klientów i przeglądarki sieci Web, Zmień typ punktu końcowego do **wprowadzania danych**, a następnie określ nazwę, numer portu publicznej i nazwa certyfikatu zarządzania.

    Należy zauważyć, że zanim będzie można określić certyfikatu zarządzania, należy zdefiniować certyfikat na stronie właściwości **Certyfikaty** .

- Aby udostępnić punktu końcowego uzyskać dostęp do innych ról w usłudze w chmurze, Zmień typ punktu końcowego na wewnętrznych i określ nazwę i możliwych prywatne portów dla tego punktu końcowego.

## <a name="local-storage-page"></a>Lokalne przechowywanie strony

Strona właściwości **Magazynu lokalnego** umożliwia rezerwowanie jeden lub więcej zasobów magazynu lokalnego dla roli. Zasób lokalnego magazynu jest zastrzeżone katalogu w systemie plików Azure maszyny wirtualnej, w którym jest uruchomione wystąpienie roli.

## <a name="certificates-page"></a>Certyfikaty strony

Na stronie **Certyfikaty** można skojarzyć certyfikaty z roli użytkownika. Certyfikaty, które możesz dodać umożliwia konfigurowanie punktów końcowych HTTPS na stronie właściwości **punktów końcowych** .

Na stronie właściwości **certyfikatów** dodaje informacji na temat certyfikatów do konfiguracji usługi. Należy zauważyć, że certyfikaty użytkownika nie są dostarczane z usługą; własnych certyfikatów należy przekazać oddzielnie Azure za pomocą [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

Aby skojarzyć certyfikat z roli użytkownika, podaj nazwę certyfikatu. Za pomocą tej nazwy odwołują się do certyfikatu, podczas konfigurowania punktu końcowego HTTPS na stronie właściwości **punktów końcowych** . Następnie określ, czy magazyn certyfikatów jest **Komputera lokalnego** lub **Bieżącego użytkownika** i nazwa magazynu. Na koniec wprowadź odcisku palca certyfikatu. Jeśli certyfikat znajduje się w bieżącym User\Personal (My) ze sklepu, możesz wprowadzić odcisku palca certyfikatu, wybierając certyfikat z listy wypełnione. Jeśli znajduje się w innej lokalizacji, wprowadź wartość ręcznie.

Po dodaniu certyfikatu z magazynu certyfikatów wszelkie pośrednie certyfikaty są automatycznie dodawane do ustawienia konfiguracji. Aby można było poprawnie skonfigurować usługi dla protokołu SSL, również można przekazać te certyfikaty pośrednie Azure.

Jakichkolwiek certyfikatów zarządzania, które można skojarzyć z usługi dotyczą usługi tylko wtedy, gdy jest uruchomiony w chmurze. Gdy usługi jest uruchomiony w środowisku lokalnym programowania, używa to standardowy certyfikat zarządza emulatora obliczeń.

## <a name="configuring-the-azure-cloud-service-project"></a>Konfigurowanie projektu usługi Azure chmury

Aby skonfigurować ustawienia, które dotyczą projektu usługi całą chmurą Azure, trzeba najpierw otworzyć menu skrótów dla tego węzła projektu, a następnie wybierz polecenie Właściwości, aby otworzyć strony jej właściwości. W poniższej tabeli przedstawiono te właściwości strony.

|Strona właściwości|Opis|
|---|---|
|Aplikacji|Na tej stronie można wyświetlić informacje o wersji narzędzia Azure, które używa tego projektu usługi w chmurze, a można też uaktualnić do bieżącej wersji narzędzia.|
|Tworzenie wydarzenia|Na tej stronie można ustawić zdarzenia kompilacji przed i po kompilacji.|
|Opracowywania|Na tej stronie można określić instrukcje dotyczące konfiguracji kompilacji i warunki, w których są uruchamiane wszelkich zdarzeń po kompilacji.|
|W sieci Web|Na tej stronie można konfigurować ustawienia związane z serwerem sieci web.|
