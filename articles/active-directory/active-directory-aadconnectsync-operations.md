<properties
   pageTitle="Azure AD Connect synchronizacją: zadań operacyjnych i zagadnienia | Microsoft Azure"
   description="W tym temacie opisano zadań operacyjnych dla synchronizacji Azure AD Connect i przygotowywania do obsługi tego składnika."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Connect synchronizacją: zadań operacyjnych i uwarunkowania
Celem w tym temacie jest do opisu zadań operacyjnych dla Azure AD Connect synchronizacji.

## <a name="staging-mode"></a>Tryb tymczasowego
Tryb tymczasowego może być używany dla kilka scenariuszy, w tym:

-   Wysokiej dostępności.
-   Testowanie i wdrażanie nowych zmian konfiguracji.
-   Wprowadzenie nowego serwera i zlikwidować starego.

Na serwerze w trybie tymczasowy możesz wprowadzić zmiany w konfiguracji i Podgląd zmian, zanim serwer być aktywna. W tym obszarze można również uruchomić pełne importowanie i pełnej synchronizacji w celu zweryfikowania, że wszystkie zmiany są poprawnie przed wprowadzeniem tych zmian w środowisku produkcyjnym.

Podczas instalacji można wybrać serwera w **trybie**. Ta akcja powoduje, że serwer jest aktywne importu i synchronizacji, ale nie działa dowolny Eksport. Serwer w trybie tymczasowy nie jest uruchomiony, synchronizacji haseł lub zapisu hasła, nawet jeśli te funkcje są wybrane podczas instalacji. Po wyłączeniu trybu tymczasowy serwer rozpoczyna eksportowanie, umożliwia synchronizacji haseł i włącza zapisu hasła.

Nadal można wymusić eksportu za pomocą Menedżera usługi synchronizacji.

Serwer w trybie tymczasowy w dalszym ciągu odbierać zmiany z usługą Active Directory i Azure AD. Zawsze ma on kopię najnowszych zmian i sporządzanie bardzo szybko mogą obowiązki innego serwera. Jeśli wprowadzisz zmiany w konfiguracji serwera podstawowego jest obowiązek wprowadzić te same zmiany na serwerze w trybie tymczasowej.

Dla osób, które znający starszych technologiami synchronizacji tymczasowy tryb różni się od serwer ma swoją własną bazę danych SQL. Ta architektura pozwala serwerze tryb znajdować się w innym centrum danych.

### <a name="verify-the-configuration-of-a-server"></a>Weryfikowanie konfiguracji serwera
Aby zastosować tej metody, wykonaj następujące czynności:

1. [Przygotowywanie](#prepare)
2. [Importowanie i synchronizowanie](#import-and-synchronize)
3. [Sprawdź](#verify)
4. [Przełączanie ASP](#switch-active-server)

#### <a name="prepare"></a>Przygotowywanie

1. Zainstaluj narzędzie Azure AD Connect, wybierz pozycję **tymczasowej tryb**i usuwanie zaznaczenia **Uruchamianie synchronizacji** na ostatniej stronie Kreatora instalacji. W tym trybie umożliwia ręczne uruchamianie aparat synchronizacji.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Zaloguj się poza i zaloguj się i z start menu wybierz pozycję **Usługi synchronizacji**.

#### <a name="import-and-synchronize"></a>Importowanie i synchronizowanie

1. Zaznacz **Łączniki**, a następnie wybierz pierwszego łącznika typu **Usług domenowych Active Directory**. Kliknij przycisk **Uruchom**, wybierz pozycję **pełne importowanie**, a następnie **przycisk OK**. Wykonaj te kroki dla wszystkich łączników tego typu.
2. Zaznacz łącznik typu **Usługi Azure Active Directory (Microsoft)**. Kliknij przycisk **Uruchom**, wybierz pozycję **pełne importowanie**, a następnie **przycisk OK**.
3. Upewnij się, że karta łączniki nadal jest zaznaczone. Dla każdego łącznika typu **Usług domenowych Active Directory**kliknij polecenie **Uruchom**, wybierz **Różnicy synchronizacji**, a następnie **przycisk OK**.
4. Zaznacz łącznik typu **Usługi Azure Active Directory (Microsoft)**. Kliknij przycisk **Uruchom**, wybierz **Różnicy synchronizacji**, a następnie **przycisk OK**.

Teraz masz umieszczane zmieni się na Azure AD eksportu i lokalnego AD (Jeśli używasz wdrożenie hybrydowe programu Exchange). Następne kroki umożliwiają sprawdzanie, co ma zostać zmienione przed rozpoczęciem rzeczywistego eksportowania do katalogów.

#### <a name="verify"></a>Sprawdź

1. Uruchom wiersz cmd i przejdź do`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Uruchom:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Nazwa łącznika można znaleźć w usługi synchronizacji. Ma nazwę podobną do "contoso.com — AAD" dla Azure AD.
3. Uruchom:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Masz pliku w % temp % o nazwie export.csv, który może sprawdzić w programie Microsoft Excel. Ten plik zawiera wszystkie zmiany, które mają być eksportowane.
5. Wprowadź niezbędne zmiany w danych lub konfiguracji i uruchom poniższe czynności ponownie (Importowanie i synchronizowanie i sprawdź) do momentu zmiany, które mają być eksportowane są poprawnie.

**Opis plików export.csv**

W większości pliku jest oczywiste. Niektóre skróty do zrozumienia zawartości:

- OMODT — Typ modyfikacji obiektu. Wskazuje, czy operacji na poziomie obiektu jest dodawanie, aktualizowanie lub usuwanie.
- AMODT — Typ modyfikacji atrybutu. Wskazuje, czy operacji na poziomie atrybutu jest dodawanie, aktualizowanie lub usuwanie.

Jeśli wartość atrybutu jest wiele wartości, zostanie wyświetlona nie wszystkie zmiany. Tylko liczba wartości dodawane i usuwane jest widoczny.

#### <a name="switch-active-server"></a>Przełączanie ASP

1. Na serwerze aktywne Wyłącz funkcję serwera (DirSync-FIM-Azure AD Sync), nie jest eksportowany do Azure AD lub ustaw w tymczasowej tryb (Azure AD Connect).
2. Uruchom Kreatora instalacji na serwerze w **trybie** i Wyłącz **Tryb tymczasowej**.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Odzyskiwanie
Jest częścią projektu implementacji w celu zaplanowania co należy zrobić w przypadku awarii miejsce, w którym tracisz serwera synchronizacji. Dostępne są różne modele i które zależy od kilku czynników, takich jak:

-   Co to są usługi uszkodzenia, dla którego nie można wprowadzać zmiany obiektów Azure AD podczas przestoje?
-   Jeśli używasz synchronizacji haseł, użytkowników zaakceptować mają stare hasło w Azure AD na wypadek one zmieniać lokalnego?
-   Czy masz zależność w czasie rzeczywistym operacji, takich jak hasło zapisu?

W zależności od odpowiedzi na te pytania i zasad organizacji można zaimplementować jedną z następujących strategii:

-   Ponownie w razie potrzeby.
-   Ma zapasowych serwera wstrzymania, znana pod nazwą **tymczasowej tryb**.
-   Za pomocą maszyn wirtualnych.

Jeśli nie używasz wbudowanych bazy danych programu SQL Express, powinni również sprawdzić sekcji [Szybkiej SQL](#sql-high-availability) .

### <a name="rebuild-when-needed"></a>Odbudowywanie w razie potrzeby
Strategia rentowny polega na planowanie Odbuduj serwer, w razie potrzeby. Zazwyczaj instalowania aparatu synchronizacji i których początkowej importowanie i synchronizowanie można wykonać w ciągu kilku godzin. Jeśli serwer zamiennych jest niedostępny, prawdopodobnie tymczasowo host aparatu synchronizacji za pomocą kontrolera domeny.

Serwer aparatu synchronizacji nie są zapisywane jakiekolwiek Państwo o obiektach, bazy danych można odbudowany danych w usłudze Active Directory i Azure AD. Atrybut **sourceAnchor** jest używany do obiektów z lokalnym a chmurą. Jeśli odbudować server z istniejących obiektów lokalnym a chmurą, następnie aparat synchronizacji dopasowuje tych obiektów razem ponownie na ponownej instalacji. Czynności, które należy dokument i zapisać są zmiany konfiguracji na serwerze, takich jak filtrowanie i synchronizacji reguł. Przed rozpoczęciem synchronizowania, należy ponownie zastosować te konfiguracji niestandardowych.

### <a name="have-a-spare-standby-server---staging-mode"></a>Serwer wstrzymania zamiennych - tymczasowej tryb
Jeśli masz środowisku bardziej złożone, a następnie zaleca się o jeden lub więcej serwery w stanie wstrzymania. Podczas instalacji możesz włączyć serwera w **trybie**.

Aby uzyskać więcej informacji zobacz [tymczasowej tryb](#staging-mode).

### <a name="use-virtual-machines"></a>Używanie maszyn wirtualnych
Metoda typowe i obsługiwane jest uruchomienie aparatu synchronizacji w maszyny wirtualnej. W przypadku, gdy host występuje problem, obraz z serwerem aparatu synchronizacji, możesz przeprowadzić migrację do innego serwera.

### <a name="sql-high-availability"></a>Wysoce SQL
Jeśli nie korzystasz z programu SQL Server Express dostępnego w usłudze Azure AD Connect, następnie wysoką dostępność dla programu SQL Server należy również uwzględnić. Rozwiązanie tylko szybkiej obsługiwane jest klaster SQL. Nieobsługiwane rozwiązań obejmować odzwierciedlające oraz zawsze.

## <a name="next-steps"></a>Następne kroki

**Przegląd tematów**  

- [Azure AD Connect synchronizacją: opis i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)  
- [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)  
