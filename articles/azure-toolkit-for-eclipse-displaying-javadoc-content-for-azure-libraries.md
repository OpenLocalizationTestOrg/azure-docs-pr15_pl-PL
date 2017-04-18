<properties
    pageTitle="Wyświetlanie zawartości Javadoc w Zaćmienie pakietu Azure bibliotek dla języka Java"
    description="Sposób wyświetlania zawartości Javadoc dla bibliotek Azure w Zaćmienie."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Wyświetlanie zawartości Javadoc w Zaćmienie pakietu Azure bibliotek dla języka Java #

Zawartość Javadoc bibliotek Azure Java można wyświetlać w środowisku Zaćmienie kojarząc Javadoc zawartości do bibliotek Azure dla języka Java. Poniższe kroki pokazują jak używać tej funkcji w obrębie Zaćmienie.

W poniższej procedurze założono, że biblioteki Azure Java zostało już dodane do ścieżki kompilacji.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Aby wyświetlić zawartość Javadoc w Zaćmienie bibliotek Azure Java ##

* W jego Zaćmienie Eksplorator projektu w sekcji **Biblioteki odwołanie do** projektu, otwieranie menu kontekstowego dla biblioteki Azure dla Java JAR. Na przykład **firmy microsoft — windowsazure-interfejsu api-0.1.0.jar** (numer wersji może być inny, w zależności od wersji zainstalowano).
* Kliknij pozycję **Właściwości**.
* W oknie dialogowym **Właściwości** w okienku po lewej stronie kliknij **W miejscu Javadoc**. Zostanie wyświetlone okno dialogowe **Lokalizacja Javadoc** .
* Możesz określić **Adres URL Javadoc**lub **Javadoc w archiwum**.
    * Jeśli chcesz określić **Adres URL Javadoc**, za pomocą adresów URL, taki jak **http://dl.windowsazure.com/javadoc** lub **http://dl.windowsazure.com/storage/javadoc**.
    * Jeśli zdecydujesz się na użycie **Javadoc w archiwum**, można określić zewnętrznego pliku lub plik obszaru roboczego.
    Wprowadź żądaną wartość, a następnie Przeglądaj i sprawdź poprawność stosownie do potrzeb. Poniższy przykład przypisuje bibliotek Azure dla języka Java odpowiednich Javadoc JAR ma pobranych lokalnie w folderze o nazwie **c:\MyAzureJARs**.
    ![][ic553487]
* *Opcjonalnie*: kliknij przycisk **Sprawdzanie poprawności**. Potencjalne problemy z Javadoc JAR mogą być wyświetlone w tym miejscu.
* Kliknij **przycisk OK**.

Gdy skojarzony z biblioteką, zawartość Javadoc powinien być wyświetlany w ramach usługi IDE Zaćmienie. Na przykład jeśli `blob` jest definiowana typu `CloudBlockBlob` w kodzie, Oto przykład Javadoc zawartości, które pojawia się podczas wpisywania `blob.acquireLease` w kodzie:

![][ic553488]

## <a name="see-also"></a>Zobacz też ##

[Azure zestaw narzędzi dla programu Eclipse][]

[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie][]

[Instalowanie narzędzi Azure dla programu Eclipse][] 

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure][].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalowanie narzędzi Azure dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png