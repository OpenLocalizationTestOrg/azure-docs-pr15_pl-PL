<properties 
    pageTitle="Tworzenie i przywracanie kopii zapasowej w usługach BizTalk | Microsoft Azure" 
    description="Usługi BizTalk zawierają i przywracania kopii zapasowych. Dowiedz się, jak tworzyć i przywracania kopii zapasowej i określ, co otrzymuje kopii zapasowej. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-backup-and-restore"></a>BizTalk usługi: Kopia zapasowa i przywracanie

Usług Azure BizTalk zawiera funkcje i przywracania kopii zapasowych. W tym temacie opisano kopia zapasowa i przywracanie usług BizTalk przy użyciu portalu klasyczny Azure.

Możesz również wykonywanie kopii zapasowej usług BizTalk przy użyciu [Interfejsu API usługi REST usług BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325584). 

> [AZURE.NOTE] Hybrydowe połączeń nie są kopii zapasowej, niezależnie od wersji. Należy ponownie utworzyć połączenia hybrydowych.

## <a name="before-you-begin"></a>Przed rozpoczęciem

- Kopia zapasowa i przywracanie nie mogą być dostępne dla wszystkich wersji. Zobacz [usług BizTalk: wykres wersje](biztalk-editions-feature-chart.md).

- Za pomocą portalu klasyczny Azure, możesz utworzenia kopii zapasowej na żądanie lub utworzyć zaplanowanej kopii zapasowej. 

- Zawartość kopii zapasowej można przywrócić do tej samej usługi BizTalk lub do nowej usługi BizTalk. Aby przywrócić usługę BizTalk za pomocą tej samej nazwie, należy usunąć Istniejąca usługa BizTalk i nazwa musi być dostępna. Po usunięciu usługi BizTalk może potrwać dłużej niż przetransponowane dla tej samej nazwy był dostępny. Jeśli nie można czekać taką samą nazwę był dostępny, przywracanie do nowej usługi BizTalk.

- Usługi BizTalk można przywrócić w tej samej wersji lub nowszej wersji. Przywracanie usługi BizTalk niższej wersji, z po wykonaniu kopii zapasowej, nie jest obsługiwane.

    Na przykład można przywrócić kopii zapasowej za pomocą Basic Edition w wersji Premium. Nie można przywrócić kopii zapasowej przy użyciu wersji Premium do wersji Standard.

- Numery kontroli Edytuj kopię zapasową do kontynuacji numery kontroli. Jeśli wiadomości są przetwarzane po ostatniej kopii zapasowej, przywracanie kopii zapasowej zawartości może spowodować numery zduplikowanych kontroli.

- Jeśli partia ma aktywne wiadomości, przetwarzanie partii **przed** wykonywanie kopii zapasowej. Podczas tworzenia kopii zapasowej (jako wymaganego lub według harmonogramu), wiadomości partiami nigdy nie są przechowywane. 

    **Podjęcia kopii zapasowej z aktywnej wiadomości w partii te wiadomości nie są kopii zapasowej i w związku z tym zostają utracone.**

- Opcjonalnie: W portalu usługi BizTalk Zatrzymaj wszystkie operacje zarządzania.


## <a name="create-a-backup"></a>Tworzenie kopii zapasowej

Kopia zapasowa może zostać wykonane w dowolnym momencie i całkowicie sterowane przez Ciebie. W tej sekcji przedstawiono procedurę tworzenia kopii zapasowych przy użyciu portalu klasyczny Azure, w tym:

[Na żądanie kopii zapasowej](#backupnow)

[Planowanie tworzenia kopii zapasowych](#backupschedule)

#### <a name="backupnow"></a>Na żądanie kopii zapasowej
1. W portalu klasyczny Azure wybierz **BizTalk usługi**, a następnie wybierz usługę BizTalk, który chcesz utworzyć kopię zapasową.
2. Na karcie **pulpitu nawigacyjnego** wybierz pozycję **Wykonywanie kopii zapasowej** w dolnej części strony.
3. Wprowadź nazwę kopii zapasowej. Na przykład wprowadź *myBizTalkService*BU*daty*.
4. Konto magazyn obiektów blob i wybierz przycisk znacznik wyboru, aby rozpocząć wykonywanie kopii zapasowej.

Po wykonaniu kopii zapasowej, kontenera z kopii zapasowej wprowadzona nazwa jest tworzona na koncie miejsca do magazynowania. Ten kontener zawiera konfiguracji kopii zapasowej usługi BizTalk.

#### <a name="backupschedule"></a>Planowanie tworzenia kopii zapasowych

1. W portalu klasycznym Azure wybierz **Usługi BizTalk**, wybierz nazwę usługi BizTalk chcesz zaplanować kopii zapasowej, a następnie wybierz kartę **Konfiguruj** .
2. Ustaw **stan kopii zapasowej** na **Automatyczne**. 
3. Wybierz **Konto miejsca do magazynowania** do przechowywania kopii zapasowej, wprowadź **częstotliwości** do tworzenia kopii zapasowych oraz jak długo, aby zachować kopie zapasowe (**Dni przechowywania**):

    ![][AutomaticBU]

    **Notatki**   
    - W **Dni przechowywania**okres przechowywania musi być większa niż częstotliwość kopii zapasowej.
    - Wybierz, **co najmniej jedna kopia zapasowa na bieżąco**, nawet jeśli po upływie okres przechowywania.
    

4. Wybierz przycisk **Zapisz**.


Po uruchomieniu zaplanowanego zadania kopii zapasowej tworzy kontenera (do przechowywania kopii zapasowych danych) w wprowadzonego konta miejsca do magazynowania. Nazwa kontenera nosi nazwę *usługi BizTalk nazwę — daty i godziny*. 

Jeśli pulpit nawigacyjny usługi BizTalk pokazuje stan **nie powiodło się** :

![Data ostatniego planowania stan kopii zapasowej][BackupStatus] 

Łącze zostanie wyświetlona dzienniki operacji usług zarządzania ułatwiających rozwiązywanie problemów. Zobacz [usług BizTalk: Rozwiązywanie problemów z używaniem dzienniki operacji](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Przywracanie

Z portalu klasyczny Azure lub [Przywracanie BizTalk usługi interfejsu API usługi REST](http://go.microsoft.com/fwlink/p/?LinkID=325582), można przywrócić kopie zapasowe. W tej sekcji przedstawiono czynności, aby przywrócić za pomocą portalu klasyczny.

#### <a name="before-restoring-a-backup"></a>Przed przywracanie kopii zapasowej

- Podczas przywracania usługi BizTalk można wprowadzić nowe śledzenie, archiwizacji i monitorowanie sklepy.

- Te same dane środowisko uruchomieniowe Edytuj zostaną przywrócone. Edytuj wykonywania kopii zapasowej przechowuje liczby kontrolki. Numery przywrócenie kontroli są w kolejności od czasu wykonywania kopii zapasowej. Jeśli wiadomości są przetwarzane po ostatniej kopii zapasowej, przywracanie kopii zapasowej zawartości może spowodować numery zduplikowanych kontroli.

#### <a name="restore-a-backup"></a>Przywracanie z kopii zapasowej

1. W portalu klasyczny Azure, wybierz **Nowy** > **Aplikacji usług** > **Usługi BizTalk** > **Przywracanie**:

    ![Przywracanie z kopii zapasowej][Restore]

2. W **Kopii zapasowej adres URL**wybierz ikonę folderu, a następnie rozwiń listę konto Azure miejsca do magazynowania, które są przechowywane kopia zapasowa konfiguracji usługi BizTalk. Rozwiń kontener, a w okienku po prawej stronie wybierz odpowiedni plik kopii zapasowej txt. 
<br/><br/>
Wybierz pozycję **Otwórz**.

3. Na stronie **Usługi przywracania BizTalk** wprowadź **Nazwę usługi BizTalk** i sprawdź **Adres URL domeny**, **Edition**i **Region** przywróceniu usługi BizTalk. **Tworzenie nowego wystąpienia bazy danych SQL** dla bazy danych śledzenia:

    ![][RestoreBizTalkService]

    Wybierz strzałkę w następnym.

4.  Sprawdź nazwę bazy danych SQL, wprowadź serwera fizycznego miejsce, w którym można utworzyć bazy danych SQL i nazwy użytkownika i hasła dla tego serwera.


    Jeśli chcesz skonfigurować edition bazy danych SQL, rozmiar i inne właściwości, wybierz **Konfigurowanie zaawansowanych ustawień bazy danych**. 

    Wybierz strzałkę w następnym.

5. Utwórz nowe konto miejsca do magazynowania lub wprowadź istniejącego konta miejsca do magazynowania w usłudze BizTalk.

7. Zaznacz znacznik wyboru, aby rozpocząć przywracanie.

Po pomyślnym zakończeniu przywracania nową usługę BizTalk znajduje się w stanie wstrzymania na stronie BizTalk usług w portalu klasyczny Azure.



### <a name="postrestore"></a>Po przywróceniu kopii zapasowej

Zawsze po przywróceniu usługi BizTalk w stanie **zawieszone** . W tym trybie zostaną wprowadzone zmiany konfiguracji przed nowym środowisku działa, w tym:

- Jeśli utworzono aplikacjami usług BizTalk przy użyciu zestawu SDK usług BizTalk Azure, może być konieczne zaktualizować poświadczeń kontroli dostępu (ACS) w tych aplikacjach do pracy ze środowiskiem przywrócone.

- Przywracanie usługi BizTalk, aby odtworzyć istniejące środowisko usługi BizTalk. W tej sytuacji, jeśli istnieją umowy skonfigurowane w portalu usługi BizTalk oryginalny korzystające z folderu FTP źródła, może być konieczne Aktualizacja umów w środowisku nowo przywrócone przy użyciu folderu FTP innego źródła. W przeciwnym razie może być dwóch różnych umów próby pobierają tego samego komunikatu.

- Jeśli przywracany ma wielu środowiskach usługi BizTalk, upewnij się, że docelowe poprawne środowiska w aplikacji programu Visual Studio, polecenia cmdlet programu PowerShell, API pozostałych lub Trading partnera zarządzania interfejsy API modelu OBIEKTOWEGO.

- Najlepiej skonfigurować automatyczne kopie zapasowe na nowo przywrócenie środowisko usługi BizTalk.

Aby uruchomić usługę BizTalk w portalu klasyczny Azure, wybierz usługę przywrócenie BizTalk i wybierz **Życiorys** na pasku zadań. 



## <a name="what-gets-backed-up"></a>Co to jest kopii zapasowej

Po utworzeniu kopii zapasowej tworzy kopię zapasową następujące elementy:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Elementy kopii zapasowej</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Portal usługi Azure BizTalk</strong></td>
</tr> 
<tr>
<td>Konfiguracja i Runtime</td> 
<td>
<ul>
<li>Szczegóły dotyczące partnera i profilu</li>
<li>Umów partnera</li>
<li>Niestandardowe zestawy wdrożony</li>
<li>Mosty wdrożony</li>
<li>Certyfikaty</li>
<li>Umożliwia przekształcenie wdrożony</li>
<li>Procesy</li>
<li>Szablony utworzone i zapisane w portalu usługi BizTalk</li>
<li>X12 mapowań ST01 i GS01</li>
<li>Numery kontroli (Edytuj)</li>
<li>Wartości AS2 Mikrofonu wiadomości</li>
</ul>
</td>
</tr> 
 
<tr>
<td colspan="2">
 <strong>Usługa Azure BizTalk</strong></td>
</tr> 
<tr>
<td>Certyfikat SSL</td> 
<td>
<ul>
<li>Dane certyfikatu SSL</li>
<li>Hasło certyfikatu SSL</li>
</ul>
</td>
</tr> 
<tr>
<td>Ustawienia usługi BizTalk</td> 
<td>
<ul>
<li>Liczba jednostek skali</li>
<li>Edition</li>
<li>Wersja produktu</li>
<li>Region i centrum danych</li>
<li>Nazw usługi ACS (Control) programu Access i klawiszy</li>
<li>Śledzenie parametry połączenia bazy danych</li>
<li>Archiwizowanie parametry połączenia konta miejsca do magazynowania</li>
<li>Monitorowanie miejsca do magazynowania parametry połączenia konta</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Dodatkowe elementy.</strong></td>
</tr> 
<tr>
<td>Śledzenie bazy danych</td> 
<td>Po utworzeniu usługę BizTalk są wprowadzane dane śledzenia bazy danych z tym serwerze bazy danych SQL Azure i nazwa bazy danych śledzenia. Śledzenie bazy danych nie jest automatycznie kopii zapasowej.
<br/><br/>
<strong>Ważne</strong><br/>
Jeśli zostanie usunięty bazy danych śledzenia i odzyskać na potrzeby bazy danych, musi istnieć poprzedniej kopii zapasowej. Jeśli nie istnieje kopii zapasowej, bazy danych śledzenia i jego danych nie są odzyskania. W takiej sytuacji należy utworzyć nową bazę danych śledzenia o takiej samej nazwie bazy danych. Zaleca się Geo replikacji.</td>
</tr> 
</table>

## <a name="next"></a>Następny

Aby utworzyć usługi Azure BizTalk w portalu klasyczny Azure, przejdź do [usług BizTalk: portal klasyczny inicjowania obsługi administracyjnej Azure za pomocą](http://go.microsoft.com/fwlink/p/?LinkID=302280). Aby rozpocząć tworzenie aplikacji, przejdź do [Usługi Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Zobacz też
- [Aby utworzyć kopię zapasową usługi BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [Przywracanie z kopii zapasowej usługi BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [Usługi BizTalk: Deweloper, podstawowej, standardowe i wykres wersji Premium](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [Usługi BizTalk: Klasyczny portal Azure za pomocą inicjowania obsługi administracyjnej](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [Usługi BizTalk: Wykres stan inicjowania obsługi administracyjnej](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk usługi: Karty pulpitu nawigacyjnego, monitorowanie i skali](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [Usługi BizTalk: ograniczanie](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [Usługi BizTalk: Nazwa wystawcy i klucza wystawcy](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Jak mogę rozpocząć przy użyciu SDK usług BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png
 
