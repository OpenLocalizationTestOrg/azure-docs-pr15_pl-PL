<properties 
   pageTitle="Azure SDK dla środowiska .NET 2.5.1 informacje o wersji" 
   description="Azure SDK dla środowiska .NET 2.5.1 informacje o wersji" 
   services="app-service" 
   documentationCenter=".net,nodejs,java" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/10/2016"
   ms.author="juliako"/>


# <a name="azure-sdk-for-net-251-release-notes"></a>Azure SDK dla środowiska .NET 2.5.1 informacje o wersji

Ten dokument zawiera informacje o wersji dla Azure SDK dla .NET 2.5.1 Zwolnij. 

##<a name="azure-sdk-for-net-251-release-notes"></a>Informacje o wersji Azure SDK dla środowiska .NET 2.5.1

Poniżej przedstawiono nowe funkcje i aktualizacje w SDK Azure dla środowiska .NET 2.5.1.

- Nowy features\scenarios związanych z **Rozszerzeniami narzędzia sieci Web**. 

    - Azure witryn sieci Web zmieniono na Azure aplikacji usługi. Aby uzyskać więcej informacji, zobacz [Usługa Azure aplikacji i istniejące usługi Azure](app-service-changes-existing-services.md).
    - Dodano Azure obsługę interfejsu API aplikacji (wersja Preview), tak, aby klienci mogą publikowanie projektów ASP.NET jako interfejsu API aplikacje, a następnie użyj Dodaj > gest Azure klienta aplikacji API języka C# projektów do generowania oparty na strukturze wdrożonym aplikacji interfejsu API kodu. 
    - Węzeł witryny sieci Web w Eksploratorze serwera została zastąpiona zamiast węźle Azure aplikacji usługi obsługuje zasobów oparte na grupach grupowania aplikacji interfejsu API Azure, aplikacje Mobile i aplikacjach sieci Web.
    - Dodano Azure obsługę aplikacji Mobile (wersja Preview), tak aby klienci mogli tworzyć nowych projektów aplikacji Mobile, dodać kontrolery aplikacji Mobile, publikowanie projektów i zdalne debugowanie aplikacji.
    - Dodawanie > gest klienta aplikacji API Azure obsługuje teraz pliki lokalne Swagger JSON, aby interfejs API sieci Web umożliwia programistom NuGets innych firm, takich jak Swashbuckle Generowanie Swagger lub autor go ręcznie. Dzięki temu deweloperzy klienta mogą używać funkcji Generowanie kodu do korzystania z dowolnego punktu końcowego Swagger w projektach C#. 
    - Udoskonalono aplikacji sieci Web i okien dialogowych publikowania aplikacji interfejsu API do obsługi pojęcia Azure Portal zasobów grupowania i zaznaczenia i tworzenie grup zasobów Azure i plany usługi aplikacji znajdują się w nowej aplikacji sieci Web i aplikacji interfejsu API okno obsługi administracyjnej. 
    - Azure węzły interfejsu API aplikacji Server Explorer zawierają łącza do aplikacji interfejsu API głębokości łącze w Azure Portal, a także inne funkcje, takie jak przesyłanie strumieniowe dziennika i zdalne debugowanie.

    Znane problemy i ograniczenia bieżącego [Azure SDK .NET 2.5.1 poniższą sekcję](app-service-release-notes.md#known_issues_2_5_1) .


- Nowy features\scenarios związane z **Narzędzia HDInsight** w programie Visual Studio są włączone w tej wersji. 
    - Lokalne sprawdzanie poprawności gałęzi skryptów. Przycisk sprawdzania poprawności skryptu na pasku narzędzi, aby sprawdzić, czy za pomocą skryptu są wszystkie błędy. 
    - Ulepszone debugowanie gałęzi zadań. Po zalogowaniu się do dzienników przędzy w programie Visual Studio, można teraz debugowania gałęzi zadania. Jeśli aplikacja ma problemy z wydajnością, badanie dzienniki PRZĘDZY zapewni użyteczne informacje.
    - (Publiczna wersja Preview) Autouzupełnianie słów kluczowych i IntelliSense obsługę gałęzi. Aby pomóc Autor skryptów gałęzi, HDInsight Tools for Visual Studio dodane Autouzupełnianie słów kluczowych i IntelliSense obsługa gałęzi.
    - Obsługa Burza. Za pomocą narzędzia HDInsight programu Visual Studio można teraz można opracowywać Burza topologii Spouts-tekst "Śruby" w języku C#. Można przesłać rozwinięte topologia do klastrów Burza i zapoznać się ze stanem topologia błyskawicy dziobek. Za pomocą dzienników klienta i dzienniki systemu rozwiązywać problemy z Burza topologii i tekst "Śruby"-Spouts. Można także użyć istniejących zbiorów JAVA w Burza na HDInsight.
    
    Aby uzyskać więcej informacji zobacz [rozpocząć korzystanie z narzędzia Hadoop HDInsight programu Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).



##<a id="known_issues_2_5_1"></a>Azure SDK dla środowiska .NET 2.5.1 znane problemy i ograniczenia

- Azure aplikacje interfejsu API jest widoczny jako cel wdrażania aplikacji Mobile. Aplikacje sieci Web powinny być tylko miejsca docelowego dla aplikacji Mobile do kolejnych wersji. 
- Azure obsługi administracyjnej aplikacji interfejsu API może spowodować sukcesu, ale sporadycznie nie można zaktualizować postęp w oknie wykonanie usługi Azure aplikacji. Obejście jest sprawdzenie stanu nowej aplikacji interfejsu API Azure w Azure Portal. 
- Plik > Nowy Projekt > interfejsu API aplikacji > środowisko F5 kończy się błędem HTTP, ponieważ istnieje nie default/index.html. Obejście to ręcznie, przejdź do adresu URL /api/values. 
- Okresowo ikony Server Explorer pojawiają się spłaszczoną. Ponowne uruchamianie w PORÓWNANIU z rozwiązuje ten problem. 
- Jeśli jest wyjątek podczas aplikacji sieci Web lub aplikacji interfejsu API inicjowania obsługi administracyjnej (na przykład błędy Przekroczono przydział lub zduplikowana nazwa bramy Azure interfejsu API aplikacji), błędy Pokaż tekst nieprzetworzonych JSON. 
- Problemy przejściowymi tworzenia projektu po zaznaczeniu wniosków aplikacji w czasie tworzenia projektu.
- Czasami wygenerowany kod klienta aplikacji API Azure brakuje nazw, należy ręcznie dołączanych (lub automatycznie zaimportowane za pośrednictwem programu Visual Studio wskaźników) umożliwia także kompilowanie kodu. 
- Projekty aplikacji Mobile ma być publikowana do aplikacji sieci web, ale należy wybrać witryny utworzone jako aplikacji Mobile w Azure Portal, ponieważ projektów w aplikacji Mobile wymaga bazy danych. 
- Strona startowa dla aplikacji Mobile używany termin "usług mobilnych" zamiast "aplikacji dla urządzeń przenośnych" 
- Tworzenie projektów w aplikacji Mobile może potrwać minutę tworzenie. 
- W aplikacji interfejsu API inicjowania obsługi administracyjnej (w niektórych przypadkach) komunikat o błędzie wróci z interfejsu API Azure odbicia, że uprawnienia nie można ustawić poprawnie, a aplikacja interfejsu API prawidłowo zainicjowano jest gotowa do użycia. Uprawnienia przy użyciu Azure Portal można ustawić ręcznie.
- Wnioski aplikacji nie jest obsługiwane na szablony aplikacji interfejsu API i szablony aplikacji Mobile.
- Interfejs API aplikacji projektów nie można używać w połączeniu z projektami usługi w chmurze.
- Szablony projektów interfejsu API aplikacji są dostępne tylko w języku C#.
- Zużycie aplikacji interfejsu API za pośrednictwem menu kontekstowego "Dodawanie Azure interfejsu API aplikacji klienckich" jest obsługiwana tylko w języku C#.

 
