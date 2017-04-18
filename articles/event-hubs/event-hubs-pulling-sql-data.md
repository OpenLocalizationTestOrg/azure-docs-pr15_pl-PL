<properties
   pageTitle="Uzyskanie danych SQL do koncentratorów zdarzenia Azure | Microsoft Azure"
   description="Omówienie koncentratory zdarzenia importowanie z próbki SQL"
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor=""/>

<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="pulling-data-from-sql-into-an-azure-event-hub"></a>Uzyskanie danych z programu SQL do koncentratora zdarzenia Azure

Typowe Architektura aplikacji na potrzeby przetwarzania danych w czasie rzeczywistym wiąże się najpierw naciśnięcie do koncentratora zdarzenia Azure. Może być scenariusz IoT, takich jak monitorowanie ruchu na różnych zbiorników autostrad lub scenariusza gier monitorowania akcje wiele frenzied contestants lub scenariusz dla przedsiębiorstwa, takich jak monitorowanie użytkowanie konstrukcyjnych. W takich przypadkach producenci danych są zazwyczaj zewnętrznych obiektów produkcji szeregu czasowego dane, które należy zbieranie, analizowanie, przechowywanie i czynności, a może mieć zainwestowanego wiele nakładu pracy w odniesieniu do zbudowania infrastruktury dla następujących procesów. Co zrobić można zrobić, jeśli chcesz wprowadzać dane z innej takich jak bazy danych, a nie źródła strumieniowego przesyłania danych, a jej używać w połączeniu z innych danych przesyłanie strumieniowe? Należy rozważyć, czy wielkości liter, której chcesz używać analizy strumieniu Azure, zdalnego Eksplorator danych (RDX) lub inne narzędzie do analizowania i działania w powoli zmieniania danych z programu Microsoft Dynamics AX lub systemie zarządzania urządzeniami niestandardowe? Aby rozwiązać ten problem, zapisywane i otwórz-powierzając jej ich konserwację małych chmury próbce, które można modyfikować i wdrażanie pobierają dane z tabeli SQL i przekazać je do koncentratora Azure wydarzenie ma zostać użyte jako dane wejściowe w aplikacjach analitycznych za. Okazuje się, że to rzadkich scenariusz i przeciwieństwem zwykle czego koncentratora zdarzenia. Jednak jeśli okaże się, w sytuacji, gdy jest to co należy zrobić, możesz znaleźć kod w galerii przykładów Azure, [w tym miejscu](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-import-from-sql/).  

Należy zauważyć, że kod w tym przykładzie jest po prostu tej próbki. To **nie** mają być aplikacją produkcji i prób nie wprowadzono był nadaje się do użytku w takim środowisku — to stricly DIY przykład oparte na Deweloper. Należy przejrzeć różnych rodzajów zabezpieczeń, wydajność i funkcjonalność i czynników kosztowych przed rozpoczęciem na architekturze zakończenia do końca.

## <a name="application-structure"></a>Struktury aplikacji

Aplikacja jest zapisywany w języku C#, a plik readme.md w próbce zawiera wszystkie informacje, które chcesz zmodyfikować, tworzenie i publikowanie aplikacji. W poniższych sekcjach przedstawiono wysokiego poziomu omówienie działanie aplikacji.

Możemy uruchomić przy założeniu, że masz dostęp do tabeli platformy SQL Azure. Również musisz skonfigurowano koncentratora zdarzenia Azure i znasz połączenie ciągu wymaganego do nich dostęp.

Po uruchomieniu rozwiązanie SqlToEventHub, odczytuje pliku konfiguracji (App.config), aby uzyskać liczbę czynności opisane w pliku readme.md. Są to dość oczywiste, takie jak nazwa tabeli danych itp, a istnieje potrzeba rehash wyjaśnienia poniżej. 

Gdy aplikacja ma odczytać pliku konfiguracji, go przechodzi w pętli odczytywania tabeli programu SQL naciśnięcie rekordów do koncentratora zdarzenia, a następnie oczekujące interwału wstrzymania zdefiniowane przez użytkownika przed jego wykonanie nowa ponownie. Kilka rzeczy są warto zauważyć:

1. Aplikacja opiera się na założeniu, że tabela SQL jest aktualizowany przy niektórych proces zewnętrzny, a chcesz wysłać wszystkie i tylko aktualizacje do koncentratora zdarzenia.
2. Tabela SQL musi mieć pola, które zawiera unikatowe oraz zwiększenie liczby, na przykład numer rekordu. Może to być tak proste jak pole o nazwie "Identyfikator" lub pozostałej zawartości, która jest zwiększany jako niezależnie od aktualizacji tej bazy danych dodaje rekordy, takie jak "Creation_time" lub "Sequence_number". Aplikacja notatek i zapisać wartość tego pola w każdej iteracji. W każdej kolejnej przekazujące pętli aplikacji zasadniczo kwerendy tabeli dla wszystkich rekordów, w którym wartość tego pola przekracza wartość go pokazano podczas ostatniego za pośrednictwem pętli. Wywołania ostatnią wartość "przesunięcia".
3. Aplikacja tworzy tabelę "TableOffsets" podczas uruchamiania, aby przechowywać przesunięcie. Tabela zostanie utworzona i kwerendy, którą "CreateOffsetTableQuery" zdefiniowane w pliku konfiguracji. 
4. Istnieje kilka kwerend używane do pracy z tabelą przesunięcie zdefiniowane w pliku konfiguracji jako "OffsetQuery", "UpdateOffsetQuery" i "InsertOffsetQuery". Nie należy zmieniać tych.
5. Na koniec kwerendy, którą "DataQuery" zdefiniowane w pliku konfiguracji jest kwerendę, która zostanie uruchomiona aby uwzględniał rekordy z tabeli SQL. Jest obecnie ograniczone do góry 1000 rekordów w każdej przekazujące pętli do optymalizacji — Jeśli na przykład 25 000 rekordy zostały dodane do bazy danych od ostatniej kwerendy, może upłynąć trochę czasu, aby wykonać kwerendę. Ograniczając kwerendy do 1000 rekordów każdym, kwerendy są szybciej. Wybieranie górnej 1000 prosty umieszcza kolejnych partie 1000 rekordów do koncentratora zdarzenia.    

## <a name="next-steps"></a>Następne kroki

Aby wdrożyć rozwiązanie, klonowanie lub pobrać aplikację SqlToEventHub, edytowania pliku App.config, jego tworzenia i na końcu opublikować go. Po opublikowaniu aplikacji, zobacz uruchomiony w portalu klasyczny Azure w obszarze usług w chmurze, a monitorować zdarzenia przychodzących na Twoim Centrum zdarzenia. Należy zauważyć, że częstotliwość zależą od dwóch rzeczy: częstotliwości aktualizacji tabelę SQL i interwał wstrzymania określone w pliku konfiguracji dla aplikacji.