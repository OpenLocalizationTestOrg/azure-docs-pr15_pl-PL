<properties
   pageTitle="Azure terminologia wykaz danych | Microsoft Azure"
   description="Ten artykuł zawiera wprowadzenie do pojęć i terminów używanych w dokumentacji Azure wykazu danych."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-terminology"></a>Azure terminologia wykazu danych

## <a name="catalog"></a>Wykazu

Wykaz danych Azure to repozytorium metadanych opartej na chmurze danych można rejestrować źródeł i danych. Wykaz służy jako miejsce przechowywania centralnej strukturalnych metadanych wyodrębnionych z źródeł danych i metadanych opisowy dodane przez użytkowników.

## <a name="data-source"></a>Źródła danych

Źródło danych jest system lub kontener zarządzającą aktywów danych. Jako przykład można wymienić bazy danych programu SQL Server, bazy danych programu Oracle, baz danych programu SQL Server Analysis Services (tabelarycznym lub wielowymiarowych) i serwery usług SQL Server Reporting Services.

## <a name="data-asset"></a>Trwały danych

Dane są zawarte w źródeł danych, które mogą być rejestrowane w katalogu obiekty. Jako przykład można wymienić tabele programu SQL Server i widoki, Oracle tabele i widoki, SQL Server Analysis Services miary, wymiary i kluczowych wskaźników wydajności i raportów usług SQL Server Reporting Services.

## <a name="data-asset-location"></a>Lokalizacja danych składników majątku

Wykaz przechowuje lokalizację źródła danych lub trwały danych, które mogą być używane do łączenia się ze źródłem przy użyciu aplikacji klienckiej. Format i uzyskać szczegółowe informacje o lokalizacji różnią się w zależności od typu źródła danych. Na przykład tabeli programu SQL Server można określić nazwy czterech części — nazwę serwera, nazwę bazy danych, nazwa schematu, nazwy obiektu — podczas Reporting Services raport programu SQL Server można rozpoznać po jego adres URL.

## <a name="structural-metadata"></a>Strukturalne metadanych

Metadane strukturalne są metadane wyodrębnionych z źródło danych, które opisuje strukturę zbiór danych. Ta opcja uwzględnia lokalizacji aktywów, jego nazwa obiektu i typ i dodatkowe właściwości specyficzne dla typu. Na przykład strukturalnych metadanych dla tabel i widoków zawiera nazwy i typy danych dla kolumny obiektu.

## <a name="descriptive-metadata"></a>Opisowy metadanych

Metadane opisowe są metadane opisujące cel lub przeznaczenie zbiór danych. Zazwyczaj opisową metadanych zostanie dodany przez wykaz użytkowników za pomocą portalu Azure wykazu danych, ale go również można wyodrębnić z źródła danych podczas rejestrowania. Na przykład narzędzie do wykazu danych Azure rejestracji będzie wyodrębnić opisy z właściwości Opis w usług SQL Server Analysis Services i SQL Server Reporting Services i z [ms_description właściwość rozszerzona](https://technet.microsoft.com/library/ms190243.aspx) w bazach danych programu SQL Server, jeśli te właściwości zostały wypełnione wartościami.

## <a name="request-access"></a>Żądanie dostępu

Metadane opisowe zbiór danych mogą zawierać informacje dotyczące żądania dostępu do zawartości danych lub źródła danych. Te informacje są prezentowane z lokalizacją zawartości danych i może zawierać jedną lub więcej z następujących opcji:

- Adres e-mail użytkownika lub zespołu, które są odpowiedzialne za udzielanie dostępu do źródła danych.
- Adres URL udokumentowane procesu, który użytkownicy należy wykonać, aby uzyskać dostęp do źródła danych.
- Adres URL tożsamości i access narzędzia do zarządzania (takich jak Microsoft Menedżer tożsamości) używanego do uzyskiwania dostępu do źródła danych.
- Wpis niezależnej w tym artykule opisano, jak użytkownicy mogą uzyskać dostęp do źródła danych.

## <a name="preview"></a>Podgląd

Podgląd w wykazie danych Azure jest migawkę maksymalnie 20 rekordów, które można wyodrębnić ze źródła danych podczas rejestrowania i przechowywane w wykazie metadanymi trwałego danych. Podgląd może ułatwić użytkowników, którzy wykrywania zbiór danych lepiej zrozumieć jej funkcji i cel. Innymi słowy są wyświetlane dane przykładowe można korzystniejsze niż są wyświetlane tylko nazwy kolumn i typy danych.
Podgląd są obsługiwane tylko dla tabel i widoków, a następnie należy jawnie wybrać przez użytkownika podczas rejestrowania.

## <a name="data-profile"></a>Profil danych

Profil danych w wykazie danych Azure jest migawki tabeli i poziomu kolumny metadanych dotyczących zawartości zarejestrowane dane, które mogą być wyodrębnionych ze źródła danych podczas rejestrowania i przechowywane w wykazie metadanymi trwałego danych. Profil danych ułatwia użytkowników, którzy wykrywania zbiór danych lepiej zrozumieć jej funkcji i cel. Podobnie jak podglądy profile danych musi być jawnie zaznaczone przez użytkownika podczas rejestrowania.

> [AZURE.NOTE] Wyodrębnianie danych profilu może być operacji kosztów dla dużych tabel i widoków i może być znacznie zwiększyć czas wymagany do rejestrowania źródła danych.

## <a name="user-perspective"></a>Perspektywy użytkownika.

W wykazie danych Azure każdy użytkownik zapewnia opisowy metadanych dla trwałego zarejestrowane dane. Każdy użytkownik ma perspektywy różnią się w danych i jego zastosowania. Na przykład administrator odpowiedzialny za serwer może dostarczyć szczegóły Umowa dotycząca poziomu usług (SLA) lub tworzenie kopii zapasowych; zarządcy danych może zapewnić łączy się z dokumentacją dla firmy przetwarza obsługiwane danych; i analityk może dostarczyć opis w zakresie, które są najlepiej odpowiadający innych analityków i które mogą być najbardziej przydatne dla tych użytkowników, którzy potrzebują odnajdowanie i rozumienie danych.

Każdy z perspektywy te są założenia cenne i z wykazem danych Azure każdy użytkownik może dostarczać informacje znaczącego, gdy wszyscy użytkownicy mogą korzystać z tych informacji do zrozumienia danych i jej cel.

## <a name="expert"></a>Eksperta

Ekspertem jest wykryto posiadanie informacji "ekspertów" perspektywa zbiór danych użytkownika. Dowolny użytkownik może dodać siebie lub innego użytkownika jako ekspert dla środka trwałego. Są wyświetlane jako ekspert nie przekazuj żadnych dodatkowych uprawnień w wykazie danych Azure; Umożliwia użytkownikom łatwe znajdowanie tych perspektyw, które są najprawdopodobniej jest przydatne podczas przeglądania metadanych opisową środka trwałego.

## <a name="owner"></a>Właściciel

Właścicielem jest użytkownik, który ma dodatkowe uprawnienia do zarządzania zbiór danych w wykazie danych Azure. Użytkownicy mogą przejmować własności aktywów zarejestrowane dane, a właściciele można dodać innym użytkownikom jako Współtworzenie właściciele. Aby uzyskać więcej informacji, zobacz [jak zarządzać danych składników majątku](data-catalog-how-to-manage.md)  
> [AZURE.NOTE] Własności i zarządzania są dostępne tylko w standardowej wersji programu Azure wykazu danych.

## <a name="registration"></a>Rejestracja

Rejestracja jest czynność wyodrębnianie metadanych zawartości danych ze źródła danych i kopiowanie go z usługą Azure wykazu danych. Składniki majątku danych, które zostały zarejestrowane następnie można dodawać adnotacje i wykryte.

## <a name="see-also"></a>Zobacz też

- [Co to jest Azure wykazu danych?](data-catalog-what-is-data-catalog.md) — W tym artykule omówiono usługi Azure wykazu danych, wartości, które znajdują się w nim i scenariusze, które obsługuje.

- [Rozpoczynanie pracy z wykazem danych Azure](data-catalog-get-started.md) — w tym artykule znajdują się samouczek zakończenia do końca, w którym pokazano, jak za pomocą wykazu danych Azure dla odnajdowania źródła danych.  
