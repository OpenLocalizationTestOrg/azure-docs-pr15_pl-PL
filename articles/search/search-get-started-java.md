<properties
    pageTitle="Wprowadzenie do wyszukiwania Azure w Java | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Sposoby tworzenia aplikacji wyszukiwania hostowanej chmura azure za pomocą języka Java językiem programowania."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-java"></a>Wprowadzenie do wyszukiwania Azure w Java
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Dowiedz się, jak utworzyć niestandardową aplikację wyszukiwania Java, która używa Azure wyszukiwanie jej wyników wyszukiwania. Ten samouczek wykorzystuje [Interfejsu API usługi REST Usługa wyszukiwania Azure](https://msdn.microsoft.com/library/dn798935.aspx) do utworzenia obiektów i używane w tym wykonywania operacji.

Aby uruchomić ten przykład, musisz mieć usługi Azure wyszukiwania, w której możesz się zalogować do usługi [Azure Portal](https://portal.azure.com). Aby uzyskać instrukcje krok po kroku, zobacz temat [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) .

Tworzenie i przetestować w tym przykładzie użyliśmy następującym oprogramowaniem:

- [Zaćmienie IDE dla deweloperów Estonia Java](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Pamiętaj pobrać wersję Estonia. Jeden z kroków weryfikacji wymaga funkcji, która znajduje się tylko w tej wersji.

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>Informacje o danych

Ta aplikacja przykładowa korzysta z danych z [Stanów Zjednoczonych geologicznych usług (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtrowane w stanie Wyspy Rhode zmniejszyć rozmiar zestawu danych. Użyjemy te dane do tworzenia aplikacji wyszukiwania, która zwraca budynki doniosłej, takie jak szpitale i szkoły, a także geologicznych funkcji, takich jak strumienie, lak i kolejnych konferencji.

W tej aplikacji **SearchServlet.java** program tworzy i załadowaniu indeksu przy użyciu konstrukcji [indeksatora](https://msdn.microsoft.com/library/azure/dn798918.aspx) pobierania filtrowanego zestawu danych USGS z publicznej bazy danych SQL Azure. Wstępnie zdefiniowane poświadczeń i informacje o połączeniu ze źródłem danych znajdują się w kodu programu. Pod względem dostęp do danych należy nie dalszej konfiguracji.

> [AZURE.NOTE] W tym zestawie danych do utrzymywania w obszarze limit 10 000 dokumentów bezpłatne poziomu cen możemy zastosowano filtr. Użycie warstwie standardowy, ten limit nie ma zastosowania, a można modyfikować tego kodu w celu użycia większy zestaw danych. Aby uzyskać szczegółowe informacje o pojemności dla każdego poziomu cen zobacz [limity i ograniczeń](search-limits-quotas-capacity.md).

## <a name="about-the-program-files"></a>Informacje o plikach programu

Na poniższej liście przedstawiono pliki, które dotyczą w tym przykładzie.

- Search.JSP: Udostępnia interfejs użytkownika
- SearchServlet.java: Oferuje metody (podobnie do kontrolera w MVC)
- SearchServiceClient.java: Żądania HTTP uchwyty
- SearchServiceHelper.java: Klasa pomocy zawierającego metody statyczne
- Document.Java: Zawiera modelu danych
- config.Properties: ustawia adres URL usługi wyszukiwania i klucz interfejsu api
- Pom.XML: Zależność środowiska Maven

<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Znajdź nazwę usługi i klucz interfejsu api usługi Azure wyszukiwania

Wszystkie wywołania interfejsu API usługi REST Azure wyszukiwania wymaga, aby podać adres URL usługi i klucz interfejsu api. 

1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. Na pasku szybkiego dostępu kliknij **Usługa wyszukiwania** , aby wyświetlić listę wszystkich usług wyszukiwania Azure zainicjowany dla subskrypcji.
3. Wybierz usługę, której chcesz użyć.
4. Na pulpicie nawigacyjnym usługi pojawi się Kafelki ważnymi informacjami, a także ikona klucza dostępu do kluczy administratora.

    ![][3]

5. Skopiuj adres URL usługi i klucz administratora. Należy je później, po dodaniu ich do pliku **config.properties** .

## <a name="download-the-sample-files"></a>Pobieranie przykładowych plików

1. Przejdź do [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) w Github.

2. Kliknij przycisk **Pobierz ZIP**, Zapisz plik zip na dysku, a następnie wyodrębnić wszystkie pliki, które zawiera. Należy rozważyć, czy wyodrębnianie plików do obszaru roboczego Java, aby ułatwić znajdowanie projektu w przyszłości.

3. Przykładowe pliki są tylko do odczytu. Kliknij prawym przyciskiem myszy właściwości folderu, a następnie wyczyść atrybut tylko do odczytu.

Wszystkie modyfikacje kolejnych plików i wykonywania instrukcji zostaną wprowadzone przed pliki w tym folderze.  

## <a name="import-project"></a>Importowanie projektu

1. W Zaćmienie, wybierz **plik** > **importowania** > **Ogólne** > **Istniejących projektów do obszaru roboczego**.

    ![][4]

2. **Wybierz pozycję katalogu**przejdź do folderu zawierającego przykładowe pliki. Wybierz folder, w którym znajduje się .project folder. Projekt powinien być wyświetlany na liście **projektów** co zaznaczony element.

    ![][12]

3. Kliknij przycisk **Zakończ**.

4. **Eksplorator projektu** umożliwia wyświetlanie i edytowanie plików. Jeśli nie jest jeszcze otwarte, kliknij przycisk **okno** > **Pokaż widok** > **Eksplorator projektu** lub użyj skrótu, aby go otworzyć.

## <a name="configure-the-service-url-and-api-key"></a>Skonfigurować adres URL usługi i klucz interfejsu api

1. W **Programie Project Explorer**kliknij dwukrotnie **config.properties** , aby zmienić ustawienia konfiguracji zawierającą nazwę serwera i klucz interfejsu api.

2. Skorzystaj z procedury opisanej w tym artykule, gdzie znaleźć adres URL usługi i klucz interfejsu api w [Azure Portal](https://portal.azure.com), aby uzyskać wartości, które można teraz rozpoczną **config.properties**.

3. W **config.properties**Zamień "Klucz interfejsu Api" klucz interfejsu api tej usługi. Następnie nazwa usługi (pierwszy składnik http://servicename.search.windows.net adres URL) zastępuje "Nazwa usługi" w jednym pliku.

    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Konfigurowanie środowiska projektu, tworzenie i środowisko uruchomieniowe

1. W Zaćmienie w oknie Project Explorer, kliknij prawym przyciskiem myszy Projekt > **Właściwości** > **Aspekty projektu**.

2. Wybierz **moduł dynamiczne sieci Web**, **Java**i **JavaScript**.

    ![][6]

3. Kliknij przycisk **Zastosuj**.

4. **Okno**wybieranie > **Preferencje** > **serwera** > **środowiskach środowisko uruchomieniowe** > **Dodaj.**.

5. Rozwiń Apache i wybierz wersję na serwerze Apache Tomcat, który został wcześniej zainstalowany. W systemie możemy zainstalowana wersja 8.

    ![][7]

6. Na następnej stronie Określ Tomcat katalog instalacji. Na komputerze z systemem Windows to będzie najprawdopodobniej C:\Program Files\Apache oprogramowania Foundation\Tomcat *wersji*.

6. Kliknij przycisk **Zakończ**.

7. **Okno**wybieranie > **Preferencje** > **Java** > **zainstalowany JREs** > **dodać**.

8. W **Dodawanie JRE**wybierz **Standardowy maszyn wirtualnych**.

10. Kliknij przycisk **Dalej**.

11. W definicji JRE, w JRE Narzędzia główne kliknij pozycję **katalogu**.

12. Przejdź do **Plików programów** > **Java** i wybierz pozycję JDK został wcześniej zainstalowany. Należy wybrać JDK jako środowiska JRE.

13. Wybieranie **JDK**w zainstalowanej JREs. Ustawienia powinna wyglądać podobnie do następujących zrzut ekranu.

    ![][9]

14. Opcjonalnie wybierz **okno** > **Przeglądarki sieci Web** > **Programu Internet Explorer** , aby otworzyć aplikację w oknie przeglądarki zewnętrznych. Za pomocą przeglądarki zewnętrznych zapewnia lepsze możliwości aplikacji sieci Web.

    ![][8]

Zadania konfiguracji zostało zakończone. Dalej można utworzyć i uruchomić projekt.

## <a name="build-the-project"></a>Tworzenie projektu

1. W oknie Project Explorer, kliknij prawym przyciskiem myszy nazwę projektu i wybierz polecenie **Uruchom jako** > **środowiska Maven tworzenie...** Konfigurowanie projektu.

    ![][10]

8. W edytowanie konfiguracji w osiąganiu celów wpisz "czystej instalacji", a następnie kliknij polecenie **Uruchom**.

Komunikaty o stanie są kierowane do okna konsoli. Powinien zostać wyświetlony tworzenie SUKCESU wskazująca projekt bez błędów.

## <a name="run-the-app"></a>Uruchamianie aplikacji

W ostatnim kroku będzie uruchamiania aplikacji w środowisku środowisko uruchomieniowe lokalnego serwera.

Jeśli jeszcze nie określono środowiska wykonawczego serwera w Zaćmienie, konieczne będzie zrobienie tego najpierw.

1. W oknie Project Explorer rozwiń **sieć Web, zawartość**.

5. Kliknij prawym przyciskiem myszy **Search.jsp** > **Uruchamianie jako** > **uruchamiane na serwerze**. Wybierz serwer Apache Tomcat, a następnie kliknij polecenie **Uruchom**.

> [AZURE.TIP] Obszar roboczy inne niż domyślne umożliwia przechowywanie projektu, musisz zmodyfikuj **Uruchamianie konfiguracji** wskaż lokalizację projektu, aby uniknąć błędów uruchamiania serwera. W oknie Project Explorer, kliknij prawym przyciskiem myszy **Search.jsp** > **Uruchom jako** > **Uruchamianie konfiguracji**. Wybierz serwer Apache Tomcat. Kliknij pozycję **argumenty**. Kliknij **obszar roboczy** lub **Systemu plików** , aby ustawić folderu zawierającego projekt.

Po uruchomieniu aplikacji powinien zostać wyświetlony oknie przeglądarki, dostarczając pole wyszukiwania do wprowadzenia terminów.

Poczekaj około minutę przed kliknięciem przycisku **wyszukiwania** dają usług do tworzenia i załadować indeksu. Jeśli zostanie wyświetlony komunikat o błędzie HTTP 404, po prostu trzeba będzie zaczekać nieco już przed próbą ponownie.

## <a name="search-on-usgs-data"></a>Wyszukiwanie danych USGS

Zestaw danych USGS zawiera rekordy, które są istotne do stanu Wyspy Rhode. Kliknięcie przycisku **wyszukiwania** w polu wyszukiwania puste, zostanie wyświetlony górny 50 pozycji, co jest ustawieniem domyślnym.

Wprowadzanie wyszukiwanego terminu da aparat wyszukiwania coś, aby przejść. Spróbuj wprowadzić nazwę regionalne. "Roger Williams" był pierwszą Gubernator Wyspy Rhode. Wiele parków, budynki i szkołach są nazywane od niego.

![][11]

Można również wykonać dowolną z następujących warunków:

- Pawtucket
- Pembroke
- gęś + zielonego

## <a name="next-steps"></a>Następne kroki

Jest to pierwszy samouczek Azure wyszukiwanie na podstawie Java i USGS zestawu danych. Czasem Rozszerzamy będzie tego samouczka, aby udowodnić funkcje dodatkowe wyszukiwania, które można używać w swojej niestandardowych rozwiązań.

Jeśli masz już niektóre tło w wyszukiwaniu Azure, można użyć w tym przykładzie jako startem dla dalszego eksperyment prawdopodobnie przestarzałe [strony wyszukiwania](search-pagination-page-layout.md)lub wykonania [nawigacji aspektowej](search-faceted-navigation.md). Możesz też poprawić na stronie wyników wyszukiwania przez dodawanie liczników i tworzeniu partii dokumenty, tak aby użytkownicy mogą przeglądania wyników.

Jesteś nowym użytkownikiem Azure wyszukiwania? Firma Microsoft zaleca próbuje innych samouczków do zrozumienia można utworzyć. Odwiedź nasze [strony dokumentacji](https://azure.microsoft.com/documentation/services/search/) , aby uzyskać więcej zasobów. Można także wyświetlić łącza w naszym [wideo i listy samouczka](search-video-demo-tutorial-list.md) dostęp do więcej informacji.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
