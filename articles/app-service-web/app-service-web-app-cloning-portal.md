<properties
    pageTitle="Za pomocą Azure Portal klonowanie aplikacji sieci Web"
    description="Dowiedz się, jak klonowanie aplikacji sieci Web do nowej aplikacji sieci Web za pomocą Azure Portal."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/08/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Klonowanie Portal Azure za pomocą aplikacji Azure aplikacji usługi#

Funkcję klonowania w [Aplikacjach sieci Web usługi aplikacji Azure](http://go.microsoft.com/fwlink/?LinkId=529714) pozwala łatwo klonowanie istniejącej aplikacji sieci web do nowo utworzonej aplikacji w innym regionie lub w tym samym regionie. Umożliwi to klientom wdrożyć liczbę aplikacji w różnych regionów, szybko i łatwo.

Klonowanie aplikacji jest obecnie obsługiwany tylko dla planów usług premium warstwy aplikacji. Nowa funkcja korzysta te same ograniczenia jako funkcja Kopia zapasowa aplikacji sieci Web, zobacz artykuł [kopię zapasową aplikacji sieci web w usłudze Azure aplikacji](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 


## <a name="cloning-an-existing-app"></a>Klonowanie istniejącej aplikacji ##

Musi być zainstalowany aplikacji sieci web w trybie **Premium** w aby można było utworzyć duplikat dla aplikacji sieci web.

1. Otwórz [Azure Portal](https://portal.azure.com/)karta aplikacji sieci web.
2. Kliknij pozycję **Narzędzia**. Następnie karta **Narzędzia** , kliknij **Klonowanie aplikacji**.

    ![][1]

    > [AZURE.NOTE]
    > Jeśli aplikacja sieci web nie jest już w trybie **Premium** , otrzymasz komunikat z informacją obsługiwane tryby dla klonowanie aplikacji. W tym momencie masz możliwość wybrania **uaktualnienia**.
    
3. Karta **Aplikacji klonowanie** Podaj nazwę nowej aplikacji sieci web, grupa zasobów i Planowanie usługi aplikacji. Również użytkownik będzie mógł określić, czy klonowanie liczbę ustawień aplikacji sieci web źródła lub nie.

    ![][2]

4. Po kliknięciu pozycji **Utwórz** platformie rozpocznie pracy o tworzeniu klonowanie źródła aplikacji sieci web.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonowanie istniejącej aplikacji w środowisku usługi aplikacji##

W karta **Aplikacji klonowanie** klienta będą mieć możliwość wyboru puli aplikacji w środowisku usługi istniejącej aplikacji.

## <a name="current-restrictions"></a>Bieżących ograniczeń ##

Ta funkcja jest obecnie w podglądzie, pracujemy nad dodawać nowe funkcje w czasie, poniżej są znane ograniczenia na podstawie bieżącej aplikacji klonowanie w Azure Portal:

- Azure ustawienia Menedżera ruch nie są sklonowany.
- Automatyczne ustawień skali nie są sklonowany.
- Harmonogram wykonywania kopii zapasowych ustawienia nie są sklonowany.
- Ustawienia VNET nie są sklonowany.
- Aplikacja wnioski nie są automatycznie skonfigurowane w aplikacji sieci web miejsca docelowego
- Łatwe ustawienia uwierzytelniania nie są sklonowany.
- Rozszerzenie kudu nie są sklonowany.
- Porada zasady nie są sklonowany.
- Zawartość bazy danych nie są sklonowany


### <a name="references"></a>Odwołania ###
- [Klonowanie aplikacji sieci Web przy użyciu programu PowerShell](app-service-web-app-cloning.md)
- [Wykonywanie kopii zapasowej aplikacji sieci web w usłudze Azure aplikacji](web-sites-backup.md)
- [Jak utworzyć środowisku aplikacji usługi](app-service-web-how-to-create-an-app-service-environment.md)
- [Tworzenie aplikacji sieci web w środowisku usługi aplikacji](app-service-web-how-to-create-a-web-app-in-an-ase.md)
- [Wprowadzenie do środowiska aplikacji usługi](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png