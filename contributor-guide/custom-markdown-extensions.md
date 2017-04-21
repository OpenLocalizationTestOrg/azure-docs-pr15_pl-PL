<properties
    title="required"
    pageTitle="Rozszerzenia niestandardowe promocji cenowych używane w naszym artykuły techniczne"
    description="Wyświetla listę rozszerzeń niestandardowych promocji cenowych, umożliwiające osadzone pliki wideo, notatek i porady, zawartość do ponownego użycia i innych elementów w artykuły techniczne azure.microsoft.com."
    services=""
    solutions=""
    documentationCenter=""
    authors="tysonn"
    manager="carolz"
    editor=""/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="01/22/2015"
    ms.author="tysonn"/>

## <a name="markdown-for-azuremicrosoftcom"></a>Promocji cenowych dla Azure.microsoft.com

Dla Porady ogólne promocji cenowych zobacz [Podstawowe informacje o promocji cenowych](https://help.github.com/articles/markdown-basics/) i naszych [cheatsheet promocji cenowych](./media/documents/markdown-cheatsheet.pdf?raw=true). Jeśli trzeba utworzyć crosslinks artykuł w promocji cenowych, zobacz [łączenie wskazówki na temat] (. / create-links-markdown.md#markdown-syntax-for-acom-relative-links.md/).

Azure.microsoft.com obsługuje [ogrodzone bloki kodu](https://help.github.com/articles/github-flavored-markdown/#fenced-code-blocks) i [wyróżnianie składni](https://help.github.com/articles/github-flavored-markdown/#syntax-highlighting). Jednak ACOM obsługuje tylko jeden składnię wyróżnianie schemat kolorów, bez względu na język, którego określić w bloku kodu.

## <a name="custom-markdown-extensions-used-in-our-technical-articles"></a>Rozszerzenia niestandardowe promocji cenowych używane w naszym artykuły techniczne

Nasze artykuły za pomocą GitHub flavored promocji cenowych dla większości formatowania artykuł - akapity, łączy, list, nagłówków itd. Ale firma Microsoft korzysta z rozszerzenia niestandardowe promocji cenowych miejsce, w którym jest potrzebna bardziej rozbudowane formatowania renderowanych strony azure.microsoft.com. Oto rozszerzenia, które obecnie użyto:

+ [Uwagi i wskazówki]
+ [Zawiera]
+ [Osadzone pliki wideo]
+ [Selektory technologii i platform]

## <a name="notes-and-tips"></a>Uwagi i wskazówki

Możesz wybrać z 4 typy uwagi i wskazówki:

- AZURE. UWAGA
- AZURE. OSTRZEŻENIE
- AZURE. TIPss
- AZURE. WAŻNE

###<a name="usage"></a>Użycie
Na ogół używany notatek i porady dotyczące opcji w całej sieci. Korzystając z nich, wybierz odpowiedni typ notatki lub porad:

- Za pomocą AZURE. PAMIĘTAJ, aby wyróżnić neutralne lub dodatnią informacjach wyróżniane lub uzupełnia najważniejszych punktów główny tekst. Uwaga zawiera informacje, które ma zastosowanie tylko w szczególnych przypadkach.

  ![](./media/custom-markdown-extensions/Notes-note.PNG)

- Za pomocą AZURE. OSTRZEŻENIE użytkownika warunek, który może powodować problemy w przyszłości. Na przykład wybieranie niektórych opcji lub wprowadzali w niektórych wybór może trwale zablokować możesz do scenariusza.

  ![](./media/custom-markdown-extensions/Notes-warning.PNG)

- Za pomocą AZURE. Porada, aby ułatwić użytkownikom stosowanie technik i procedur opisanych w polu tekst do określonych potrzeb. Porada na ten temat może także zaproponować alternatywne metody, które mogą nie być oczywiste. Porady, jednak nie są niezbędne do podstawy tekstu.

  ![](./media/custom-markdown-extensions/Notes-tip.PNG)

- Za pomocą AZURE. NALEŻY podać informacje, które są niezbędne do wykonania zadania.

  ![](./media/custom-markdown-extensions/Notes-important.PNG)

Gdy te notatki i porady dotyczące obsługuje bloki kodu, obrazy, listy i łącza, spróbuj organizowanie notatek i porady dotyczące niezwykle proste. Jeśli okaże się tworzenia złożonych uwag z wieloma formatowania, który może być podczas logowania, że wystarczy innej sekcji w tekście głównym tego artykułu. I zbyt wiele uwagi w artykule może być rozpraszające uwagę i trudne do przeglądania lub czytania.

###<a name="sample-markdown"></a>Przykładowe promocji cenowych

Wszystkich przykładach pokazano AZURE. UWAGA. Aby użyć Porada, ostrzeżenia lub ważne, należy zastąpić "NOTATEK" w promocji cenowych:

    > [AZURE.TIP]

    > [AZURE.WARNING]

    > [AZURE.IMPORTANT]

Pojedynczego akapitu:

    > [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Microsoft Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut.

Multiparagraph:

    > [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Microsoft Azure aktywne.
    >
    > Jeśli nie masz konta, możesz [utworzyć bezpłatne konto wersji próbnej](http://www.windowsazure.com/pricing/free-trial/) na kilka minut.

## <a name="includes"></a>Zawiera

Tekst do ponownego użycia w naszym repozytorium GitHub znajduje się w plikach nazywamy "zawiera". Jeśli tekst, który ma być używane w wielu artykułów, możesz umieścić odwołanie do tego pliku danych do ponownego użycia. Dodaj sam jest plikiem prosty promocji cenowych (.md). Może zawierać dowolne prawidłowe promocji cenowych, łącznie z tekstu, łączy i obrazów. Zawiera wszystkie promocji cenowych pliki muszą znajdować się w [/ zawiera katalog](https://github.com/Azure/azure-content/tree/master/includes) w katalogu głównym repozytorium. Po opublikowaniu tego artykułu tekst Dołącz bezproblemowo jest zintegrowany opublikowanych tematu.

- Aby odwołać dołączania korzystamy składni.

- Pliki multimedialne, umieszczone w polu Dołącz muszą zostać utworzone w folderze media specyficzne dla Dołącz. Zawiera foldery multimediów znajdować się w [folderze azure — zawartość i zawiera i multimediów](https://github.com/Azure/azure-content/tree/master/includes/media). Katalog multimedia nie powinien zawierać wszystkie obrazy w głównym. Jeśli zawartość nie zawiera obrazy, odpowiedniego katalogu multimedia nie jest wymagane.

###<a name="usage"></a>Użycie

- Użyj zawiera dowolnym miejscu sam tekst do wyświetlenia w wielu artykułach.

- Zawiera mają być używane w przypadku znacznej ilości zawartości — akapitu lub dwie, udostępnionego procedurę lub udostępnionej sekcji. Nie należy używać dla mniejszego niż zdania; **są one nie dla nazwy produktów**.

- Upewnij się, wszystko, co jest napisany tekst w polu Dołącz całe zdania lub frazy, które nie są zależne od poprzedniego tekstu lub następujący tekst w artykułu, który odwołuje się do dołączenia. Ignorowanie tych wskazówek tworzy untranslatable ciąg w artykule podziały zlokalizowanym środowiska. 

- Osadzanie nie zawiera w innych zawiera. Nie są obsługiwane przez DPS publikowania system.

- Nie udostępniaj multimediów między plikami. Za pomocą unikatową nazwę dla każdego Dołącz i artykuł osobny plik. Przechowywanie plików multimedialnych w folderze multimediów skojarzone z Dołącz.

- Nie używaj Dołącz jako tylko zawartość artykułu.  Zawiera mają być dodatkowe w stosunku do zawartości w dalszej części artykułu.

- Ponieważ wszystkie zawiera muszą znajdować się w / zawiera katalog, ścieżka do dołączenia z artykułu jest zawsze

    .. / zawiera

- Nie powtarzaj odwołanie filename łącza lub obrazu w zarówno w tym artykule i Dołącz. Dodawanie "-Dołączanie" do nazwy odwołanie lub multimediów łącza do odwołania:

 **Odwołanie łącza**

 Zmiana: odata.org do: obejmują odata.org

 **Obraz odniesienia**

 Zmiana: table.png do: include.png tabeli

###<a name="sample-markdown"></a>Przykładowe promocji cenowych
Składnia dodawania dołączania do artykułu dokumentacji jest następująca:

    [AZURE.INCLUDE [include-short-name](../includes/include-file-name.md)]

Przykład

    [AZURE.INCLUDE [howto-blob-storage](../includes/howto-blob-storage.md)]

Nazwą Dołącz bez ścieżkę i bez rozszerzenia .md jest pierwsza część Dołącz. Druga część jest ścieżką względną do dołączenia w / zawiera katalog z rozszerzeniem .md.

###<a name="rendering"></a>Renderowanie

Na stronie GitHub renderowanych dołączenia będą renderowane w następujący sposób:

 [AZURE. Zawiera instrukcje obiektów blob magazynowania]

W renderowanych HTML na azure.microsoft.com, kodu HTML z zawiera scalone z resztą dokumentu w formacie HTML. Kod HTML będą jednak zawierać HTML komentarz z oryginalną obejmują nazwę promocji cenowych i mieszania Zatwierdź GitHub. Ten komentarz jest dołączany w celu rozwiązywania problemów, aby źródłem zawartości można łatwo zidentyfikować i znaleziony w GitHub:

  ![](./media/custom-markdown-extensions/include.png)


## <a name="embedded-videos"></a>Osadzone pliki wideo

Nasze artykuły techniczne obsługiwał klipy wideo embeddeded w artykułach technicznych, dopóki klipów wideo w witrynie firmy Microsoft [kanału 9](http://channel9.msdn.com/) . Klipy wideo z kanału 9 musi zostać zintegrowany z [azure.microsoft.com Centrum klip wideo](http://azure.microsoft.com/documentation/videos/home/). Obecnie nie jest obsługiwana osadzonego wideo YouTube; Jeśli jesteś współautora społeczności, to łącze do witryny YouTube, jeśli plik wideo, który chcesz wyróżnić została ogłoszona — Zapraszamy. Współautorzy Microsoft należy użyć kanału 9 i wyśrodkuj wideo.

### <a name="usage"></a>Użycie

- Upewnij się, że klip wideo jest w Centrum wideo.

- Skopiuj identyfikator wideo z przyjazny adres URL klipu wideo na kanału 9 lub Centrum Azure klip wideo. Na przykład identyfikator wideo dla klipu wideo w [http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/](http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/) jest **azure harmonogram nietypowe harmonogramów**.

### <a name="syntax"></a>W składni

    > [AZURE.VIDEO video-id-string]

### <a name="rendering"></a>Renderowanie

W GitHub: [https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md](https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md)

Opublikowany artykuł: [http://azure.microsoft.com/documentation/articles/web-sites-backup/](http://azure.microsoft.com/documentation/articles/web-sites-backup/)


## <a name="technology-and-platform-selectors"></a>Selektory technologii i platform

Za pomocą technologii i platformy switchers w artykułach technicznych podczas tworzenia wielu podtypy tego samego artykułu adres różnic w celu wykonania w technologii lub platformy. To jest zazwyczaj najbardziej odpowiednią zawartość naszej platformy urządzeń przenośnych dla deweloperów. Obecnie są dwa różne typy [selektory prosty](#simple-selectors) i [selektory dwukierunkowe](#two-way-selectors)selektorów.

Tym samym promocji cenowych selektor odbywa się w każdym temacie w ramach zaznaczenia, dlatego zalecamy umieszczenie selektor temat w polu Dołącz, a następnie odwoływanie się do tego Dołącz we wszystkich tematów korzystające z tym samym selektor.

###<a id="simple-selectors"></a>Prosta selektory

Prosta selektory (jednokierunkowe) są renderowane jako zestaw przycisków opcji bezpośrednio pod tytułem. Użyj tych przycisków, gdy użytkownicy musieli wybierać tematy w jeden zestaw platformy lub technologii, takich jak .NET, Node.js i Java.  Korzystać z rozszerzeniem niestandardowe promocji cenowych dla dowolnego selektorów — nie należy używać HTML selektory.  

Zobacz [Rozpoczynanie pracy z koncentratorów powiadomień](http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started/) aby zobaczyć, jak autora wersji 8 samego artykułu, ale używanych selektory umożliwiające nawigacji przez ich wszystkich.

![Selektor prosty przykład](./media/custom-markdown-extensions/selectors.PNG)

####<a name="syntax"></a>W składni

    > [AZURE.SELECTOR]
    - [Łączenie etykiet #1](link #1 url)
    - [Etykieta łącza #2](link #2 url)

Przykład:

    > [AZURE.SELECTOR]
    - [Uniwersalny Windows](../articles/notification-hubs-windows-store-dotnet-get-started/)
    - [Windows Phone](../articles/notification-hubs-windows-phone-get-started/)
    - [iOS](../articles/notification-hubs-ios-get-started/)
    - [Android](../articles/notification-hubs-android-get-started/)
    - [Kindle](../articles/notification-hubs-kindle-get-started/)
    - [Baidu](../articles/notification-hubs-baidu-get-started/)
    - [Xamarin.iOS](../articles/partner-xamarin-notification-hubs-ios-get-started/)
    - [Xamarin.Android](../articles/partner-xamarin-notification-hubs-android-get-started/)

#### <a name="rendering"></a>Renderowanie

Obraz powyżej zawiera renderowanie azure.microsoft.com. Strony GitHub renderowanych selektory są renderowane jako lista punktowana łączy.

###<a id="two-way-selectors"></a>Dwukierunkowa selektory

Dwukierunkowa selektory umożliwiająca użytkownikom wybieranie tematy z macierzą w dwie strony. Jest to istotne w przypadku, gdy Azure technologii, takich jak usługi Mobile obsługuje wiele platform wewnętrznej bazy danych, a także wielu klientów. Należy pamiętać o następujących:

- Gdy została zaprojektowana jako `(Platform | Backend)`, tekst dropwdown teraz można dostosować.
- Element listy nie ma potrzeby dla każdego punktu w matrycy, ale tylko elementu, której adres URL tematu istnieje i nie jest taki sam.
- Łącze może być dowolny adres URL, chociaż jest zazwyczaj innego tematu GitHub.

Zobacz [Rozpoczynanie pracy z usługami Mobile](http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/) , aby zobaczyć, jak autora wersji 15 sam artykuł (9 platformy klientów przenośnych i 2 wewnętrznej bazy danych platformy), ale używanych selektory umożliwiające nawigacji przez ich wszystkich. Należy zauważyć, że 3 nie zawierają obie wersje wewnętrznej bazy danych.

![Przykład selektory dwukierunkowe](./media/custom-markdown-extensions/selector-list.png)

####<a name="syntax"></a>W składni

    > [AZURE. SELEKTOR listy (Dropdown1 | Dropdown2)]     -  [(Dropdown1Text1 | Dropdown2Text1)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text1 | Dropdown2Text2)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text2 | Dropdown2Text3)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text3 | Dropdown2Text4)](../articles/dropdown1-text1-dropdown2-text1.md)

Przykład:

    > [AZURE. SELEKTOR listy (platformy | Wewnętrznej bazy danych)]     -  [(iOS | .NET)](./mobile-services-dotnet-backend-ios-get-started-push.md)
    - [(iOS | JavaScript)](./mobile-services-javascript-backend-ios-get-started-push.md)
    - [(Windows uniwersalny C# | .NET)](./mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows uniwersalny C# | JavaScript)](./mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows Phone | .NET)](./mobile-services-dotnet-backend-windows-phone-get-started-push.md)
    - [(Windows Phone | JavaScript)](./mobile-services-javascript-backend-windows-phone-get-started-push.md)
    - [(Android | .NET)](./mobile-services-dotnet-backend-android-get-started-push.md)
    - [(Android | JavaScript)](./mobile-services-javascript-backend-android-get-started-push.md)
    - [(Xamarin iOS | JavaScript)](./partner-xamarin-mobile-services-ios-get-started-push.md)
    - [(Xamarin Android | JavaScript)](./partner-xamarin-mobile-services-android-get-started-push.md)

#### <a name="rendering"></a>Renderowanie

Obraz powyżej zawiera renderowanie azure.microsoft.com. Strony GitHub renderowanych selektory są renderowane jako lista punktowana łączy.

<!--Anchors-->
[Uwagi i wskazówki]: #notes-and-tips
[Zawiera]: #includes
[Osadzone pliki wideo]: #embedded-videos
[Selektory technologii i platform]: #technology-and-platform-selectors

###<a name="contributors-guide-links"></a>Współautorzy przewodnik łącza

- [Artykuł Omówienie](./../README.md)
- [Indeks artykułów ze wskazówkami dotyczącymi](./contributor-guide-index.md)
