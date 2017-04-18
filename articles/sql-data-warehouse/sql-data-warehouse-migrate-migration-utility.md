<properties
   pageTitle="Migrowanie: Danych magazynu narzędzie migracji | Microsoft Azure"
   description="Migrowanie do magazynu danych SQL."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="data-warehouse-migration-utility-preview"></a>Narzędzie migracji magazynu danych (wersja Preview)

> [AZURE.SELECTOR]
- [Pobierz narzędzie migracji][]

Narzędzie migracji magazynu danych to narzędzie przeznaczone do migracji schematów i danych z programu SQL Server i bazy danych SQL Azure do magazynu danych SQL Azure. Podczas migracji schematu narzędzie map automatycznie odpowiedni schemat ze źródła do miejsca docelowego. Po przeprowadzeniu migracji schematu narzędzia umożliwia przenoszenie danych z automatycznie wygenerowanego skryptów.

Oprócz migracji danych i schematu to narzędzie oferuje opcję, aby wygenerować raporty zgodności, które podsumowywanie niezgodności między wystąpieniami źródłowe i docelowe, które usprawniony migracji.

## <a name="get-started"></a>Rozpoczynanie pracy
Jako wymagania wstępne dotyczące instalacji konieczne będzie narzędzie wiersza polecenia BCP na uruchamianie skryptów migracji i pakietu Office, aby wyświetlić raport zgodności. Po uruchomieniu jest pobierany plik wykonywalny wyświetli monit o Zaakceptuj standardowe umowę licencyjną użytkownika oprogramowania przed narzędzie zostanie zainstalowana.

Ponadto, aby uruchomić Utiliy migracji, konieczne będzie ono po uprawnienia bazy danych, w której chcesz przeprowadzić migrację: tworzenie bazy danych, ZMIEŃ DOWOLNE bazę danych lub dowolny definicji WIDOKU.

### <a name="launching-the-tool-and-connecting"></a>Uruchamianie narzędzia i łączenia
Uruchom narzędzie, klikając ikonę pulpitu, która jest wyświetlana instalacji wpisu. Po otwarciu narzędzia, zostanie wyświetlony monit z strony początkowego połączenia, w którym można wybrać usługi źródłowej i docelowej dla narzędzia do migracji. W tej chwili obsługujemy programu SQL Server i bazy danych SQL Azure jako źródeł i magazynu danych SQL jako miejsca docelowego. Po wybraniu to zostanie wyświetlony monit do nawiązania połączenia z serwerem źródła wypełniania w polu Nazwa serwera i uwierzytelniania, a następnie klikając przycisk "Połącz".

Po uwierzytelnieniu, narzędzie wyświetli listę baz danych, które znajdują się na serwerze, które są połączone z. Wybieranie bazy danych, w której chcesz przeprowadzić migrację, a następnie klikając polecenie na zaznaczonym migracji można rozpocząć migrację.

## <a name="migration-report"></a>Raport migracji
Wybieranie "Sprawdzanie zgodności bazy danych" w narzędziu spowoduje wygenerowanie raportu Podsumowanie wszystkich niezgodności obiektu w bazie danych że wymagane do migrowania. Pełniejsza listę niektóre funkcje programu SQL Server, której nie ma w magazynie danych SQL można znaleźć w naszą [dokumentację migracji][]. Po wygenerować raport można zapisać i otworzyć raport w programie Excel.

Należy pamiętać, że jeśli generowanie schematu migracji, większość problemów określone jako "Object" będzie dostosowany w celu umożliwienia natychmiastowej migracji danych. Przejrzyj zmiany, aby upewnić się, że nie chcesz wprowadzić dodatkowe zmiany przed zastosowaniem w schemacie.

## <a name="migrate-schema"></a>Migrowanie schematu

Po nawiązaniu połączenia Wybieranie 'Migrowanie schemat' wygeneruje skrypt migracji schematu dla wybranych tabel. Porty tego skryptu strukturę tabeli, niezgodnych danych mapy typy formularzy bardziej zgodny i tworzy schematów i poświadczeń zabezpieczeń, jeśli to jest wskazywana przez użytkownika w ustawieniach migracji. Kod mogą być uruchamiane na wystąpienie programu SQL Data Warehouse docelowej, zapisane w pliku, skopiowany do Schowka lub nawet edycji w tekście przed podjęciem dalszych działań.  

Jako określone powyżej, gdy Migrowanie Recenzja schematu migracji zmiany, które narzędzie wprowadził w celu zapewnienia, że należy zapoznać się je.  

## <a name="migrate-data"></a>Migrowanie danych

Przez kliknięcie opcji "Migrowanie dane" istnieje możliwość wygenerowania BCP skrypty powoduje przeniesienie danych najpierw do prostym plików na serwerze, a następnie bezpośrednio do magazynu danych SQL. Zalecamy ten proces przenoszenia niewielkich ilości danych, a jako ponowne próby nie są wbudowane i błędy mogą występować w przypadku utraty połączenia sieciowego. Aby wykonać to, muszą mieć zainstalowanego narzędzia wiersza polecenia BCP i schemat danych musi już zostały utworzone.

Po wypełnieniu powyższych parametrów należy po prostu kliknij procesu migracji i zestaw dwóch pakietów zostanie utworzony do określonej lokalizacji. Uruchamianie pliku eksportu w celu Wyeksportuj dane ze źródła migracji do plików prostych, a następnie uruchom plik importu w celu zaimportowania danych do magazynu danych SQL.

## <a name="next-steps"></a>Następne kroki
Teraz, gdy niektóre dane zostały zmigrowane, zapoznaj się z [opracowywaniu][].

<!--Image references-->

<!--Article references-->
[Dokumentacja dotycząca migracji]: sql-data-warehouse-overview-migrate.md
[można opracowywać]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Pobierz narzędzie migracji]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
