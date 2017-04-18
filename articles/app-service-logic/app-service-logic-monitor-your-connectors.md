<properties
    pageTitle="Zarządzanie i monitorowanie do łączników i interfejsu API aplikacji w aplikacji usługi | Microsoft Azure"
    description="Widok wydajności łączników i aplikacje interfejsu API w aplikacjach logiczny; Architektura microservices"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger"
    manager="anneta"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="mandia"/>

# <a name="manage-and-monitor-your-built-in-api-apps-and-connectors"></a>Monitorowanie wbudowanych łączników i aplikacjami interfejsu API i zarządzanie nimi

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2014-12-01-podgląd aplikacji logicznych.

Utworzono wbudowanej aplikacji interfejsu API. Co teraz?

Platformy Azure co aplikacja interfejsu API jest osobne witryny sieci web hostowanej Azure. Dzięki temu łatwo widać, ile żądań zostały wprowadzone i zobacz, jak dużo danych jest używany przez łącznik. Można także utworzyć kopię zapasową aplikacji interfejsu API, tworzenie alertów, włączyć Tinfoil zabezpieczeń i dodawanie użytkowników i ról.

W tym temacie opisano niektóre z różnych opcji, aby zarządzać aplikacji interfejsu API.

Aby wyświetlić te funkcje wbudowane, Otwórz aplikację interfejsu API w [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Jeśli aplikacja interfejsu API znajduje się na Twojej startboard, zaznacz go, aby wyświetlić właściwości. Można również zaznaczyć pole wyboru **Przeglądaj**, wybierz pozycję **Aplikacje interfejsu API**, a następnie wybierz aplikację interfejsu API:

![][browse]

## <a name="see-the-properties-you-entered"></a>Zobacz wprowadzonych właściwości

Po otwarciu aplikacji interfejsu API dostępnych kilka funkcji i zadań:

![][settings]

Można:

- **Ustawienia** zawiera określone informacje w aplikacji interfejsu API, łącznie z Szczegóły subskrypcji i listę użytkowników, którzy mają dostęp do aplikacji interfejsu API. Można również zwiększyć lub zmniejszyć liczbę wystąpień aplikacji interfejsu API przy użyciu funkcji skali; wśród innych funkcji.
- Przyciski **uruchamiać** i **zatrzymywać** służą do sterowania aplikacji interfejsu API.
- Podczas aktualizacji produktów zostały źródłowych plików używanych przez aplikację interfejsu API, możesz kliknąć pozycję **Aktualizuj** , aby korzystać z najnowszych wersji. Na przykład jeśli istnieje aktualizacji zabezpieczeń udostępnionych przez firmę Microsoft lub poprawki, klikając automatycznie **zaktualizować** aktualizacje aplikacji interfejsu API do uwzględnienia tej poprawki.
- Umożliwia **Zmianę planowanie** uaktualnienia lub na podstawie zużycia danych aplikacji interfejsu API na starszą wersję. Ta funkcja umożliwia również zobacz Korzystanie z danych.
- Po utworzeniu łącznik, który zawiera tabele, takich jak łącznik SQL Opcjonalnie można wprowadzić nazwę tabeli, aby nawiązać połączenie. Schemat oparta na tabeli jest automatycznie utworzone i dostępne po kliknięciu przycisku **Pobierz schematów**. Ten schemat pobrany umożliwia następnie tworzenie przekształcenie lub mapy.

## <a name="change-your-connector-or-api-configuration-values-you-entered"></a>Zmienić łącznik lub wprowadzonych wartości konfiguracji interfejsu API

Po utworzono skonfigurowane lub wybrany łącznik wbudowany, można zmienić wartości, które zostały wprowadzone. Na przykład jeśli skonfigurowano łącznika SQL i chcesz zmienić nazwę programu SQL Server lub nazwa tabeli, możesz to zrobić w karta aplikacji interfejsu API dla tego łącznika.

Dostępne są następujące kroki:

1. Otwórz łącznik lub aplikacji interfejsu API. Po wykonaniu, zostanie wyświetlona karta interfejsu API aplikacji.
2. W **Essentials**kliknij hiperłącze w obszarze właściwości hosta. Hiperłącze jest nazwana takiego jak *slackconnector* lub *microsoftsqlconnector123*:

    ![][apiapphost]

3. Karta hosta aplikacji interfejsu API wybierz **Ustawienia**. W karta Ustawienia wybierz pozycję **Ustawienia aplikacji**. Konfiguracja wartości są wyświetlane w obszarze **Ustawienia aplikacji**:

    ![][hostsettings]

4. Kliknij pozycję Ustawienia, które chcesz zmienić, wpisz nową wartość i **zapisać** wprowadzone zmiany.


## <a name="install-the-hybrid-connection-manager---optional"></a>Instalowanie Menedżera połączeń hybrydowym — opcjonalne

![][hcsetup]

Hybrydowe Connection Manager umożliwia możliwość nawiązywania połączenia z lokalnym systemem, takich jak program SQL Server lub SAP. Ten łączność hybrydowych używa Bus usługi Azure nawiązania połączenia i kontrolować zabezpieczenia między Azure zasobów i zasobów lokalnych.

Zobacz [Korzystanie z Menedżera połączeń hybrydowych w usłudze Azure aplikacji](app-service-logic-hybrid-connection-manager.md).

> [AZURE.NOTE] Hybrydowe Connection Manager jest wymagana tylko wtedy, gdy łączysz się zasobów lokalnych za zaporą. Jeśli nie łączysz się z systemu lokalnego, hybrydowych Connection Manager nie może być widoczne w swojej karta Łącznik.

## <a name="monitor-the-performance"></a>Monitorowanie wydajności
Wskaźniki są wbudowanych funkcji i dołączone do każdej aplikacji interfejsu API tworzenia. Te wskaźniki są specyficzne dla swojego interfejsu API aplikacji obsługiwany w Azure. Przykładowe wskaźniki:

![][monitoring]

Można:

- Wybierz pozycję **żądania i błędów** dodawać różne wskaźniki tym powszechnie znane kody błędów HTTP, takich jak 200, 400 i 500 kody stanu HTTP. Można również Zobacz czasy odpowiedzi, zobacz, ile żądania są kierowane do aplikacji interfejsu API i zobacz ilości danych moduł i ilości danych składa się z. Na podstawie wydajności metryk, możesz utworzyć alerty e-mail Jeśli metryki przekracza próg wybrane.
- Widać **Użycie** **Procesora** jest używany przez aplikację interfejsu API, przejrzyj bieżącego **Przydział użycia** w MB i zobacz Korzystanie z usługi danych maksymalna zgodnie z poziomu koszt. **Poświęcają szacowany** mogą ułatwić podjęcie potencjalne koszty uruchamiania aplikacji interfejsu API.
- Wybierz pozycję **procesów** , aby otworzyć Eksploratora proces. Spowoduje to wyświetlenie wystąpień usługi sieci web i ich właściwości, w tym użycie pamięci i liczba wątku.

Za pomocą tych narzędzi, można określić, jeśli Plan usługi aplikacji powinny być konfigurowania lub skalowany dół, w zależności od potrzeb firmy. Te funkcje są wbudowane do portalu z żadnych dodatkowych narzędzi wymagane.

## <a name="control-the-security"></a>Kontrola zabezpieczeń

Interfejs API aplikacje za pomocą oparta na rolach zabezpieczeń. Role te dotyczą całej środowisko Azure, takich jak aplikacje interfejsu API i inne zasoby Azure. Role są następujące:

Rola | Opis
--- | ---
Właściciel | Pełny dostęp do środowiska zarządzania i może udzielić dostępu do pozostałych użytkowników lub grup.
Trybu współautora | Mieć pełny dostęp do środowiska zarządzania. Nie można udzielić dostępu do pozostałych użytkowników lub grup.
Czytnik | Można wyświetlić wszystkie zasoby z wyjątkiem hasła.
Administrator dostępu użytkowników | Można wyświetlić wszystkie zasoby, tworzenie i zarządzanie rolami i tworzenie Zarządzanie obsługują bilety.

Zobacz [Kontrola dostępu oparta na rolach w portalu Microsoft Azure](../active-directory/role-based-access-control-configure.md).

Można łatwo dodawać użytkowników i przydzielanie ich określonych ról do aplikacji interfejsu API. Portalu zawiera użytkowników, którzy mają dostęp i ich przypisanych ról:

![][access]  

- Wybierz **użytkowników** Dodawanie użytkownika i przypisz rolę oraz usuwanie użytkownika.
- Wybierz **role** , aby wyświetlić wszystkich użytkowników w konkretnej roli dodać użytkownika do roli i usunąć użytkownika z roli.


## <a name="more-good-stuff"></a>Więcej elementów dobre
- Wybierz **definicji interfejsu API** , aby otworzyć plik Swagger tworzone automatycznie dla określonej aplikacji interfejsu API.
- Wybierz **zależności** , aby wyświetlić pliki wymagane przez aplikację interfejsu API. Jeśli korzystasz z systemu SAP łącznik, na przykład zainstalować niektóre dodatkowe pliki na lokalnym hybrydowych Connection Manager. Zależności są wyświetlane w swojej karta aplikacji interfejsu API.

>[AZURE.IMPORTANT] Po otwarciu właściwości aplikacji interfejsu API i spójrz na **Essentials**, istnieją **hosta** i **bramy** łącza, które otwiera się nowe karty:
>
> ![][host]
>
>Poniższe właściwości są specyficzne dla witryny sieci Web obsługującego aplikacji interfejsu API. Podczas korzystania z wbudowanych aplikacji interfejsu API lub łącznik, większość z tych właściwości naprawdę nie można stosować i zaleca się, że nie możesz zaktualizować tych właściwości. Jeśli utworzono aplikacji interfejsu API programu Visual Studio i wdrożyć go do subskrypcji usługi Azure, można użyć karty hosta i bramy. <br/><br/>


>[AZURE.NOTE] Aby rozpocząć pracę z aplikacjami logiczny przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj logiczny aplikacji](https://tryappservice.azure.com/?appservice=logic). Możesz utworzyć aplikację logiczny krótkotrwałe starter. Nie kart kredytowych wymagane i nie zobowiązań.

## <a name="read-more"></a>Dowiedz się więcej

[Monitorowanie aplikacji warunków logicznych](app-service-logic-monitor-your-logic-apps.md)<br/>
[Łączniki i interfejsu API listy aplikacji w aplikacji usługi](app-service-logic-connectors-list.md)<br/>
[Kontrola dostępu oparta na rolach w portalu Microsoft Azure](../active-directory/role-based-access-control-configure.md)<br/>
[Hybrydowe Connection Manager przy użyciu Azure aplikacji usługi](app-service-logic-hybrid-connection-manager.md)


<!--Image references-->
[browse]: ./media/app-service-logic-monitor-your-connectors/browse.png
[settings]: ./media/app-service-logic-monitor-your-connectors/settings.png
[hcsetup]: ./media/app-service-logic-monitor-your-connectors/hcsetup.png
[monitoring]: ./media/app-service-logic-monitor-your-connectors/monitoring.png
[access]: ./media/app-service-logic-monitor-your-connectors/access.png
[host]: ./media/app-service-logic-monitor-your-connectors/host.png
[hostsettings]: ./media/app-service-logic-monitor-your-connectors/hostsettings.png
[apiapphost]: ./media/app-service-logic-monitor-your-connectors/apiapphost.png
