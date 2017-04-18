<properties 
    pageTitle="Pobierz Azure SDK dla języka Java" 
    description="Dowiedz się, jak pobrać SDK Azure dla języka Java, z próbki kod przewidziany dla środowiska Maven projektów i podstawowa procedura instalacją Tookit Azure dla programu Eclipse." 
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

# <a name="download-the-azure-sdk-for-java"></a>Pobierz Azure SDK dla języka Java

Ten artykuł zawiera instrukcje dotyczące pobraniu i zainstalowaniu bibliotek Azure dla języka Java.

**Uwaga:** Biblioteki Azure dla języka Java są rozdzielane w obszarze [Licencja Apache, wersja 2.0][license].

## <a name="azure-libraries-for-java---manual-download"></a>Azure bibliotek dla języka Java — pobieranie ręczne

Aby ręcznie zainstalować bibliotek Azure dla języka Java, kliknij przycisk <http://go.microsoft.com/fwlink/?LinkId=690320> do pobrania pliku ZIP zawierający wszystkich bibliotek, a wszystkie zależności.

Pliku zip pobrany z komputerem pobierającego zawartość i jedną z następujących opcji umożliwia dodawanie plików JAR do projektu:

* Importowanie plików JAR do projektu Java w Zaćmienie.

* Konfigurowanie **Ścieżki tworzenie** projektu języka Java w Zaćmienie do dołączono ścieżkę dostępu do plików JAR.

Aby uzyskać szczegółowe informacje o konfigurowaniu ścieżek kompilacji w Zaćmienie zobacz artykuł [Java Tworzenie ścieżki] w witrynie sieci Web Zaćmienie.

**Uwaga:** Zobacz license.txt i ThirdPartyNotices.txt pliku wewnątrz ZIP licencji i inne informacje.

## <a name="azure-libraries-for-java---building-with-maven"></a>Azure biblioteki Java — tworzenie ze środowiska Maven

### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Krok 1 — Konfigurowanie projektu, aby użyć środowiska Maven dla kompilacji

Tworzenie projektów środowiska Maven w Zaćmienie, które przy użyciu bibliotek Azure dla języka Java, postępując zgodnie z instrukcjami w [Wprowadzenie do bibliotek zarządzania Azure dla języka Java] [ maven-getting-started] artykuł. 

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Krok 2 — Konfigurowanie ustawień środowiska Maven z wymaganych zależności

Po projektu została skonfigurowana do używania środowiska Maven dla kompilacji, możesz dodać zależności wymaganych do pliku pom.xml składni, takich jak poniższym przykładzie. Należy zauważyć, że nie musisz dodać co współzależności, który jest wyświetlany w poniższym przykładzie; Musisz dodać określone zależności, które wymagają projektu.

> [AZURE.NOTE] W każdej `<version>` elementu w następującym przykładzie, zastąp symbole zastępcze "n.n.n" w tym przykładzie z liczbami prawidłowej wersji, który można uzyskać z [Repozytorium bibliotek Azure w środowiska Maven].

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Instalowanie narzędzi Azure dla programu Eclipse

W tej sekcji przedstawiono podstawowe instrukcje dotyczące instalowania narzędzi Azure dla programu Eclipse; Aby uzyskać szczegółowe instrukcje zobacz [Instalowanie narzędzi Azure dla programu Eclipse].

### <a name="prerequisites"></a>Wymagania wstępne

1. Systemy Windows operting, wymienione w artykule [Co nowego w zestaw narzędzi Azure dla Zaćmienie] .
1. Macintosh i Linux oraz systemy operting wymienione w artykule [Co nowego w zestaw narzędzi Azure dla Zaćmienie] .
1. Zaćmienie-IDE dla deweloperów Estonia Java indygo lub nowszym. To można pobrać z <http://www.eclipse.org/downloads/>.

### <a name="basic-installation-steps"></a>Podstawowe kroki instalacji

1. W Zaćmienie w menu **Pomoc** wybierz **Instalowanie nowego oprogramowania**.
1. Wprowadź lokalizację witryny <http://dl.microsoft.com/eclipse> i naciśnij klawisz **Enter**.
1. Zaznacz elementy, które mają być zainstalowana, a następnie kliknij przycisk **Zakończ**.

Zestaw narzędzi Azure dla Zaćmienie używa najnowszą wersję pakietu Azure SDK. To można pobrać za pomocą Instalatora platformy sieci Web (WebPI) w <http://go.microsoft.com/fwlink/?LinkID=252838>. Jednak jeśli nie jest zainstalowany, podczas tworzenia pierwszego projektu wdrożenia Azure zestaw narzędzi Azure dla Zaćmienie automatycznie instaluje odpowiednią wersję zestawu SDK Azure.

## <a name="see-also"></a>Zobacz też

[Azure zestaw narzędzi dla programu Eclipse]

[Instalowanie narzędzi Azure dla programu Eclipse] 

[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie]

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Repozytorium Azure bibliotek w środowiska Maven]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalowanie narzędzi Azure dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Ścieżka kompilacji Java]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Co nowego w Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=690333
