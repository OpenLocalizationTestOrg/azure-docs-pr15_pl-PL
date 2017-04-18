<properties
   pageTitle="Instalowanie programu Visual Studio i SSDT dla magazynu danych SQL | Microsoft Azure"
   description="Instalowanie programu Visual Studio i narzędzia rozwoju programu SQL Server (SSDT) dla magazynu danych Azure SQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>Instalowanie programu Visual Studio 2015 i SSDT dla magazynu danych SQL

Aby opracowywania aplikacji dla magazynu danych SQL, zaleca się przy użyciu programu Visual Studio 2015 z najbardziej aktualną wersję narzędzia danych programu SQL Server (SSDT).  Obsługiwane jest też Visual Studio 2013 aktualizacji 5 z SSDT zgodności z poprzednimi wersjami.  

Przy użyciu programu Visual Studio z SSDT umożliwia graficzne Eksplorowanie tabel, widoków, procedur składowanych i wiele więcej obiektów w magazynie danych SQL, a także uruchamianie kwerend za pomocą Eksploratora obiektu programu SQL Server.

> [AZURE.NOTE] SQL Data Warehouse nie obsługuje jeszcze Visual Studio bazy danych projektów.  Ta funkcja zostanie dodany w przyszłej wersji.

## <a name="step-1-install-visual-studio-2015"></a>Krok 1: Instalowanie programu Visual Studio 2015 r.

Skorzystaj z poniższych łączy, aby pobrać i zainstalować program Visual Studio 2015 r. Jeśli masz już Visual Studio 2013 lub 2015 zainstalowany, możesz przejść do kroku 2, zainstaluj SSDT.

1. [Pobieranie programu Visual Studio 2015 r][].
2. Prowadnicy [Instalowania programu Visual Studio][] w witrynie MSDN i wybierz pozycję domyślne konfiguracje.

## <a name="step-2-install-ssdt"></a>Krok 2: Instalowanie SSDT

Aby zainstalować SSDT programu Visual Studio po prostu sprawdzanie aktualizacji SSDT z poziomu programu Visual Studio, wykonując następujące kroki.

1. W programie Visual Studio kliknij menu **Narzędzia** / **rozszerzenia i aktualizacje...**  /  **Aktualizacji**
2. Wybierz **Aktualizacje produktów** , a następnie poszukaj **Narzędzie Microsoft SQL Server Update dla bazy danych**

Aktualizacja nie zostanie znaleziony, powinien mieć najnowszej zainstalowanej wersji.  Aby potwierdzić, SSDT jest zainstalowany, kliknij na **Pomoc** / **O Microsoft Visual Studio** i poszukaj SQL Server Data Tools na liście.  Najnowsza wersja pakietu SSDT jest 14.0.60525.0.  Jeśli opcja zainstalowania nie jest dostępna z programu Visual Studio, można też kliknąć mogą odwiedzić stronę [SSDT Pobierz][] , aby pobrać i zainstalować SSDT ręcznie.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy masz najnowszą wersję pakietu SSDT, możesz przystąpić do [łączenia][] do magazynu danych programu SQL.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[Nawiązywanie połączenia]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Pobieranie programu Visual Studio 2015 r.]: https://www.visualstudio.com/downloads/
[Instalowanie programu Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[Plik do pobrania SSDT]: https://msdn.microsoft.com/library/mt204009.aspx
