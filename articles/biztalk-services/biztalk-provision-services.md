<properties
    pageTitle="Tworzenie usługi Azure BizTalk w portalu Azure | Microsoft Azure"
    description="Dowiedz się, jak dodawać lub tworzenie usługi Azure BizTalk w portal Azure. MABS, WABS"
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
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>



# <a name="create-biztalk-services-using-the-azure-portal"></a>Tworzenie usług BizTalk przy użyciu Azure portal

Tworzenie usługi Azure BizTalk w portalu Azure.

> [AZURE.TIP] Aby zalogować się do portalu Azure, potrzebne jest konto Azure i Azure subskrypcji. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej, po upływie kilku minut. Zobacz [Azure bezpłatnej wersji próbnej](http://go.microsoft.com/fwlink/p/?LinkID=239738).

## <a name="create-a-biztalk-service"></a>Tworzenie usługi BizTalk
W zależności od wersji, którą możesz wybrać nie wszystkie ustawienia usługi BizTalk mogą być dostępne.

1. Zaloguj się do [portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W dolnym okienku nawigacji wybierz pozycję **Nowy**:  
![Kliknij przycisk Nowy][NEWButton]

3. Wybierz **Aplikację usługi** > **usługi BIZTALK** > **Utwórz niestandardowy**:  
![Wybierz usługę BizTalk i wybierz opcję Utwórz niestandardowe][NewBizTalkService]

4. Wprowadź ustawienia usługi BizTalk:

    <table border="1">
    <tr>
    <td><strong>Nazwa usługi BizTalk</strong></td>
    <td>Można wprowadzić dowolną nazwę, ale być specyficzne. Przykłady:<br/><br/>
    <em>Moja firma</em>. biztalk.windows.net<br/>
    <em>mycompanymyapplication</em>. biztalk.windows.net<br/>
    <em>MyApplication</em>. biztalk.windows.net<br/><br/>". biztalk.windows.net" są automatycznie dodawane do wprowadzoną nazwę. Spowoduje to utworzenie adres URL, który jest używany do uzyskania dostępu do usługi BizTalk, takich jak <strong>https://<em>myapplication</em>. biztalk.windows.net</strong>.
    </td>
    </tr>
    <tr>
    <td><strong>Edition</strong></td>
    <td>Jeśli jesteś w fazie badania i rozwój, wybierz pozycję <strong>Deweloper</strong>. Jeśli pracujesz w fazie produkcji, użyj <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302279">usług BizTalk: wykres wersje</a> do ustalenia, czy <strong>Premium</strong>, <strong>Standardowy</strong>lub <strong>podstawowe</strong> jest odpowiedniego wyboru dla danego scenariusza biznesowego.
    </td>
    </tr>
    <tr>
    <td><strong>Region</strong></td>
    <td>Wybierz pozycję regionu geograficznego do obsługi usługi BizTalk.</td>
    </tr>
    <tr>
    <td><strong>Adres URL domeny</strong></td>
    <td><strong>Opcjonalnie</strong>. Adres URL domeny jest domyślnie <em>YourBizTalkServiceName</em>. biztalk.windows.net. Można także wprowadzić domenę niestandardową. Na przykład jeśli Twoja domena jest <em>firmy contoso</em>, możesz wprowadzić: <br/><br/>
    <em>Moja firma</em>. contoso.com<br/>
    <em>MyCompanyMyApplication</em>. contoso.com<br/>
    <em>MyApplication</em>. contoso.com<br/>
    <em>YourBizTalkServiceName</em>. contoso.com<br/>
    </td>
    </tr>
    </table>
Wybierz strzałkę w NASTĘPNYM.

5. Wprowadź przechowywania i baz danych ustawień:

    <table border="1">
    <tr>
    <td><strong>Konto magazynowania monitorowania i archiwizacji</strong></td>
    <td>Wybierz istniejące konto miejsca do magazynowania lub Utwórz nowe konto miejsca do magazynowania. <br/><br/>Jeśli tworzysz nowe konto miejsca do magazynowania, wprowadź <strong>Nazwę konta magazynu</strong>.</td>
    </tr>
    <tr>
    <td><strong>Baza danych do śledzenia</strong></td>
    <td>Jeśli korzystasz z istniejącą bazą danych SQL Azure, nie można używać w innej usłudze BizTalk. Potrzebujesz nazwę logowania i hasło wprowadzone po utworzenia serwera bazy danych SQL Azure.<br/><br/><strong>Porada</strong> Tworzenie bazy danych śledzenia i monitorowania i archiwizacji miejsca do magazynowania konta w tym samym co usługa BizTalk.</td>
    </tr>
    </table>
Wybierz strzałkę w NASTĘPNYM.

6. Wprowadź ustawienia bazy danych:

    <table border="1">
    <tr>
    <td><strong>Nazwa</strong></td>
    <td>Dostępne po zaznaczeniu <strong>Tworzenie nowego wystąpienia bazy danych SQL</strong> w poprzedniego ekranu.
    <br/><br/>
   Wprowadź nazwę bazy danych SQL może być używany przez usługę BizTalk.</td>
    </tr>
    <tr>
    <td><strong>Serwer</strong></td>
    <td>Dostępne po zaznaczeniu <strong>Tworzenie nowego wystąpienia bazy danych SQL</strong> na poprzednim ekranie.
    <br/><br/>
   Wybierz z istniejącą bazą danych SQL lub Utwórz nowy serwer bazy danych SQL.</td>
    </tr>
    <tr>
    <td><strong>Nazwa serwera logowania</strong></td>
    <td>Wprowadź nazwę użytkownika logowania.</td>
    </tr>
    <tr>
    <td><strong>Hasło logowania do serwera</strong></td>
    <td>Wprowadź hasło logowania.</td>
    </tr>
    <tr>
    <td><strong>Region</strong></td>
    <td>Dostępne po zaznaczeniu <strong>Tworzenie nowego wystąpienia bazy danych SQL</strong> . Wybierz pozycję regionu geograficznego do obsługi bazy danych SQL.</td>
    </tr>
    </table>

Zaznacz pole wyboru, aby zakończyć działanie kreatora. Zostanie wyświetlona ikona postępu:  
![Ikona informacji o postępie wyświetla po zakończeniu][ProgressComplete]

Po zakończeniu usługa Azure BizTalk jest utworzone i gotowe do aplikacji. Domyślne ustawienia są wystarczające. Jeśli chcesz zmienić ustawienia domyślne, wybierz pozycję **BIZTALK usług** w lewym okienku nawigacji, a następnie wybierz usługi BizTalk. Dodatkowe ustawienia są wyświetlane w obszarze [karty pulpitu nawigacyjnego, Monitor i skala](biztalk-dashboard-monitor-scale-tabs.md) u góry.

W zależności od stanu usługi BizTalk istnieje kilka operacji, które nie może się zakończyć. Aby uzyskać listę tych operacji przejdź do [Wykresu stan usług BizTalk](biztalk-service-state-chart.md).


## <a name="post-provisioning-steps"></a>Po wprowadzeniu inicjowania obsługi administracyjnej czynności

-  [Instalowanie certyfikatu na komputerze lokalnym](#InstallCert)
-  [Dodawanie certyfikatu zawsze gotowe do produkcji](#AddCert)
-  [Uzyskiwanie nazw kontroli dostępu](#ACS)

#### <a name="InstallCert"></a>Instalowanie certyfikatu na komputerze lokalnym
W ramach usługi BizTalk inicjowania obsługi administracyjnej certyfikat z podpisem własnym jest utworzone i skojarzone z subskrypcją usługi BizTalk. Należy pobrać ten certyfikat i zainstaluj go na komputerach z miejsce, w którym możesz wdrażanie aplikacji usług BizTalk lub wysyłać wiadomości do punktu końcowego usługi BizTalk.

1. Zaloguj się do [portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W okienku nawigacji po lewej stronie wybierz **Usług BIZTALK** , a następnie wybierz subskrypcję usługi BizTalk.
3. Wybierz kartę **pulpitu nawigacyjnego** .
4. Wybierz pozycję **Pobieranie certyfikatu SSL**:  
![Modyfikowanie certyfikat SSL][QuickGlance]
5. Kliknij dwukrotnie certyfikat i uruchamianie za pomocą kreatora, aby zainstalować certyfikat. Upewnij się, że zainstalować certyfikat w magazynie **Zaufane główne urzędy certyfikacji** .

#### <a name="AddCert"></a>Dodawanie certyfikatu zawsze gotowe do produkcji
Certyfikatu z podpisem własnym utworzony automatycznie podczas tworzenia usług BizTalk jest przeznaczony do użytku w środowiskach projektowania tylko. W przypadku scenariuszy produkcji zastąpić ją za pomocą certyfikatu zawsze gotowe do produkcji.

1. Na karcie **pulpitu nawigacyjnego** wybierz **Certyfikat SSL aktualizacji**.
2. Przejdź do swojej prywatne certyfikat SSL (pfx*CertificateName*) zawiera nazwę usługi BizTalk, wprowadź hasło, a następnie kliknij znacznik wyboru.

#### <a name="ACS"></a>Uzyskiwanie nazw kontroli dostępu

1. Zaloguj się do [portalu Azure](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. W okienku nawigacji po lewej stronie wybierz **Usług BIZTALK** , a następnie wybierz usługi BizTalk.
3. Na pasku zadań wybierz **Informacje o połączeniu**:  
![Zaznacz informacje o połączeniu][ACSConnectInfo]

4. Skopiuj wartości kontroli dostępu.

Podczas wdrażania projektu usługi BizTalk z programu Visual Studio, wprowadź ten obszar nazw kontrola dostępu. Przestrzeń nazw kontrola dostępu jest tworzona automatycznie tej usługi BizTalk.

Kontrola dostępu wartości można używać z dowolnej aplikacji. Po utworzeniu usługi Azure BizTalk tego obszaru nazw kontrola dostępu określa uwierzytelnianie za pomocą wdrożenia usługi BizTalk. Jeśli chcesz zmienić subskrypcję lub zarządzać nazw, zaznacz **Usługi ACTIVE DIRECTORY** w okienku nawigacji po lewej stronie, a następnie wybierz pozycję obszaru nazw. Na pasku zadań listą dostępnych opcji.

Kliknięcie przycisku **Zarządzaj** otwarcia portalu zarządzania kontroli dostępu. W portalu zarządzania kontrola dostępu usługi BizTalk używa **tożsamości usługi**:  
![Usługa ACS tożsamości do programu Access Control portalu zarządzania][ACSServiceIdentities]

Tożsamość usługi Kontrola dostępu jest zestaw poświadczeń aplikacji lub Twoi klienci do uwierzytelnienia bezpośrednio z kontrola dostępu i odbierania token.

> [AZURE.IMPORTANT]Usługa BizTalk używa **właściciela** dla tożsamości domyślnej usługi i wartość **hasła** . Użycie wartości klucza symetrycznej zamiast wartości hasła, może wystąpić następujący komunikat o błędzie.<br/><br/>*Nie można połączyć z kontem usługi zarządzania kontrola dostępu z podanymi poświadczeniami*

[Zarządzanie i Namespace ACS](https://msdn.microsoft.com/library/azure/hh674478.aspx) lista wskazówki i zalecenia.

## <a name="requirements-explained"></a>Wymagania dotyczące

Bezpłatna wersja nie dotyczą tych wymagań.
<table border="1">
<tr bgcolor="FAF9F9">
        <td><strong>Co jest potrzebne</strong></td>
        <td><strong>Dlaczego jest potrzebny</strong></td>
</tr>
<tr>
<td>Azure subskrypcji</td>
<td>Subskrypcja Określa, kto może zalogować się do portalu Azure. Właściciel konta tworzy subskrypcji <a HREF="https://account.windowsazure.com/Subscriptions">Azure subskrypcji</a>.
<br/><br/>
Konto Azure może mieć wiele subskrypcji i mogą być zarządzane przez każdą osobę, która jest dozwolone. Na przykład usługi Azure użytkownik tworzy subskrypcji o nazwie <em>BizTalkServiceSubscription</em> i zapewnia administratorom BizTalk w firmie (na przykład ContosoBTSAdmins@live.com) dostęp do tej subskrypcji. W tym scenariuszu Administratorzy BizTalk zalogować się do portalu Azure i mieć pełny prawa administratora do wszystkich usług obsługiwanych w subskrypcji, łącznie z usługi Azure BizTalk. Administratorzy BizTalk są posiadacze kont Azure i dlatego nie mają dostępu do dowolnego rozliczeniowym.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=267577">Zarządzaj subskrypcjami i kont miejsca do magazynowania w portalu Azure</a> znajdziesz więcej informacji.
</td>
</tr>
<tr>
<td>Baza danych SQL Azure</td>
<td>Przechowuje tabel, widoków i procedur składowanych używane przez usługę BizTalk, w tym dane śledzenia.
<br/><br/>
Po utworzeniu usługi BizTalk, można użyć istniejącego serwera SQL Azure, bazy danych SQL Azure lub automatyczne utworzenie nowego serwera lub bazy danych.
<br/><br/>
Skala bazy danych SQL jest automatycznie skonfigurowany. Zazwyczaj Skala domyślna jest wystarczające dla usługi BizTalk. Zmienianie skali wpływa na cennik. Zobacz <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=234930">kont i rozliczeń w bazie danych Azure SQL</a>
<br/><br/>
<strong>Notatki</strong>
<br/>
<ul>
<li> Po utworzeniu nowego Azure SQL Server i bazy danych usługi Azure jest włączany automatycznie. Usługa BizTalk wymaga usługi Azure można włączyć.</li>
<li>Po utworzeniu nowej bazy danych SQL Azure na istniejącym serwerze SQL Azure reguły zapory, serwera nie ulegają zmianie. Dzięki temu możliwe jest innych usług Azure nie mają dostęp do serwera bazy danych.</li>
</ul>
</td>
</tr>
<tr>
<td>Azure nazw kontroli dostępu</td>
<td>Uwierzytelnianie przy użyciu usługi Azure BizTalk. Podczas wdrażania projektu usługi BizTalk z programu Visual Studio, wprowadź ten obszar nazw kontrola dostępu. Po utworzeniu usługi BizTalk nazw kontrola dostępu jest tworzona automatycznie.</td>
</tr>

<tr>
<td>Konto Azure miejsca do magazynowania</td>
<td>Zapewnia dostęp do tabel, obiektów blob i kolejki używane przez usługę BizTalk do Zapisz poniższe dane:

<ul>
<li>Pliki dziennika, które Monitoruj usługę BizTalk. Monitorowanie wynik jest również wyświetlany na karcie **monitorowania** w portalu Azure.</li>
<li>Podczas tworzenia X12 lub umowy AS2 między partnerów, możesz włączyć funkcję archiwizacji w celu przechowywania właściwości wiadomości. Te dane są zapisywane na koncie miejsca do magazynowania.</li>
</ul>
<br/>
Po utworzeniu usługi BizTalk można Użyj istniejącego konta miejsca do magazynowania lub automatycznie utworzyć nowe konto miejsca do magazynowania.
<br/><br/>
Domyślne ustawienia przechowywania są wystarczające dla usługi BizTalk.
<br/><br/>
Podczas tworzenia konta miejsca do magazynowania, klucz podstawowy i pomocniczy klucz są tworzone automatycznie. Klucze kontrolowanie dostępu do konta miejsca do magazynowania. Usługa BizTalk automatycznie używa klucza podstawowego.
<br/><br/>
Aby uzyskać więcej informacji, zobacz <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285671">Magazyn</a> .
</td>
</tr>

<tr>
<td>Prywatne certyfikat SSL</td>
<td>
Po utworzeniu usługi Azure BizTalk adresu URL HTTPS, która zawiera nazwę usługi BizTalk również zostanie utworzona. Ten adres URL jest automatycznie skonfigurowany do używania tylko do rozwoju certyfikat z podpisem własnym. Dla produkcji trzeba prywatne certyfikat SSL.
<br/><br/>
<strong>Informacje na temat certyfikatu SSL ważne</strong>

<ul>
<li>Data wygaśnięcia certyfikatu musi być mniejsza niż 5 lat.</li>
<li>Wszystkie certyfikaty prywatne żądanie podania hasła. To hasło i zgodnie z zaleceniami dotyczącymi udostępnić to hasło administratorami.</li>
<li>Certyfikaty z podpisem własnym są używane w środowisku badania i rozwój. Podczas korzystania z certyfikatów z podpisem własnym, importowanie certyfikatu w magazynie certyfikatów osobistych i magazynie certyfikatów Zaufane główne urzędy certyfikacji.</li>
</ul>
<br/>Podczas wysyłania produkcji żądania certyfikatu do urzędu certyfikacji, zapewniają następujące właściwości certyfikatu:
<br/>

<ul>
<li><strong>Użycie klucza rozszerzonego</strong>: co najmniej usługi Azure BizTalk wymaga uwierzytelniania serwera.</li>
<li><strong>Typowe nazwy</strong>: wpisz w pełni kwalifikowaną nazwę domeny (FQDN) adresu URL usługi BizTalk Azure. Zobacz <a HREF="#BizTalk">Tworzenie usługi BizTalk</a> w tym artykule.</li>
</ul>
<br/>
Po utworzeniu usługę BizTalk można dodać nowy lub inny certyfikat.
</td>
</tr>
</table>



## <a name="hybrid-connections"></a>Hybrydowe połączenia

Po utworzeniu usługi Azure BizTalk kartę **Połączenia hybrydowego** jest dostępny:

![Karta połączenia hybrydowego][HybridConnectionTab]

Hybrydowe połączenia są używane do łączenia Azure witryny sieci Web lub Azure usługi mobilnej dowolny zasób lokalnych korzysta z portu TCP statycznego, takich jak program SQL Server, MySQL HTTP sieci Web API, usług Mobile i większość niestandardowych usług sieci Web.  Połączenia hybrydowych i Usługa karty BizTalk są różne. Usługa karty BizTalk umożliwia nawiązywanie połączenia usługi Azure BizTalk z lokalnym systemem linii biznesowych (LOB).

 Zobacz [Hybrydowych połączenia](integration-hybrid-connection-overview.md) , aby dowiedzieć się więcej, takich jak tworzenie i zarządzanie połączeniami hybrydowych.


## <a name="next-steps"></a>Następne kroki

Teraz, gdy usługa BizTalk zostanie utworzona, zapoznaj się z różnych [usług BizTalk: karty pulpitu nawigacyjnego, monitorowanie i skali](biztalk-dashboard-monitor-scale-tabs.md). Twoja usługa BizTalk jest gotowy do aplikacji. Aby rozpocząć tworzenie aplikacji, przejdź do [Usługi Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Zobacz też
- [Usługi BizTalk: Wykres wersji](biztalk-editions-feature-chart.md)<br/>
- [Usługi BizTalk: Wykres stanu](biztalk-service-state-chart.md)<br/>
- [BizTalk usługi: Kopia zapasowa i przywracanie](biztalk-backup-restore.md)<br/>
- [Usługi BizTalk: ograniczanie](biztalk-throttling-thresholds.md)<br/>
- [Usługi BizTalk: Nazwa wystawcy i klucza wystawcy](biztalk-issuer-name-issuer-key.md)<br/>
- [Jak mogę rozpocząć przy użyciu SDK usług BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
- [Hybrydowe połączenia](integration-hybrid-connection-overview.md)

[NewBizTalkService]: ./media/biztalk-provision-services/WABS_NewBizTalkService.png
[NEWButton]: ./media/biztalk-provision-services/WABS_New.png
[ProgressComplete]: ./media/biztalk-provision-services/WABS_ProgressComplete.png
[ACSConnectInfo]: ./media/biztalk-provision-services/WABS_ACSConnectInformation.png
[QuickGlance]: ./media/biztalk-provision-services/WABS_QuickGlance.png
[ACSServiceIdentities]: ./media/biztalk-provision-services/WABS_ACSServiceIdentities.png
[HybridConnectionTab]: ./media/biztalk-provision-services/WABS_HybridConnectionTab.png
