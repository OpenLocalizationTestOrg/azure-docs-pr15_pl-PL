<properties
   pageTitle="Szybki Start dla wykresu Azure AD interfejsu API | Aure firmy Microsoft"
   description="Azure Active Directory wykresu interfejsu API programu zapewnia programowy dostęp do Azure AD za pośrednictwem interfejsu API usługi REST OData punktów końcowych. Aplikacje za pomocą interfejsu API wykresu można wykonywać tworzenie, odczytywanie, aktualizowanie i usuwanie operacji (OBSŁUGIWAŁ) w katalogu danych i obiektów."
   services="active-directory"
   documentationCenter="n/a"
   authors="PatAltimore"
   manager="mbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="09/16/2016"
      ms.author="patricka"/>

# <a name="quickstart-for-the-azure-ad-graph-api"></a>Szybki Start dla wykresu Azure AD interfejsu API

Interfejs API wykresu Azure Active Directory (AD) zapewnia programowy dostęp do Azure AD za pośrednictwem interfejsu API usługi REST OData punktów końcowych. Aplikacje za pomocą interfejsu API wykresu można wykonywać tworzenie, odczytywanie, aktualizowanie i usuwanie operacji (OBSŁUGIWAŁ) w katalogu danych i obiektów. Na przykład można użyć interfejsu API wykresu utworzyć nowego użytkownika, wyświetlanie lub aktualizowanie właściwości użytkownika, zmiany hasła użytkownika, sprawdź członkostwo w grupach dostępu oparta na rolach, wyłączanie lub usuwanie użytkownika. Aby uzyskać więcej informacji o funkcjach interfejsu API wykresu i scenariuszy aplikacji zobacz [interfejsu API Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) i [wstępnych interfejsu API Azure AD wykresu](https://msdn.microsoft.com/library/hh974476.aspx). 

> [AZURE.IMPORTANT] Funkcje Azure AD wykres interfejsu API jest również dostępne za pośrednictwem [Programu Microsoft Graph](https://graph.microsoft.io/), ujednolicony interfejs API, który zawiera interfejsy API z innych usług firmy Microsoft, takich jak program Outlook, usługi OneDrive programu OneNote, terminarz i funkcji Office Graph, dostępne za pośrednictwem jeden punkt końcowy i przy użyciu tokenu pojedynczy dostęp.

## <a name="how-to-construct-a-graph-api-url"></a>Sposób tworzenia adresu URL interfejsu API wykresu

W API wykresu Aby uzyskać dostęp do katalogu danych i obiektów (innymi słowy, zasobów lub jednostki), z którymi ma do wykonywania operacji OBSŁUGIWAŁ, można użyć adresy URL, na podstawie protokołu danych otwartych (OData). Adresy URL używane w wykresie interfejsu API składa się z czterech głównych części: usługi głównego, identyfikator dzierżawy, ścieżek zasobów i opcje ciągu kwerendy: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Przykładowy następujący adres URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

- **Podstawy usługi**: Azure AD interfejsu API wykres, na poziomie głównym usługi jest zawsze https://graph.windows.net.
- **Identyfikator dzierżawy**: może to być nazwę zweryfikowaną domeną (zarejestrowane), w tym przykładzie powyżej contoso.com. Można również go identyfikator obiektu dzierżawy lub "myorganiztion" lub "me" alias. Aby uzyskać więcej informacji zobacz [adresowania podmioty i operacje w interfejsie API wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
- **Ścieżka zasobu**: w tej sekcji adresu URL identyfikuje zasób do przetwarzanie (użytkowników, grupy, określonego użytkownika lub określonej grupy, itp.) W powyższym przykładzie jest najwyższego poziomu "grupy" adres ustawiany przez zasób. Można też rozwiązać, określonego obiektu, na przykład "Użytkownicy / {identyfikator obiektu}" lub "użytkowników i userPrincipalName".
- **Parametry zapytania**:? oddziela sekcji Ścieżka zasobów z sekcji Parametry kwerendy. Parametr zapytania "wersja api" jest wymagany na wszystkie żądania w interfejsie API wykresu. Interfejs API wykresu obsługuje także następujące opcje kwerendy OData: **$filter**, **$orderby**, **Rozwiń $**, **$top**i **$format**. Nie są obecnie obsługiwane następujące opcje kwerendy: **$count**, **$inlinecount**i **$skip**. Aby uzyskać więcej informacji zobacz [obsługiwane kwerend, filtry i stronicowania opcje interfejsu API Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Wykres interfejsu API wersji

Wersja żądania interfejsu API wykresu można określić w parametrze "wersja api" kwerendy. Dla wersji 1,5 lub nowszy Użyj wartości liczbowe wersji; wersja API = 1.6. We wcześniejszych wersjach pomocą ciągu daty zgodnego formatu YYYY-MM-DD; na przykład wersja api = 11-08-2013. W przypadku funkcji podglądu należy użyć ciągu "beta"; na przykład wersja api = beta. Aby uzyskać więcej informacji o różnicach między wersjami interfejsu API wykresu zobacz [Przechowywanie wersji interfejsu API Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Wykres interfejsu API metadanych

Aby zwrócić pliku metadanych interfejsu API wykres, Dodaj segment "$metadata" po identyfikator dzierżawy, na przykład adres URL dla, następujący adres URL zwraca metadanych firmy pokaz używane przez Eksploratora wykres: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Możesz wprowadzić ten adres URL na pasku adresu przeglądarki sieci web, aby wyświetlić metadane. Dokument metadanych CSDL zwrócony w tym artykule opisano jednostki i złożonych typów, ich właściwości i funkcje i udostępniane przez wersję interfejsu API wykresu żądanej akcji. Pominięcie parametru wersja api zwróci metadanych dla najnowszej wersji.

## <a name="common-queries"></a>Typowe kwerendy

[Typowe kwerendy interfejsu API Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) lista typowych kwerend, które mogą być używane z Azure AD wykresu, łącznie z których można uzyskać dostęp do najwyższego poziomu zasobów w katalogu i kwerend do wykonywania operacji w katalogu.

Na przykład `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` zwraca firmy informacje dotyczące katalogu contoso.com.

Lub `https://graph.windows.net/contoso.com/users?api-version=1.6` Wyświetla listę wszystkich obiektów użytkownika w katalogu contoso.com.

## <a name="using-the-graph-explorer"></a>Za pomocą Eksploratora wykresu

Za pomocą Eksploratora wykres dla interfejsu API Azure AD wykres do pobrania danych katalogu podczas tworzenia aplikacji.

> [AZURE.IMPORTANT] Eksplorator wykresu nie obsługuje zapisywania i usuwania danych z katalogu. Można wykonać tylko operacji odczytu na katalogu Azure AD przy użyciu Eksploratora wykresu.

Oto wynik można zobaczyć w przejdź do Eksploratora wykresu, wybierz pozycję Użyj pokaz firmy i wprowadź `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` Aby wyświetlić wszystkich użytkowników w katalogu Pokaz:

![Azure AD wykresu interfejsu api Eksploratora](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Ładowanie Eksploratora wykresu**: Aby załadować narzędzie, przejdź do [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/). Kliknij pozycję **Użyj pokaz firmy** w celu uruchomienia Eksploratora wykres z dzierżawy przykładowych danych. Nie są potrzebne poświadczenia, aby użyć firmy pokaz. Możesz także kliknij przycisk **Zaloguj** i zaloguj się przy użyciu poświadczeń konta Azure AD w celu uruchomienia Eksploratora wykres dzierżawy usługi. Po uruchomieniu Eksploratora wykresu przed własne dzierżawy, użytkownik lub administrator będzie konieczne zgodę podczas logowania. Jeśli masz subskrypcję usługi Office 365, masz automatycznie dzierżawę Azure AD. Poświadczenia, których używasz do zalogowania się do usługi Office 365 są w rzeczywistości Azure AD kont, a za pomocą tych poświadczeń w Eksploratorze wykresu.

**Uruchamianie kwerendy**: Aby uruchomić kwerendę, wpisz tekst kwerendy w polu tekstowym żądania i kliknij przycisk **Pobierz** lub kliknij pozycję **Wprowadź** klucz. Wyniki są wyświetlane w polu odpowiedź. Na przykład `https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.6` spowoduje wyświetlenie listy wszystkich obiektów grupy w katalogu pokaz.

Zwróć uwagę na następujące funkcje i ograniczenia Eksploratora wykresu:
- Funkcja Autouzupełnianie na zestawach zasobów. Aby to sprawdzić, kliknij polecenie **Użyj pokaz firma** , a następnie kliknij w polu tekstowym żądania (gdzie adres URL firmy pojawia się). Możesz wybrać zasób z listy rozwijanej.

- Obsługa "me" i "myorganization" adresowania aliasów. Na przykład, można użyć `https://graph.windows.net/me?api-version=1.6` zwraca obiekt użytkownika zalogowany użytkownik lub `https://graph.windows.net/myorganization/users?api-version=1.6` zwraca wszystkich użytkowników w bieżącym katalogu. Należy zauważyć, że przy użyciu alias "me" zwraca błąd firmy pokaz ponieważ istnieje nie zalogowano użytkownika zgłaszającego żądanie.

- Sekcja nagłówki odpowiedzi. To może służyć do pomocne w rozwiązywaniu problemów występujących podczas wykonywania kwerend.

- Podgląd JSON dla odpowiedzi z możliwością rozwijania i zwijania.

- Brak obsługi wyświetlania miniatur fotografii.

## <a name="using-fiddler-to-write-to-the-directory"></a>Za pomocą Fiddler do zapisywania w katalogu

Na potrzeby tego przewodnika Szybki Start służy debugowania Web Fiddler było ćwiczenia przeprowadzanie operacji "Pisanie" przed możesz Azure AD katalogu. Aby uzyskać więcej informacji i zainstalować Fiddler zobacz [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

W poniższym przykładzie będzie utworzyć nową grupę zabezpieczeń "MyTestGroup" w katalogu Azure AD za pomocą Fiddler debugowania sieci Web.

**Uzyskaj token dostępu**: Aby uzyskać dostęp do Azure AD wykresu, klienci są wymagane udało się uwierzytelnić Azure AD najpierw. Aby uzyskać więcej informacji zobacz [Uwierzytelnianie scenariusze Azure AD](active-directory-authentication-scenarios.md).

**Redagowanie i uruchom kwerendę**: należy wykonać następujące czynności.

1. Otwórz Fiddler debugowania sieci Web i przejdź na kartę **Kompozytor** .
2. Ponieważ chcesz utworzyć nową grupę zabezpieczeń, wybierz przycisk **Opublikuj** jako metodę HTTP z menu rozwijanego. Aby uzyskać więcej informacji na temat działania i uprawnienia do obiektu grupy zobacz [grupy](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) w [Azure AD wykres pozostałych API Reference](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. W polu obok **wpisu**w wpisz jedną z następujących adresu URL żądania: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.

    > [AZURE.NOTE] Należy podstawić mytenantdomain z nazwą domeny katalogu Azure AD.

4. W polu bezpośrednio poniżej rozwijanego wpis wpisz następujące polecenie:

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] Podstaw programu &lt;token dostępu&gt; z katalogu Azure AD token dostępu.

5. W polu **treści żądania** wpisz następujące polecenie:

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    Aby uzyskać więcej informacji o tworzeniu grup zobacz [Tworzenie grupy](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Więcej informacji na temat obiektów Azure AD i typów, które są dostępne w wykresie i informacji na temat operacji, które można wykonywać na nich za pomocą wykresu zobacz [odwołanie interfejsu API pozostałych Azure AD wykres](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o interfejsie [API Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
- Dowiedz się więcej o [zakresów uprawnień interfejsu API Azure AD wykresu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
