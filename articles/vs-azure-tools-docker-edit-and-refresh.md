<properties
   pageTitle="Debugowanie aplikacji w kontenerze Docker lokalne | Microsoft Azure"
   description="Dowiedz się, jak zmodyfikować aplikację, która jest uruchomiona w kontenerze Docker lokalnych, odśwież kontener za pośrednictwem edytowanie i odświeżanie i ustawiania debugowania punktów kontrolnych"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/22/2016"
   ms.author="mlearned" />

# <a name="debugging-apps-in-a-local-docker-container"></a>Debugowanie aplikacji w kontenerze Docker lokalne

## <a name="overview"></a>Omówienie
Visual Studio Tools dla Docker zapewnia spójny sposób można opracowywać w i sprawdź poprawność aplikacji lokalnie w kontenerze Linux Docker.
Nie musisz ponownym kontenerze każdego wprowadzone zmiany kodu.
W tym artykule przedstawiają sposób za pomocą funkcji "Edytowanie i odświeżanie" Uruchamianie aplikacji sieci Web programu ASP.NET Core w kontenerze Docker lokalnych, wprowadź niezbędne zmiany, a następnie odśwież przeglądarkę, aby te zmiany są widoczne.
Go będzie również pokazano, jak ustawić punktów kontrolnych do debugowania.

> [AZURE.NOTE] Obsługa kontenerem systemu Windows będzie wkrótce w przyszłej wersji

## <a name="prerequisites"></a>Wymagania wstępne
Następujące narzędzia musi być zainstalowany.

- [Visual Studio aktualizacja 2015 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- Aktualizacji [programu Visual Studio 2015 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Zestaw SDK programu Microsoft ASP.NET Core 1.0](https://go.microsoft.com/fwlink/?LinkID=809122)

Aby uruchomić kontenerów Docker lokalnie, musisz klienta docker lokalnego.
Można użyć wydaną [Docker Przybornik](https://www.docker.com/products/overview#/docker_toolbox) wymaga funkcji Hyper-V zostanie wyłączona, lub możesz użyć [Docker dla Windows w wersji Beta](https://beta.docker.com) , która używa funkcji Hyper-V i wymaga systemu Windows 10.

Jeśli przy użyciu zestawu narzędzi Docker, musisz skonfigurować [klienta Docker](./vs-azure-tools-docker-setup.md)

## <a name="1-create-a-web-app"></a>1. Tworzenie aplikacji sieci web

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Dodaj Docker pomocy technicznej

[AZURE.INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]


## <a name="3-edit-your-code-and-refresh"></a>3. edytowanie kodu i odświeżania

Aby szybko przejść zmian, możesz uruchomić aplikacji znajdujące się w kontenerze i wprowadzanie dalszych zmian, ich przeglądanie, jak w programie IIS Express.

1. Ustawienia konfiguracji rozwiązanie `Debug` i naciśnij klawisz ** &lt;klawisze CTRL + F5 >** do tworzenia obrazu docker i uruchom lokalnie.

    Gdy obraz kontener został utworzony i jest uruchomiony w kontenerze Docker, Visual Studio powoduje uruchomienie aplikacji sieci Web w domyślnej przeglądarce.
    Jeśli używasz przeglądarki Microsoft Edge lub w przeciwnym razie zawierają błędy, zobacz sekcję [Rozwiązywanie problemów](vs-azure-tools-docker-troubleshooting-docker-errors.md) .

1. Przejdź do strony informacje, które to miejsce, w którym możemy wykonane w naszym zmiany.

1. Wróć do programu Visual Studio i Otwórz `Views\Home\About.cshtml`.

1. Dodaj następujące zawartości HTML na końcu pliku i zapisać zmiany.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```

1.  Wyświetlanie okna dane wyjściowe po ukończeniu kompilacji .NET i wyświetlić te wiersze, przejdź z powrotem do przeglądarki i Odśwież strony informacje.

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.  Wprowadzone zmiany zostały zastosowane!

## <a name="4-debug-with-breakpoints"></a>4. debugowanie punkty

Często zmiany będą potrzebujesz dalszej inspekcji, używanie funkcji debugowania programu Visual Studio.

1.  Wróć do programu Visual Studio i Otwórz`Controllers\HomeController.cs`

1.  Zamień zawartość metody About() następujące czynności:

    ```
    string message = "Your application description page from wthin a Container";
    ViewData["Message"] = message;
    ````

1.  Ustaw punkt przerwania na lewo od `string message`... linii.

1.  Naciśnij ** &lt;F5 >** uruchomić debugowanie.

1.  Przejdź do strony o trafienie usługi przerwania.

1.  Przełącz się do programu Visual Studio, aby wyświetlić przerwania i sprawdzenie wartości wiadomości.

    ![][2]

##<a name="summary"></a>Podsumowanie

[Visual Studio 2015 Tools Docker](https://aka.ms/DockerToolsForVS)możesz uzyskać wydajności lokalnie, Praca z wzrostu produkcji rozwijających się w kontenerze Docker.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

[Rozwiązywanie problemów z rozwoju Docker programu Visual Studio](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>Więcej informacji na temat Docker przy użyciu programu Visual Studio, systemu Windows i Azure

- [Narzędzia docker programu Visual Studio](http://aka.ms/dockertoolsforvs) — opracowywania kodu .NET Core w kontenerze
- [Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) — tworzenie i wdrażanie docker kontenerów
- [Docker Tools for Visual Studio kod](http://aka.ms/dockertoolsforvscode) — usług językowych do edytowania plików docker z scenariuszy e2e wkrótce
- [Informacje dotyczące kontenera systemu Windows](http://aka.ms/containers)— Windows Server i informacje o serwerze Nano
- [Usługa Azure kontener](https://azure.microsoft.com/services/container-service/) - [zawartości usługi Azure kontenera](http://aka.ms/AzureContainerService)
-    Więcej przykładów pracy z Docker zobacz [Praca z Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) z Połącz [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [Pokaz](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Aby uzyskać więcej Przewodniki Szybki Start z pokaz HealthClinic.biz zobacz [Azure Deweloper narzędzia Przewodniki Szybki Start](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

## <a name="various-docker-tools"></a>Różne narzędzia Docker

[Niektóre narzędzia doskonała docker (Lasker Wojciech blog)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Warto artykuły

[Wprowadzenie do Microservices z NGINX](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Prezentacje

- [Wojciech Lasker: W PORÓWNANIU z Live Warszawie 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [Wprowadzenie do podstawowych ASP.NET @ tworzenie 2016 — miejsce, w którym możesz w pokaz](https://channel9.msdn.com/Events/Build/2016/B810)
- [Opracowywania aplikacji .NET w kontenerach kanału 9](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
