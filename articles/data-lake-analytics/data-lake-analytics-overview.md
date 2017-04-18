<properties 
   pageTitle="Omówienie analizy Microsoft Azure danych Lake | Azure" 
   description="Analizy Lake danych to usługa obliczeń Azure Big Data, która pozwala na dysk firmy przy użyciu wnioski uzyskane na podstawie danych w chmurze, niezależnie od tego, gdzie jest i bez względu na jego rozmiar przy użyciu danych. Analizy Lake danych umożliwia to w najłatwiejszym, możliwy sposób najbardziej skalowalna i najbardziej ekonomicznych. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Omówienie analizy danych Lake bazy wiedzy Microsoft Azure

## <a name="what-is-azure-data-lake-analytics"></a>Co to jest Azure danych Lake analizy?

Azure analizy Lake danych to nowa usługa, utworzoną w celu ułatwienia analizy danych duży. Usługa pozwala skupić się na pisania, uruchomiony i zarządzanie zadaniami, zamiast operacyjnym rozkładana infrastruktury. Zamiast wdrażania, konfigurowania i dostosowywania sprzęt pisania kwerend w celu przekształcenia danych i wyodrębnić przydatne wnioski. Usługi analizy mogą obsługiwać zadania dowolnego skali błyskawicznie, po prostu ustawiając wybierania numerów dla power, ile potrzebujesz. Tylko płatność dla zadania gdy jest uruchomiony, dzięki czemu efektywne pod względem kosztów. Usługa analizy obsługuje usługi Azure Active Directory pozwala po prostu Zarządzanie dostępu i ról zintegrowany z systemem tożsamości lokalnego. Zawiera również U-SQL, w języku, który łączy zalety SQL wyrazisty możliwości kod użytkownika. Środowisko uruchomieniowe rozłożone skalowalna U SQL umożliwia efektywne analizowania danych w magazynie i na serwerach SQL Azure, bazy danych SQL Azure i magazynu danych SQL Azure.


## <a name="key-capabilities"></a>Kluczowe funkcje

- **Dynamiczne skalowania** 

    Analizy Lake danych jest zaprojektowane pod od podstaw na potrzeby cloud skalą i wydajności.  Dynamiczne przepisy zasoby i pozwala wykonywać analizy na terabajtów lub nawet eksabajtów danych. Po zakończeniu zadania, jej okręgów szczegółów zasobów automatycznie, a płacisz tylko w przypadku energii przetwarzania. Jak zwiększyć lub zmniejszyć rozmiar danych przechowywanych lub ilość używanych do uruchamiania, nie musisz ponownie zapisać kod. Pozwala skupić się na logiki biznesowej tylko, a nie na sposobu przetwarzania i przechowywania dużych zestawów danych. 

- **Można opracowywać szybciej, debugowanie i zoptymalizować sprawniejszą za pomocą znanych narzędzi**

    Analizy Lake danych ma ścisła integracja z programem Visual Studio tak, aby za pomocą znanych narzędzi do uruchamiania, debugowania i dostosować swój kod. Wizualizacje zadań U SQL pozwalają na wyświetlanie wykonywania kodu w skali, aby można było łatwo zidentyfikować wydajność, wąskie gardła i Optymalizowanie kosztów. 

- **U-SQL: proste i znanych zaawansowanych i extensible**

    Analizy Lake danych zawiera U-SQL, języka kwerend, który rozszerza znanych, prosta, deklaracyjnych rodzaj SQL z wyrazisty możliwości C#. Języka U SQL jest oparty na tym samym obsługi rozłożone uprawnień systemy duży danych w programie Microsoft. Miliony deweloperów SQL i .NET można teraz przetwarzać i analizować wszystkie swoje dane w zakresie umiejętności, które mają.

- **Bezproblemowo integruje się z inwestycji informatycznych**

    Analizy Lake danych można użyć istniejących inwestycji informatycznych dla tożsamości, zarządzania, zabezpieczenia i magazynowania danych. Upraszcza zarządzanie danymi to i ułatwia Rozszerzanie bieżącej aplikacji danych. Analizy Lake danych jest zintegrowany z usługą Active Directory do zarządzania użytkownikami i uprawnieniami i pochodzi z wbudowanych monitorowania i inspekcja.

- **Przystępnej i niski koszt**

    Analizy Lake danych jest rozwiązaniem efektywne pod względem kosztów do uruchamiania obciążenia duży danych. Płatność na podstawie dla zadania i podczas przetwarzania danych. Nie sprzętu, licencje lub umów pomocy technicznej dotyczącej usługi są wymagane. System jest automatycznie skalowany w górę lub dół zadania rozpoczęciu i zakończeniu, co oznacza nigdy nie Płacenie za dla więcej niż co jest potrzebne. 

- **Współpraca z danymi Azure**

    Analizy Lake danych można pracować z większą liczbą źródeł danych Azure: magazyn obiektów Blob platformy Azure baza danych Azure SQL i oczywiście analizy Lake danych jest specjalnie zoptymalizowane pod kątem pracy z magazynu Lake danych Azure — dostarczając najwyższego poziomu wydajności i przepustowość i parallelization dla Ciebie obciążenia duży danych.

## <a name="see-also"></a>Zobacz też

- Rozpoczynanie pracy
    - [Wprowadzenie do analiz Lake danych za pomocą Azure Portal](data-lake-analytics-get-started-portal.md)
    - [Wprowadzenie do analizy Lake danych przy użyciu programu PowerShell Azure](data-lake-analytics-get-started-powershell.md)
    - [Wprowadzenie do analizy Lake danych przy użyciu zestawu SDK .NET Azure](data-lake-analytics-get-started-net-sdk.md)
    - [Można opracowywać skrypty U SQL za pomocą narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
    - [Wprowadzenie do języka Azure danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md)
    
- U SQL i rozwój
    - [Wprowadzenie do języka Azure danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md)
    - [Użyj funkcji okna U SQL Azure danych Lake analizy zadań](data-lake-analytics-use-window-functions.md)
    - [Można opracowywać U SQL operatorów zdefiniowanych przez użytkownika dla zadań analizy Lake danych](data-lake-analytics-u-sql-develop-user-defined-operators.md)
    
- Zarządzanie
    - [Zarządzanie analiz Lake danych Azure za pomocą Azure Portal](data-lake-analytics-manage-use-portal.md)
    - [Zarządzanie analiz Lake danych Azure za pomocą programu PowerShell Azure](data-lake-analytics-manage-use-powershell.md)
    - [Monitorowanie i rozwiązywanie problemów z Azure danych Lake analizy zadań przy użyciu Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
    - [Uzyskiwanie dostępu do dzienników Diagnostyka analiz Lake danych Azure](data-lake-analytics-diagnostic-logs.md)

- Samouczek zakończenia do końca
    - [Interaktywne podręczniki przedstawiają analizy Lake danych Azure za pomocą](data-lake-analytics-use-interactive-tutorials.md)
    - [Analizowanie dzienniki witryny sieci Web przy użyciu analizy Lake danych Azure](data-lake-analytics-analyze-weblogs.md)

- Trafić co sądzisz
    - [Komentarz zaległości naszą dokumentację](data-lake-analytics-documentation-backlog.md)
    - [Przesyłanie żądania funkcji](http://aka.ms/adlafeedback)
    - [Uzyskaj pomoc na forach](http://aka.ms/adlaforums)


