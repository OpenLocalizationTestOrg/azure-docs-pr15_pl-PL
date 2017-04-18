<properties 
    pageTitle="Analizy strumieniu: Obracanie poświadczeń logowania dane wejściowe i wyjściowe | Microsoft Azure" 
    description="Dowiedz się, jak zaktualizować poświadczeń dla analizy strumieniu dane wejściowe i wyjściowe."
    keywords="poświadczenia logowania"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Obracanie poświadczenia logowania dane wejściowe i wyjściowe w zadaniach analizy strumieniu

##<a name="abstract"></a>Abstrakcyjne
Azure analizy strumieniu dzisiaj nie umożliwia zastąpienie poświadczeń na wyjścia podczas uruchamiania zadania.

Podczas analizy strumieniu Azure obsługuje wznawianie zadanie z ostatniej, trzeba udostępnić cały proces zminimalizowanie czasu zwłoki między zatrzymywanie i uruchamianie zadania i obracanie poświadczenia logowania.

##<a name="part-1---prepare-the-new-set-of-credentials"></a>Część 1: przygotowywanie nowy zestaw poświadczeń:
Ta część dotyczy dane wejściowe i wyjściowe:

* Magazyn obiektów blob
* Koncentratory zdarzenia
* Baza danych SQL
* Magazyn tabel

Dla innych dane wejściowe i wyjściowe przejdź do części 2.

###<a name="blob-storagetable-storage"></a>Magazyn obiektów blob miejsca do magazynowania i tabel
1.  Przejdź do rozszerzenie przestrzeni dyskowej w portalu zarządzania Azure:  
![graphic1][graphic1]
2.  Zlokalizuj Magazyn używany przez zadanie, a następnie przejdź do niej:  
![graphic2][graphic2]
3.  Kliknij polecenie Zarządzaj klawiszy dostępu:  
![graphic3][graphic3]
4.  Między klucza podstawowego programu Access i pomocniczą klawisz dostępu, **Wybierz ten, który nie jest używany przez zadanie**.
5.  Trafienie Regeneruj:  
![graphic4][graphic4]
6.  Skopiuj wygenerowanym klucz:  
![graphic5][graphic5]
7.  Przejdź do strony 2.

###<a name="event-hubs"></a>Koncentratory zdarzenia
1.  Przejdź do rozszerzenia Bus usługi w portalu zarządzania Azure:  
![graphic6][graphic6]
2.  Zlokalizuj Namespace Bus usługi używane przez zadanie, a następnie przejdź do niej:  
![graphic7][graphic7]
3.  Jeśli zadanie używa zasady udostępniania na Namespace Bus usługi, przejdź do kroku 6  
4.  Przejdź na kartę koncentratory zdarzeń:  
![graphic8][graphic8]
5.  Zlokalizuj Centrum zdarzeń, używane przez zadanie, a następnie przejdź do niej:  
![graphic9][graphic9]
6.  Przejdź na kartę Konfigurowanie:  
![graphic10][graphic10]
7.  Na nazwie zasad listy rozwijanej odszukaj zasady udostępniania używane przez zadania:  
![graphic11][graphic11]
8.  Między klucz podstawowy i pomocniczy klucza, **Wybierz ten, który nie jest używany przez zadanie**.  
9.  Regeneruj trafień:  
![graphic12][graphic12]
10. Skopiuj wygenerowanym klucz:  
![graphic13][graphic13]
11. Przejdź do strony 2.  

###<a name="sql-database"></a>Baza danych SQL

>[AZURE.NOTE] Uwaga: należy połączyć się z usługą bazy danych SQL. Chwilę pokazująca, jak to zrobić przy użyciu środowiska zarządzania w portalu zarządzania Azure, ale możesz użyć niektórych po stronie klienta narzędzia takie jak SQL Server Management Studio także.

1.  Przejdź do rozszerzenia bazy danych programu SQL w portalu zarządzania Azure:  
![graphic14][graphic14]
2.  Zlokalizuj bazę danych SQL używane przez łącze zadania i **kliknij na serwerze** w tym samym wierszu:  
![graphic15][graphic15]
3.  Kliknij polecenie Zarządzaj:  
![graphic16][graphic16]
4.  Typ wzorca bazy danych:  
![graphic17][graphic17]
5.  Wpisz nazwę użytkownika, hasło i kliknij przycisk Zaloguj:  
![graphic18][graphic18]
6.  Kliknij pozycję nowe zapytanie:  
![graphic19][graphic19]
7.  Wpisz poniższe zapytanie, zamieniając < login_name > swoją nazwę użytkownika i zamieniania <enterStrongPasswordHere> przy użyciu nowego hasła:  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  Kliknij przycisk Uruchom:  
![graphic20][graphic20]
9.  Wróć do kroku 2 i tego czasu, kliknij bazę danych:  
![graphic21][graphic21]
10. Kliknij polecenie Zarządzaj:  
![graphic22][graphic22]
11. Wpisz nazwę użytkownika, hasło i kliknij przycisk logowania:  
![graphic23][graphic23]
12. Kliknij pozycję nowe zapytanie:  
![graphic24][graphic24]
13. Wpisz poniższe zapytanie, zamieniając < nazwa_użytkownika > pod nazwą, w którym mają być identyfikowane tego logowania w kontekście tej bazy danych (można udostępnić tę samą wartość nadana < login_name >, na przykład) i zastępowanie < login_name > Nowa nazwa użytkownika:  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. Kliknij przycisk Uruchom:  
![graphic25][graphic25]
15. Teraz należy podać nowy użytkownik o tej samej ról i uprawnień do oryginalnej użytkownik ma.
16. Przejdź do strony 2.

##<a name="part-2-stopping-the-stream-analytics-job"></a>Część 2: Zatrzymywanie zadania analizy strumieniu
1.  Przejdź do rozszerzenia analizy strumieniu w portalu zarządzania Azure:  
![graphic26][graphic26]
2.  Znajdź zadanie, a następnie przejdź do niej:  
![graphic27][graphic27]
3.  Przejdź do karty danych wejściowych lub kartę wyników według tego, czy są obracanie poświadczenia na dane wejściowe lub wyjściowe.  
![graphic28][graphic28]
4.  Kliknij polecenie Zatrzymaj i upewnij się, że zadanie zostało zatrzymane:  
![graphic29][graphic29] Czekaj na zadania zakończyć.
5.  Znajdź wejścia i wyjścia, który chcesz obrócić poświadczenia i przejdź do niej:  
![graphic30][graphic30]
6.  Przejdź do części 3.

##<a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Część 3: Edytowanie poświadczenia na zadaniu analizy strumieniu

###<a name="blob-storagetable-storage"></a>Magazyn obiektów blob miejsca do magazynowania i tabel
1.  Znajdź pole klucz konta miejsca do magazynowania i wkleić klucz wygenerowanym:  
![graphic31][graphic31]
2.  Kliknij polecenie Zapisz i Potwierdź zapisanie wprowadzonych zmian:  
![graphic32][graphic32]
3.  Testowanie połączenia będzie automatycznie uruchamiany, zapisując wprowadzone zmiany, upewnij się, czyli pomyślnie minął.
4.  Przejdź do części 4.

###<a name="event-hubs"></a>Koncentratory zdarzenia
1.  Znajdź pole klucza zasad Centrum zdarzeń i wkleić klucz wygenerowanym:  
![graphic33][graphic33]
2.  Kliknij polecenie Zapisz i Potwierdź zapisanie wprowadzonych zmian:  
![graphic34][graphic34]
3.  Testowanie połączenia będzie automatycznie uruchamiany, zapisując wprowadzone zmiany, upewnij się, został pomyślnie przekazany.
4.  Przejdź do części 4.

###<a name="power-bi"></a>Power BI
1.  Kliknij pozycję autoryzacji odnawiania:  
* ![graphic35][graphic35]
* Otrzymasz potwierdzenie następujące czynności:  
* ![graphic36][graphic36]
2.  Kliknij polecenie Zapisz i Potwierdź zapisanie wprowadzonych zmian:  
![graphic37][graphic37]
3.  Testowanie połączenia będzie automatycznie uruchamiany, gdy zapiszesz zmiany, upewnij się, że został pomyślnie przeszła.
4.  Przejdź do części 4.

###<a name="sql-database"></a>Baza danych SQL
1.  Znajdowanie pola Nazwa użytkownika i hasło i wkleić je do nowo utworzonego zestaw poświadczeń:  
![graphic38][graphic38]
2.  Kliknij polecenie Zapisz i Potwierdź zapisanie wprowadzonych zmian:  
![graphic39][graphic39]
3.  Testowanie połączenia będzie automatycznie uruchamiany, zapisując wprowadzone zmiany, upewnij się, został pomyślnie przekazany.  
4.  Przejdź do części 4.

##<a name="part-4-starting-your-job-from-last-stopped-time"></a>Część 4: Rozpoczynanie pracy z ostatniego przestał
1.  Poruszanie się w kierunku od wejścia i wyjścia:  
![graphic40][graphic40]
2.  Kliknij polecenie Rozpocznij:  
![graphic41][graphic41]
3.  Wybierz ostatniego zatrzymana, a następnie kliknij przycisk OK:  
 ![graphic42][graphic42]
4.  Przejdź do części 5.  

##<a name="part-5-removing-the-old-set-of-credentials"></a>Część 5: Usuwanie starej zestaw poświadczeń
Ta część dotyczy dane wejściowe i wyjściowe:
* Magazyn obiektów blob
* Koncentratory zdarzenia
* Baza danych SQL
* Magazyn tabel

###<a name="blob-storagetable-storage"></a>Magazyn obiektów blob miejsca do magazynowania i tabel
Powtórz część 1 dla klawisz dostępu użytego wcześniej przez zadanie do odnawiania teraz nieużywanego klucza dostępu.

###<a name="event-hubs"></a>Koncentratory zdarzenia
Powtórz część 1 klucza, który wcześniej był używany do odnawiania klucza obecnie nieużywane zadania.

###<a name="sql-database"></a>Baza danych SQL
1.  Wróć do okna kwerendy z część 1 — krok 7 i wpisz poniższe zapytanie, zamieniając nazwę użytkownika, która była używana wcześniej przez zadanie < previous_login_name >:  
`DROP LOGIN <previous_login_name>`  
2.  Kliknij przycisk Uruchom:  
    ![graphic43][graphic43]  

Otrzymasz potwierdzenie następujące czynności: 

    Command(s) completed successfully.

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 
