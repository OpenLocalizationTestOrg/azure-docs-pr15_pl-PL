<properties
    pageTitle="Azure AD Connect kondycji — często zadawane pytania"
    description="Niniejszy artykuł odpowiedzi na pytania dotyczące Azure AD łączenie kondycji. Niniejszy artykuł dotyczy pytania dotyczące korzystania z usługi, w tym modelu rozliczeń możliwości, ograniczenia i pomocy technicznej."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>


# <a name="azure-ad-connect-health-frequently-asked-questions-faq"></a>Azure AD Connect kondycji — często zadawane pytania

Niniejszy artykuł odpowiedzi na pytania dotyczące Azure AD łączenie kondycji. Niniejszy artykuł dotyczy pytania dotyczące korzystania z usługi, w tym modelu rozliczeń możliwości, ograniczenia i pomocy technicznej.

## <a name="general-questions"></a>Ogólne pytania



**P: zarządzać wielu katalogów Azure AD. Jak przełączyć na jeden z Azure Active Directory Premium?**

Możesz przełączać się między różne Azure AD dzierżaw zaznaczając obecnie podpisanego w polu Nazwa użytkownika w prawym górnym rogu i wybierając odpowiednie konto. Jeśli konto nie ma na liście, wybierz pozycję Wyloguj się, a następnie użyj poświadczeń administratora globalnego katalogu, która ma Azure Active Directory Premium włączone do zalogowania się.

## <a name="installation-questions"></a>Pytania dotyczące instalacji



**P: jaki jest wpływ na instalowania Azure AD łączenie Agent kondycji na poszczególnych serwerach?**

Wpływ instalowania Microsoft Azure AD łączenie kondycji agentów ADFS, serwerów Proxy aplikacji sieci Web, serwer Azure AD Connect (sycn), kontrolery są minimalne dotyczące Procesora, przepustowość sieci zużycie pamięci i miejsca do magazynowania.

Poniższe numery są przybliżenie.

- Użycie Procesora: Zwiększ ~ 1%
- Zużycie pamięci: do 10% sumy pamięci

>[AZURE.NOTE] W przypadku agenta nie można komunikować się Azure agent przechowuje dane lokalnie, aż do zdefiniowana maksymalna. Agent zastępuje "" buforowane na podstawie "najmniej ostatnio obsługiwane".

- Lokalne buforu miejsca do magazynowania dla Azure AD łączenie agentów kondycji: ~ 20 MB
- Serwer usług AD FS zaleca się obsługi administracyjnej miejsca 1024 MB (1 GB) dla kanału inspekcji AD FS dla Azure AD łączenie agentów kondycji przetwarzania wszystkie dane inspekcji, zanim zostanie zastąpiony.

**P: czy mam Uruchom ponownie serwerów podczas instalacji Azure AD łączenie agentów kondycji?**

Wartość nie. Instalacja czynników nie wymaga ponownego uruchomienia serwera. Jednak instalacji niektórych czynności wstępne może wymagać ponownego uruchomienia serwera.

Na przykład w systemie Windows Server 2008 R2 instalacji programu .net Framework 4,5 wymaga ponownego uruchomienia serwera.


**Łączenie usługi kondycji p: czy Azure AD pracować przez serwer proxy http przekazująca?**

Wartość Tak.  Do na rozpoczęcie operacji, należy skonfigurować agenta kondycji, aby przesłać dalej żądania http ruchu wychodzącego przy użyciu serwer HTTP Proxy. Aby uzyskać więcej informacji, zobacz [Konfigurowanie Azure AD łączenie kondycji agentów do użytku serwer HTTP Proxy.](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)
Jeśli musisz skonfigurować serwer proxy podczas rejestrowania agenta, może być konieczne wcześniej zmodyfikować ustawienia serwera Proxy programu Internet Explorer.
1. Otwieranie programu Internet Explorer -> Ustawienia -> Internet połączenia -> Opcje -> Ustawienia sieci LAN.
2. Wybierz pozycję Użyj serwera Proxy dla sieci LAN.
3. Jeśli masz porty innego serwera proxy dla protokołu HTTP i HTTPS-HTTPS, wybierz pozycję Zaawansowane.

**P: czy Azure AD łączenie kondycji usługi obsługi uwierzytelniania podstawowego podczas łączenia się z serwerami proxy Http**

Wartość nie. Określanie dowolnego nazwy użytkownika i hasła dla uwierzytelniania podstawowego mechanizm nie jest obecnie obsługiwane.


**P: jakie wersji usług AD DS są obsługiwane przez Azure AD łączenie zdrowia w usługach AD DS?**

Monitorowanie usług AD DS jest obsługiwana, gdy zainstalowana na następujących wersji systemu operacyjnego:

- Windows Server 2008 R2
- System Windows Server 2012
- Windows Server 2012 R2

## <a name="operations-questions"></a>Operacje na pytania



**P: czy potrzebujesz umożliwia inspekcję na serwerów Proxy aplikacji Moje AD FS lub Moje serwerów Proxy aplikacji sieci Web?**

Nie, inspekcja nie muszą być włączona na serwerach Proxy aplikacji sieci Web (WAP). Włącz go na serwerach usług AD FS.


**P: jak uzyskiwanie rozwiązany Azure AD łączenie kondycji alertów?**

Azure AD łączenie alerty kondycji uzyskiwanie rozpoznać pod warunkiem sukcesu. Azure AD łączenie agentów kondycji wykrywanie i zgłoś warunków sukcesu do usługi w regularnych odstępach czasu. Kilka alertów zwalczaniu jest opartych na czasie. Innymi słowy Jeśli tego samego warunku błędu nie zostaną spełnione w ciągu 72 godzin od Generowanie alertów, alert jest automatycznie rozwiązany.




**P: jakie portów zapory należy otworzyć Azure AD łączenie Agent kondycji do pracy?**

Musisz mieć porty TCP/UDP 443 i 5671 otwarty Azure AD łączenie Agent kondycji mieć możliwość komunikowania się z punktów końcowych usługi Azure AD kondycji.


**P: Dlaczego wyświetlać dwa serwery o takiej samej nazwie w Azure AD łączenie kondycji portalu?**

Po usunięciu agenta z serwera, serwer nie są automatycznie usuwane z Portal Azure łączenie AD.  Jeśli ręcznie usunięte agenta z serwera lub usunięta z serwera, należy ręcznie usunąć pozycję serwera z portal Azure AD łączenie kondycji. Aby uzyskać więcej informacji, przejdź do [Usuń wystąpienie serwera lub usługi.](active-directory-aadconnect-health-operations.md#delete-a-server-or-service-instance)

Jeśli reimaged serwer lub utworzyć nowy serwer z tym samym szczegóły (na przykład nazwa komputera) i nie usunięcia już zarejestrowanego serwera z portal Azure AD łączenie kondycji zainstalowany agent na nowym serwerze mogą być wyświetlane dwa wpisy o takiej samej nazwie.  
W tym przypadku należy usunąć wpis należących do serwera starszych ręcznie. Dane dla tego serwera powinny być nieaktualne.

## <a name="related-links"></a>Łącza pokrewne

* [Azure AD Connect kondycji](active-directory-aadconnect-health.md)
* [Azure AD Connect instalacji agenta kondycji](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect operacje kondycji](active-directory-aadconnect-health-operations.md)
* [Podłącz za pomocą Azure AD kondycji z usług AD FS](active-directory-aadconnect-health-adfs.md)
* [Przy użyciu Azure AD łączenie kondycji dla synchronizacji](active-directory-aadconnect-health-sync.md)
* [Za pomocą Azure AD nawiązywanie połączenia z usługami AD DS kondycji](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect kondycji Historia wersji](active-directory-aadconnect-health-version-history.md)
