<properties
    pageTitle="Jak przeprowadzić migrację logiki aplikacji do wersji schematu 2015-08-01-Podgląd | Microsoft Azure aplikacji usługi"
    description="Możesz łatwo przeprowadzić migrację aplikacji logiki do najnowszej wersji schematu. Po prostu wykonaj następujące kroki."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Jak przeprowadzić migrację logiki aplikacji do wersji schematu 2015-08-01-preview

Aby przenieść istniejące aplikacji logika nowy schemat, wykonaj następujące czynności:  
1. Otwórz aplikację warunków logicznych w portalu Azure  
2. Kliknij przycisk Aktualizuj schemat:

 ![Ikona interfejsu API][step1]   
Na stronie aktualizacja schematu Wyświetla oraz łącze do dokumentu, które szczegółowo ulepszenia w nowy schemat: ![Ikona interfejsu API][step2]

>[AZURE.NOTE] Po wybraniu **Schematu aktualizacji**automatycznie uruchom kroków migracji i zapewnić wynik kodu. Można to jednak aktualizacji do definicji, upewnij się, że można śledzić dobrych praktyk kodowania, takie jak kontakty przedstawionych w poniższej sekcji **najważniejszych wskazówek** .

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Najważniejsze wskazówki dotyczące migrowania aplikacji logiki do najnowszej wersji schematu:  

- Skopiuj skrypt migrowane do nowej aplikacji logika — nie zastąpić poprzedni, które do momentu zakończeniu testowania i potwierdzone migracją aplikacji działają zgodnie z oczekiwaniami.
- Testowanie sieci logiki aplikacji **przed** wprowadzeniem produkcji
- Po zakończeniu migracji Rozpocznij aktualizowanie aplikacji logika używać [zarządzanych interfejsów API](./apis-list.md) , jeśli to możliwe. Na przykład możesz rozpocząć korzystanie z usługi Dropbox w wersji 2, przypuszczalnym korzystasz z usługi DropBox w wersji 1.


## <a name="whats-next"></a>Co to jest dalej
-  [Dowiedz się, jak ręczne migrowanie aplikacji warunków logicznych](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






