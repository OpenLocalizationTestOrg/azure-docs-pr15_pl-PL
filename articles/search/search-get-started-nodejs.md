<properties
    pageTitle="Wprowadzenie do wyszukiwania Azure w NodeJS | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Szczegółową konstruowania aplikacji wyszukiwania na usługi wyszukiwania w chmurze hostowanej Azure za pomocą NodeJS językiem programowania."
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

# <a name="get-started-with-azure-search-in-nodejs"></a>Wprowadzenie do wyszukiwania Azure w NodeJS
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Dowiedz się, jak utworzyć niestandardową aplikację wyszukiwania NodeJS, która używa Azure wyszukiwanie jej wyników wyszukiwania. Ten samouczek wykorzystuje [Interfejsu API usługi REST Usługa wyszukiwania Azure](https://msdn.microsoft.com/library/dn798935.aspx) do utworzenia obiektów i używane w tym wykonywania operacji.

Użyliśmy [NodeJS](https://nodejs.org) i NPM, [Sublime tekst 3](http://www.sublimetext.com/3)i programu Windows PowerShell w systemie Windows 8.1 do projektowania i testowania kod.

Aby uruchomić ten przykład, musisz mieć usługi Azure wyszukiwania, w której możesz się zalogować do usługi [Azure Portal](https://portal.azure.com). Aby uzyskać instrukcje krok po kroku, zobacz temat [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) .

## <a name="about-the-data"></a>Informacje o danych

Ta aplikacja przykładowa korzysta z danych z [Stanów Zjednoczonych geologicznych usług (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtrowane w stanie Wyspy Rhode zmniejszyć rozmiar zestawu danych. Użyjemy te dane do tworzenia aplikacji wyszukiwania, która zwraca budynki doniosłej, takie jak szpitale i szkoły, a także geologicznych funkcji, takich jak strumienie, lak i kolejnych konferencji.

W tej aplikacji **DataIndexer** program tworzy i ładowania indeksu przy użyciu konstrukcji [indeksatora](https://msdn.microsoft.com/library/azure/dn798918.aspx) pobierania filtrowanego zestawu danych USGS z publicznej bazy danych SQL Azure. Poświadczenia i połączenia ze źródłem danych online informacje w kodzie programu. Nie dalszej konfiguracji jest to konieczne.

> [AZURE.NOTE] W tym zestawie danych do utrzymywania w obszarze limit 10 000 dokumentów bezpłatne poziomu cen możemy zastosowano filtr. Jeśli używasz warstwie standardowy, ten limit nie ma zastosowania. Aby uzyskać szczegółowe informacje o pojemności dla każdego poziomu cen zobacz [limity usługi wyszukiwania](search-limits-quotas-capacity.md).


<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Znajdź nazwę usługi i klucz interfejsu api usługi Azure wyszukiwania

Po utworzeniu usługę, wróć do portalu można uzyskać adres URL lub `api-key`. Połączenia z usługą wyszukiwania są wymagane zarówno adres URL i `api-key` do uwierzytelnienia połączenia.

1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. Na pasku szybkiego dostępu kliknij **Usługa wyszukiwania** , aby wyświetlić listę wszystkich usług wyszukiwania Azure zainicjowany dla subskrypcji.
3. Wybierz usługę, której chcesz użyć.
4. Na pulpicie nawigacyjnym usługi pojawi się Kafelki ważnymi informacjami, a także ikona klucza dostępu do kluczy administratora.

    ![][3]

5. Skopiuj adres URL usługi, klucz Administrator i klucza kwerendy. Konieczne będzie wszystkie trzy później, po dodaniu ich do pliku config.js.

## <a name="download-the-sample-files"></a>Pobieranie przykładowych plików

Do pobrania próbki, stosując jedną z poniższych metod.

1. Przejdź do [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo).
2. Kliknij przycisk **Pobierz ZIP**, Zapisz plik zip, a następnie wyodrębnić wszystkie pliki, które zawiera.

Wszystkie modyfikacje kolejnych plików i wykonywania instrukcji zostaną wprowadzone przed pliki w tym folderze.


## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>Aktualizowanie config.js. za pomocą adresu URL usługi wyszukiwania i klucz interfejsu api usługi

Przy użyciu adresu URL i klucz interfejsu api, który został skopiowany wcześniej, określ adres URL, administrator klucza i klucza kwerendy w pliku konfiguracji.

Administrator klawiszy udzielanie pełną kontrolę nad operacje usług, takich jak tworzenie lub usuwanie indeksu i ładowania dokumentów. Natomiast kwerendy klawisze służą do operacji tylko do odczytu, zwykle są używane w aplikacjach klienckich, łączących się Azure wyszukiwania.

W tym przykładzie możemy zawiera klucz kwerendy w celu, utrwalanie najlepsze praktyki przy użyciu klucza kwerendy w aplikacjach klienckich.

Następujące zrzut ekranu zawiera **config.js** otwarte w tekście edytor z odpowiednich wpisów rozgraniczone tak, aby było widać, gdzie można zaktualizować plik z wartościami, które są ważne tej usługi wyszukiwania.

![][5]


## <a name="host-a-runtime-environment-for-the-sample"></a>Host środowiska wykonawczego dla próbki

Próbki wymaga serwera HTTP, którą można zainstalować za pomocą npm.

Okno programu PowerShell dla następujących poleceń.

1. Przejdź do folderu, w którym znajduje się plik **package.json** .
2. Typ `npm install`.
2. Typ `npm install -g http-server`.

## <a name="build-the-index-and-run-the-application"></a>Tworzenie indeksu i uruchamianie aplikacji

1. Typ `npm run indexDocuments`.
2. Typ `npm run build`.
3. Typ `npm run start_server`.
4. Bezpośrednie w przeglądarce`http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>Wyszukiwanie danych USGS

Zestaw danych USGS zawiera rekordy, które są istotne do stanu Wyspy Rhode. Kliknięcie przycisku **wyszukiwania** w polu wyszukiwania puste, zostanie wyświetlony górny pozycje 50, co jest ustawieniem domyślnym.

Wprowadzanie wyszukiwanego terminu da aparat wyszukiwania coś, aby przejść. Spróbuj wprowadzić nazwę regionalne. "Roger Williams" był pierwszą Gubernator Wyspy Rhode. Wiele parków, budynki i szkołach są nazywane od niego.

![][9]

Można również wykonać dowolną z następujących warunków:

- Pawtucket
- Pembroke
- gęś + zielonego


## <a name="next-steps"></a>Następne kroki

Jest to pierwszy samouczek Azure wyszukiwanie na podstawie NodeJS i USGS zestawu danych. Czasem Rozszerzamy będzie tego samouczka, aby udowodnić funkcje dodatkowe wyszukiwania, które można używać w swojej niestandardowych rozwiązań.

Jeśli masz już niektóre tła w wyszukiwaniu Azure, można użyć w tym przykładzie jako startem dla próby suggesters (zapytania dynamiczne lub autouzupełniania), filtry i nawigacji aspektowej. Możesz też poprawić na stronie wyników wyszukiwania przez dodawanie liczników i tworzeniu partii dokumenty, tak aby użytkownicy mogą przeglądania wyników.

Jesteś nowym użytkownikiem Azure wyszukiwania? Firma Microsoft zaleca próbuje innych samouczków do zrozumienia można utworzyć. Odwiedź nasze [strony dokumentacji](https://azure.microsoft.com/documentation/services/search/) , aby uzyskać więcej zasobów. Można także wyświetlić łącza w naszym [wideo i listy samouczka](search-video-demo-tutorial-list.md) uzyskiwania dostępu do więcej informacji.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png
