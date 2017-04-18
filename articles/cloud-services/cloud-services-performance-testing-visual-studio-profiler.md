<properties 
    pageTitle="Profilowanie usługi w chmurze lokalnie w emulatorze obliczeń | Microsoft Azure" 
    services="cloud-services"
    description="Badanie problemów z wydajnością w usług w chmurze przy użyciu programu Visual Studio" 
    documentationCenter=""
    authors="TomArcher" 
    manager="douge" 
    editor=""
    tags="" 
    />

<tags 
    ms.service="cloud-services" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/30/2016" 
    ms.author="tarcher"/>

# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>Testowanie wydajność usługi w chmurze lokalnie w emulatorze Azure obliczeń przy użyciu programu Visual Studio

Szeroką gamę narzędzi i technik są dostępne dla testów wydajności usług w chmurze.
Podczas publikowania usługi w chmurze Azure, możesz mieć Visual Studio zbierać dane profilowania, a następnie analizować lokalnie, jako opisano w [profilowania aplikacji Azure][1].
Umożliwia także diagnostyki do śledzenia różnych liczniki wydajności, zgodnie z opisem w [Używanie liczników wydajności platformy Azure][2].
Można także profilu aplikacji lokalnie w emulatorze obliczeń przed wdrożeniem jej w chmurze.

W tym artykule opisano, jak przy próbkowaniu Procesor metodę profilowanie, które można wykonać lokalnie w emulatorze. Procesor pobierania jest metoda profilowania, który nie jest bardzo inwazyjnych. W odstępach wyznaczonych pobierania programu pobiera migawkę stosu połączenia. Dane są zbierane przez czas i wyświetlane w raporcie. Ta metoda profilowanie zwykle wskazuje, gdzie w praktyce intensywnie aplikacji większość pracy Procesora jest wykonywana jest.  Uzyskasz możliwość skoncentrować się na "ścieżkę dostępu" miejsce, w którym aplikacja spędza najwięcej czasu.



## <a name="1-configure-visual-studio-for-profiling"></a>1: Konfigurowanie programu Visual Studio dla profilowania

Najpierw istnieje kilka opcji konfiguracji programu Visual Studio, które mogą być przydatne, gdy profilowania. Rozsądnie profilowania raportów, musisz symboli (pliki .pdb) dla swojej aplikacji, a także symbole dla bibliotek systemu. Zapewne zechcesz się upewnić, że dokumentacja dotycząca serwery dostępne symbol. Aby to zrobić, w menu **Narzędzia** w programie Visual Studio, wybierz pozycję **Opcje**, a następnie wybierz pozycję **Debugowanie**, następnie **symbole**. Upewnij się, że serwery Microsoft Symbol jest wyświetlany w obszarze **Symbol lokalizacjami plików (.pdb)**.  Można także odwoływać http://referencesource.microsoft.com/symbols, który może być pliki dodatkowych symboli.

![Opcje symbolu][4]

W razie potrzeby można uprościć raportów, generowane przez ustawienie tylko moje kodu programu. Tylko moje kod włączone stosy wywołań funkcji są uproszczone tak, aby całkowicie wewnętrznych dla bibliotek i .NET Framework połączenia są ukryte przed raportów. W menu **Narzędzia** wybierz pozycję **Opcje**. Następnie rozwiń węzeł **Narzędzi wydajności** , a następnie wybierz pozycję **Ogólne**. Zaznacz pole wyboru **Włącz tylko moje**kodu profiler w raportach.

![Tylko moje opcje][17]

Za pomocą tych instrukcji z istniejącego projektu lub nowego projektu.  Jeśli tworzysz nowy projekt, aby wypróbować techniki opisane poniżej projektu C# **Azure usługi w chmurze** i wybierz przycisk **Ról w sieci Web** i **Roli pracownika**.

![Role usługi w chmurze Azure w projekcie][5]

Na przykład celów, dodawanie kodu do projektu, na który zajmuje dużo czasu i zaprezentowano kilka problem z wydajnością oczywiste. Do projektu roli Pracownik, na przykład dodać następujący kod:

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

Zadzwoń kod z metody RunAsync klasy uzyskana RoleEntryPoint roli pracownika. (Ignoruj ostrzeżenia o metody działanie synchroniczne).

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Tworzenie i uruchamianie usługi w chmurze lokalnie bez debugowania (Ctrl + F5) z konfiguracją rozwiązanie, ustaw do **wersji**. Dzięki temu wszystkie pliki i foldery są tworzone lokalnie uruchamiania aplikacji, a zapewnia rozpoczętej wszystkich emulatory. Rozpocznij Interfejs emulatora obliczyć z paska zadań, aby sprawdzić, czy Twoja rola Pracownik jest uruchomiony.

## <a name="2-attach-to-a-process"></a>2: dołączanie do procesu

Zamiast profilowanie aplikacji, uruchamiając go w programie Visual Studio 2010 IDE, należy połączyć proces uruchamiania programu. 

Aby dołączyć programu do procesu, w menu **Analiza** wybierz **Profiler** i **Dołącz/Odłącz**.

![Dołączanie opcji profilu][6]

Znajdź proces WaWorkerHost.exe dla roli pracownika.

![Proces WaWorkerHost][7]

Jeśli folder projektu jest na dysku sieciowym, programu wyświetli monit o podanie innej lokalizacji, aby zapisać profilowania raporty.

 Można także dołączyć do roli sieci web, dołączając do WaIISHost.exe.
W przypadku wielu procesy ról w aplikacji, należy użyć identyfikator procesu, aby je rozróżnić. Identyfikator procesu mogą programowo kwerendy, po zalogowaniu się do obiektu proces. Na przykład po dodaniu tego kodu do metody Run uzyskana RoleEntryPoint klasy w roli zapoznanie się z dziennika w interfejsie użytkownika emulatora obliczyć wiedzieć o procedury, aby nawiązać połączenie.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Aby wyświetlić dziennik, uruchom interfejs emulatora obliczenia.

![Rozpoczynanie emulatora obliczeń interfejsu użytkownika][8]

Otwórz okno konsoli dziennik roli pracownika w Interfejsie emulatora obliczenia, klikając na pasku tytułu okna konsoli. Można zobaczyć identyfikator procesu w dzienniku.

![Identyfikator procesu widoku][9]

Język, który dołączono, wykonaj kroki w interfejsie użytkownika aplikacji (w razie potrzeby) w celu odtworzenia tego scenariusza.

Gdy chcesz zatrzymać profilowania, wybierz pozycję **Zatrzymaj profilowanie** łącze.

![Zatrzymaj profilowanie opcji][10]

## <a name="3-view-performance-reports"></a>3: wyświetlanie raportów dotyczących wydajności

Zostanie wyświetlony raport wydajności aplikacji.

W tym momencie programu przestaje wykonywanie, zapisuje dane w pliku .vsp i wyświetla raportu, który zawiera analizę danych.

![Raport Profiler][11]


Jeśli zostanie wyświetlony String.wstrcpy w ścieżce dostępu, kliknij po prostu Moje kod, aby zmienić widok, aby pokazać tylko kod użytkownika.  Jeśli zostanie wyświetlony String.Concat, spróbuj, naciśnięcie przycisku Pokaż cały kod.

Metoda ZŁĄCZ.teksty i String.Concat podejmowania duża część czas wykonywania powinny być widoczne.

![Analiza raportu][12]

Po dodaniu kodu łączenie ciągów w tym artykule należy wyświetlane ostrzeżenie o na liście zadań, w tym. Można też zobaczyć ostrzeżeniem, że jest nadmierna ilość śmieci jest ze względu na liczbę ciągów, które są tworzone i usuwane.

![Ostrzeżenia wydajności][14]

## <a name="4-make-changes-and-compare-performance"></a>4: wprowadzanie zmian i porównywanie wydajności

Możesz również porównać działanie przed i po zmianie kodu.  Zatrzymanie uruchomionego procesu i edytować kod, aby zamienić operacji łączenia ciągów przy użyciu StringBuilder:

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

Przeprowadź innego wydajności, a następnie porównaj wydajność. W Eksploratorze wydajności Jeśli zostanie uruchomiona znajdują się w tej samej sesji możesz można tylko zaznacz oba raporty, otwórz menu skrótów i wybierz **Porównanie raportów dotyczących wydajności**. Jeśli chcesz porównać przy użyciu polecenia Uruchom w innej sesji wydajności, otwórz menu **Analiza** , a następnie wybierz **Porównanie raportów dotyczących wydajności**. Określ obu plików w oknie dialogowym, które zostanie wyświetlone.

![Porównanie opcji raportów wydajności][15]

Raporty Wyróżnij różnice między dwoma zostanie uruchomiony.

![Porównanie raportu][16]

Gratulacje! Pobrano rozpoczęciu z okna.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

- Upewnij się, są profilowanie kompilacji wersji i uruchomienie bez debugowania.

- Jeśli opcja Dołącz/Odłącz nie jest włączona w Profiler menu, uruchom Kreatora wydajności.

- Interfejs emulatora obliczyć umożliwia wyświetlanie stanu aplikacji. 

- Jeśli masz problemy z uruchamianiem aplikacji w emulatorze lub dołączanie okna, zamykanie emulatora obliczeń w dół, a następnie uruchom go ponownie. Jeśli to nie rozwiąże problemu, spróbuj ponownie uruchomić. Ten problem może wystąpić, użycie emulatora obliczyć zawieszenia i usuwać uruchomionego wdrożenia.

- Jeśli zastosowano jakichkolwiek poleceń profilowania z poziomu wiersza polecenia, szczególnie ustawień globalnych upewnij się wywołana VSPerfClrEnv /globaloff i zamknij VsPerfMon.exe.

- Jeśli podczas pobierania próbek, zostanie wyświetlony komunikat "PRF0025: Brak danych zostały pobrane," Sprawdź, czy Proces dołączone do ma aktywności Procesora. Aplikacje, które są bezczynny obliczeniowa może nie spełnić wszystkie dane przy próbkowaniu.  Istnieje także możliwość, że proces zakończony przed wykonano wszystkie próbki. Sprawdź, czy metody Run dla roli, które są profilowania nie kończy.

## <a name="next-steps"></a>Następne kroki

Instrumentacja Azure plików binarnych w emulatorze nie jest obsługiwana w programu Visual Studio, ale jeśli chcesz przetestować przydzielanie pamięci, można wybierz tę opcję, gdy profilowania. Możesz również wybrać współbieżności profilowanie, który pomaga stwierdzić, czy wątków marnowaniem czasu uczestniczących w zawodach blokady lub warstwa profilowanie interakcji, która ułatwia śledzenie problemów z wydajnością podczas interakcji między warstwy aplikacji, najczęściej między warstwy danych i roli pracownika.  Można wyświetlić zapytania bazy danych, które generuje aplikacji i usprawnić korzystanie z bazy danych przy użyciu profilowania danych. Aby dowiedzieć się, jak profilowanie interakcji warstwa, zobacz wpis w blogu [Instruktaż: za pomocą programu interakcji warstwa w Visual Studio 2010 System zespołu][3].



[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
 