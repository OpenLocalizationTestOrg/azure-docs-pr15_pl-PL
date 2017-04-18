<properties
    pageTitle="Definiowanie Azure Active Directory hybrydowych tożsamości zagadnienia projektowe - strategię ochrony danych | Microsoft Azure"
    description="Będzie Definiowanie strategię ochrony danych dotyczących rozwiązania hybrydowego tożsamości do wymagań biznesowych zdefiniowane."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Definiowanie strategię ochrony danych dotyczących rozwiązania hybrydowego tożsamości

W tym zadaniu będzie zdefiniować strategię ochrony danych dotyczących hybrydowych tożsamości rozwiązania do wymagań firm zdefiniowanego w:

- [Określanie wymagania dotyczące ochrony danych](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
- [Określenie wymagań dotyczących zarządzania zawartością](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
- [Określanie wymagania kontroli dostępu](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
- [Określenie wymagań odpowiedzi wystąpieniu zdarzenia](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Definiowanie opcji ochrony danych
Jak wyjaśniono w [wymagania dotyczące synchronizacji katalogów Określanie](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)Microsoft Azure AD może być synchronizowana z usługach domenowych Active Directory (AD DS) znajduje się w lokalnej. Integracja umożliwia organizacjom wykorzystać Azure AD do weryfikacji poświadczeń użytkownika w przypadku, gdy chce uzyskać dostęp do zasobów firmy. Można to zrobić w obu przypadkach: danych na pozostałych lokalnej i w chmurze.  Dostęp do danych w Azure AD wymaga uwierzytelniania użytkowników za pomocą usługi tokenu zabezpieczającego (STS).

Po uwierzytelnieniu głównej nazwy użytkownika (UPN) jest odczyt token uwierzytelniania i partycją zreplikowanej i kontener domeny użytkownika odpowiadające jest określona. Informacje dotyczące istnienia, stan włączenia i roli użytkownika jest używany przez system autoryzacji do określenia, czy żądany dostęp do dzierżawy docelowej jest autoryzowany dla tego użytkownika w tej sesji. Niektóre akcje autoryzowanych (w szczególności tworzenie użytkowników, Resetuj hasła) tworzenie dziennik inspekcji, który może być używany przez administratora dzierżawy do zarządzania staraniach zgodności lub dochodzenia.

Przenoszenie danych z centrum danych lokalnych do magazynu Azure przez połączenie internetowe mogą nie być możliwe z powodu woluminu danych, dostępność przepustowości lub inne zagadnienia. [Usługa Importuj/Eksportuj magazynu Azure](../storage/storage-import-export-service.md) udostępnia opcję związane ze sprzętem do wprowadzania i pobierania dużych ilości danych w magazynie obiektów blob. Umożliwia wysyłanie [zaszyfrowanych funkcją BitLocker](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) dysków twardych bezpośrednio do Azure centrum danych miejsce, w którym operatorów chmury będzie przekazać zawartość do swojego konta miejsca do magazynowania lub mogą pobrać Azure danych na dysku, aby powrócić do Ciebie. Tylko zaszyfrowanych dysków są akceptowane dla tego procesu (za pomocą klawisza funkcji BitLocker wygenerowany przez usługę podczas konfigurowania zadania). Klucz funkcji BitLocker zapewnia Azure oddzielnie, zapewniając poza pasek klucza udostępniania.

Ponieważ danych podczas przesyłania może się odbywać w różnych scenariuszach, dotyczy również wiedzieć, że Microsoft Azure używa [wirtualnej sieci](https://azure.microsoft.com/documentation/services/virtual-network/) do wyodrębnienia ruchu dzierżaw ze sobą, zastosowanie miar, takich jak zapory poziomie hosta i gościa, filtrowanie pakietów IP, blokowanie portów i punkty końcowe HTTPS. Większość komunikacji wewnętrznej Azure firmy, w tym infrastruktury do infrastruktury i infrastruktury do klienta (lokalnego), również są szyfrowane. Inny scenariusz ważne jest komunikacji w ramach Azure centrach danych; Microsoft zarządza sieci w celu zapewnienia, że nie maszyn wirtualnych można personifikacji lub podsłuchiwać adres IP innego. TLS/SSL jest używana podczas uzyskiwania dostępu do przechowywania Azure lub bazy danych SQL lub podczas nawiązywania połączenia z usługami w chmurze. W takim przypadku administrator klienta jest odpowiedzialny za uzyskiwanie certyfikatu TLS/SSL i wdrożenie go na infrastruktury dzierżawy. Przenoszenie ruch danych między maszyn wirtualnych w tym samym wdrożenia lub między dzierżawami w jednym wdrożenia za pośrednictwem wirtualnej sieci Microsoft Azure mogą być chronione za pomocą protokołów komunikacji zaszyfrowanej przykład HTTPS, SSL/TLS lub innych osób.

W zależności od tego, jak możesz odpowiedzi na pytania w [Określanie wymagań ochrony danych](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)możesz należy określić, jak ma być ochrona danych i jak rozwiązanie tożsamości hybrydowych pomoże Ci na tym. W tabeli przedstawiono opcje obsługiwane przez Azure, które są dostępne dla każdego scenariusza ochrony danych.


| Opcje ochrony danych         | W pozostałych w chmurze | W pozostałych lokalnego | Podczas przesyłania |
|---------------------------------|----------------------|---------------------|------------|
| Szyfrowanie dysków funkcją BitLocker      | X                    | X                   |            |
| Program SQL Server szyfrowanie bazy danych | X                    | X                   |            |
| Szyfrowanie maszyn wirtualnych do maszyn wirtualnych             |                      |                     | X          |
| PROTOKÓŁ SSL/TLS                         |                      |                     | X          |
| SIECI VPN                             |                      |                     | X          |


>[AZURE.NOTE]
W tym artykule [zgodności przez funkcję](https://azure.microsoft.com/support/trust-center/services/) w [Centrum zaufania programu Microsoft Azure](https://azure.microsoft.com/support/trust-center/) , aby dowiedzieć się więcej o certyfikaty, które jest zgodne z każdej usługi Azure.
W związku z opcjami ochrona danych za pomocą wielowarstwowego podejście, porównanie te opcje nie są stosowane do tego zadania. Upewnij się, korzystają wszystkie opcje dostępne dla każdego Województwo, z którego będą dane.

## <a name="define-content-management-options"></a>Definiowanie opcji zarządzania zawartością
Jedną z zalet stosowania Azure AD zarządzania infrastrukturą tożsamości hybrydowego jest całkowicie przezroczyste z perspektywy użytkownika końcowego procesu. Użytkownik będzie próbował uzyskać dostęp do zasobów współużytkowanych, zasób wymaga uwierzytelniania, użytkownik będzie miał wysłać żądanie uwierzytelnienia do Azure AD w celu uzyskania tokenu i uzyskać dostęp do zasobu. Całego procesu się dzieje w tle, bez interakcji użytkownika. Użytkownik może również udzielanie uprawnień do [grupy](active-directory-manage-groups.md#getting-started-with-access-management) użytkowników w celu umożliwienia im wykonywania niektórych typowych akcji.

Organizacji, które są zwykle dotyczy prywatności danych wymagają klasyfikacji danych dotyczących ich rozwiązania. Jeśli bieżący infrastruktury lokalnego już używa danych klasyfikacji, prawdopodobnie wykorzystać Azure AD jako główne repozytorium dla tożsamości użytkownika. Typowe narzędzia jest używane w lokalnej klasyfikacji danych jest nazywany [Narzędzi klasyfikacji danych](https://msdn.microsoft.com/library/Hh204743.aspx) dla systemu Windows Server 2012 R2. To narzędzie może pomóc zidentyfikować, będzie on klasyfikować i ochrony danych na serwerach plików w chmurze sieci prywatnej. Użytkownik może również korzystać z [Automatyczną klasyfikację pliku](https://technet.microsoft.com/library/hh831672.aspx) w systemie Windows Server 2012, aby osiągnąć ten cel.

Jeśli Twoja organizacja nie ma klasyfikacji danych w miejscu, ale należy chronić poufne pliki bez dodawania nowego serwerów lokalnych, mogą również używać Microsoft [Azure usługi zarządzania prawami dostępu](https://technet.microsoft.com/library/JJ585026.aspx).  Azure RMS używa szyfrowania, tożsamości i zasady autoryzacji do zabezpieczania plików i wiadomości e-mail i działa na wielu urządzeniach — telefony, tabletów i komputerów. Ponieważ usługą Azure RMS jest usługi w chmurze, istnieje bez konieczności wyraźnie skonfigurować relacje zaufania z innymi organizacjami przed udostępnieniem zawartości chronionej z nimi. Jeśli już mają usługi Office 365 lub katalogu Azure AD, współpracy w organizacji automatycznie jest obsługiwana. Można także synchronizować tylko atrybutów katalogu, które usługą Azure RMS musi obsługiwać tożsamości wspólnych dla konta usługi Active Directory w lokalnej za pomocą usługi Azure do synchronizacji Active Directory (AAD Sync) lub narzędzie Azure AD Connect.

Jest ważną zarządzania zawartością, aby dowiedzieć się, kto uzyskuje dostęp do zasobów, dlatego funkcja rejestrowania sformatowanego jest ważna w przypadku Zarządzanie tożsamościami. Azure AD udostępnia dziennika ponad 30 dni, w tym:

- Zmiany w członkostwo w roli (ex: użytkownik dodany do roli administratora globalnego)
- Poświadczeń aktualizacji (ex: zmiany hasła)
- Zarządzania domeną (ex: Sprawdzanie domeny niestandardowej, usunięcie domeny)
- Dodawanie lub usuwanie aplikacji
- Zarządzanie użytkownikami (ex: Dodawanie, usuwanie, aktualizowanie użytkownika)
- Dodawanie lub usuwanie licencji

>[AZURE.NOTE]
Przeczytaj [Zarządzanie dziennika inspekcji i zabezpieczeń firmy Microsoft Azure](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) Aby dowiedzieć się więcej o możliwości rejestrowania platformy Azure.
W zależności od tego, jak możesz odpowiedzi na pytania w sekcji [Określanie wymagania zarządzania zawartością](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)możesz należy określić, jak zawartość, aby zarządzać w rozwiązaniu tożsamości hybrydowych. Podczas wszystkich opcji widocznych w tabeli 6 są w stanie integracji z usługą Azure Active Directory, należy określić, która jest bardziej odpowiednie dla potrzeb firmy.

| Opcji zarządzania zawartością                                                               | Zalety                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Wady                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Scentralizowane lokalnego (serwer zarządzania prawami usługi Active Directory)                      | Pełna kontrola nad infrastrukturą serwera odpowiedzialny za klasyfikacji danych <br> Wbudowanej funkcji w systemie Windows Server, bez potrzeby dodatkowych licencji lub subskrypcji <br> Można zintegrować z usługą Azure Active Directory w scenariuszu hybrydowego <br> Obsługa informacji o funkcjach zarządzania (IRM) prawami w usługach Online firmy Microsoft, takich jak usługi Exchange Online i SharePoint Online, a także usługi Office 365 <br> Obsługuje lokalnego produktów serwerowych firmy Microsoft, takich jak programu Exchange Server, program SharePoint Server i serwery plików, z systemem Windows Server i infrastruktury klasyfikacji pliku (FCI). | Wyższa, konserwacji (razem w górę z aktualizacji, konfiguracji i potencjalne uaktualnienia), od IT właścicielem serwera <br> Wymaganie infrastruktury serwera lokalnego<br> Doesn'tleverage Azure możliwości macierzyste                                     |
| Scentralizowane w chmurze (Azure RMS)                                                     | Łatwiejsze do zarządzania w porównaniu z lokalnego rozwiązania <br> Może zostać zintegrowany z usługami AD DS w scenariuszu hybrydowego <br>  W pełni zintegrowany z usługą Azure Active Directory <br> Nie wymaga lokalnego serwera wdrażaniu usługi <br> Obsługa lokalnych produktów serwerowych firmy Microsoft, takich jak serwera Exchange, programu SharePoint Server i serwery plików, z systemem Windows Server i plik klasyfikacji, infrastruktura (FCI) <br> IT, może mieć pełną kontrolę nad klucza ich dzierżawy z możliwością BYOK.                                                                                    | Twoja organizacja musi mieć subskrypcji chmury, która obsługuje usługi RMS <br> Twoja organizacja musi mieć katalog Azure AD do obsługi uwierzytelniania użytkowników w przypadku usługi RMS                                                                                  |
| Hybrydowe (zintegrowane z usługą Azure RMS, lokalnego Active Directory Rights Management Server) | W tym scenariuszu sumuje zalet obu scentralizowane lokalnej i w chmurze.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Twoja organizacja musi mieć subskrypcji chmury, która obsługuje usługi RMS <br> Twoja organizacja musi mieć katalog Azure AD do obsługi uwierzytelniania użytkowników w przypadku usługi RMS, <br> Wymaga połączenia między usługi w chmurze Azure i lokalnych infrastruktury |

## <a name="define-access-control-options"></a>Definiowanie opcji kontroli dostępu
Dzięki zastosowaniu uwierzytelniania, autoryzacji i access kontroli funkcji dostępnych w Azure AD można włączyć Twojej firmy na używanie logowania jednokrotnego (SSO), jak pokazano na poniższej ilustracji przy użyciu repozytorium centralnej tożsamości umożliwienie użytkownikom i partnerów:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Centralne zarządzanie i w pełni Integracja z innych katalogów

Azure Active Directory zawiera rejestracji jednokrotnej do tysięcy aplikacji władz akredytacji bezpieczeństwa i aplikacji sieci web w lokalnej. Przeczytaj [listę zgodności usługi Azure Active Directory federation: dostawców tożsamości innych firm, które mogą być używane w celu wdrożenia logowania jednokrotnego](https://msdn.microsoft.com/library/azure/jj679342.aspx) artykuł, aby uzyskać więcej informacji o rejestracji Jednokrotnej firmy, które zostały przetestowane przez firmę Microsoft. Ta funkcja umożliwia organizacji do wykonania różnych scenariuszy B2B przy zachowaniu kontroli nad zarządzania tożsamości i access. Jednak podczas B2B proces projektowania ważne jest, aby zrozumieć metody uwierzytelniania, które będą używane przez partnera i sprawdź ta metoda jest obsługiwana przez Azure. Obecnie są obsługiwane przez Azure AD:

- Zabezpieczenia potwierdzenia Markup Language (SAML)
- OAuth
- Protokół Kerberos
- Tokeny
- Certyfikaty


>[AZURE.NOTE] w tym artykule [Azure Active Directory protokoły](https://msdn.microsoft.com/library/azure/dn151124.aspx) , aby dowiedzieć się więcej szczegółów dotyczących każdego protokołu i jego możliwości platformy Azure.

Korzystanie z obsługi Azure AD, aplikacji dla urządzeń przenośnych firmy, można użyć samego łatwe obsługi uwierzytelniania usług Mobile umożliwia pracownikom zalogować się do ich aplikacji dla urządzeń przenośnych przy użyciu poświadczeń usługi Active Directory firmy. Za pomocą tej funkcji Azure AD jest obsługiwany jako dostawcy tożsamości w usługach Mobile wraz z innych dostawców tożsamości już obsługiwane (zawierające Accounts Microsoft, identyfikator Facebook, identyfikator Google i serwisu Twitter identyfikator). Jeśli aplikacje lokalnego używa poświadczeń użytkownika znajdujący się w usługach AD DS firmy, dostęp z partnerami i użytkowników pochodzące z chmury powinny być przezroczysty. Możesz zarządzać kontrolę warunkowe dostępu użytkownika do aplikacji sieci web (opartych na chmurze), interfejsu API sieci web, usługami w chmurze firmy Microsoft, 3 aplikacji władz akredytacji bezpieczeństwa i klientami (urządzeń przenośnych) aplikacji i mają zalety zabezpieczeń, inspekcji, raportowania w jednym miejscu. Jednak zaleca się poprawność to w środowisku produkcyjnym niezwiązanych z ograniczonym użytkowników.


>[AZURE.TIP] należy wspomnieć, że Azure AD nie ma zasad grupy w usługach AD DS zawierającej. Aby wymusić zasady dla urządzeń będzie potrzebne rozwiązanie do zarządzania urządzeń przenośnych, takich jak [Usługi Microsoft](https://technet.microsoft.com/library/jj676587.aspx).

Po uwierzytelnieniu użytkownika przy użyciu Azure AD należy ocenić poziom dostępu, że użytkownik uzyskuje go. Poziom dostępu, który użytkownik uzyskuje przez zasób mogą się różnić, Azure AD można dodawać warstwy dodatkowe zabezpieczenia kontrolowanie dostępu do niektórych zasobów, również należy pozostawić Pamiętaj, że sam zasób można także używać własnej listy kontroli dostępu oddzielnie, takich jak kontrola dostępu do plików znajdujących się na serwerze plików. Na poniższym rysunku przedstawiono poziomy kontroli dostępu, zawierające w scenariuszu hybrydowych:

![](./media/hybrid-id-design-considerations/accesscontrol.png)


Każdy interakcji na diagramie pokazano na ilustracji X oznacza jeden scenariusz kontrolki programu access mogą być objęte Azure AD. Poniżej masz opis każdego scenariusza:

1. warunkowe dostęp do aplikacji, które są obsługiwane w lokalnej: można używać urządzeń zarejestrowanych z zasady dostępu dla aplikacji, które są skonfigurowane do korzystania z systemu Windows Server 2012 R2 usług AD FS. Aby uzyskać więcej informacji na temat konfigurowania dostępu warunkowego dla lokalnego zobacz [Konfigurowanie warunkowego dostępu do lokalnego przy użyciu Azure Active Directory Rejestracja urządzenia](active-directory-conditional-access-on-premises-setup.md).
2. ACL do portalu zarządzania Azure: Azure ma także możliwość kontrolowania dostępu do portalu zarządzania przy użyciu RBAC (kontrola dostępu oparta roli). Ta metoda umożliwia firmie ograniczyć liczbę operacji, które umożliwiają wykonywanie osoba, gdy ma on dostęp do portalu zarządzania Azure. Przy użyciu RBAC kontrolowanie dostępu do portalu, Administratorzy urzędu certyfikacji pełnomocnictw, korzystając z poniższych metod zarządzania programu access:

 - Przypisanie roli oparte na grupach: programu access można przypisać grupom Azure AD, które mogą być synchronizowane z lokalną usługą Active Directory. Pozwala na korzystanie z istniejących inwestycji, wprowadzone w narzędzia i procesów do zarządzania grupami Twojej organizacji. Umożliwia także funkcję zarządzania delegowanej grupy Azure AD premii.
 - Wbudowana ról w Azure dźwigni: są dostępne trzy role — właściciel współautorów i Reader zapewnienie użytkownikom i grupom uprawnień tylko zadania, które są potrzebne do wykonywania zadań.
 - Szczegółowego dostęp do zasobów: możesz przypisać role użytkowników i grup dla danej subskrypcji, grupa zasobów lub poszczególnych zasobów Azure, na przykład w witrynie sieci Web lub bazy danych. W ten sposób można upewnij się, że użytkownicy mają dostęp do wszystkich zasobów, które są potrzebne i Brak dostępu do zasobów, które nie do zarządzania.

>[AZURE.NOTE] w tym artykule [Kontrola dostępu oparta na rolach w Azure](https://azure.microsoft.com/updates/role-based-access-control-in-azure-preview-portal/) , aby dowiedzieć się więcej informacji na temat tej funkcji. Dla deweloperów, którzy są tworzenia aplikacji i chcesz, aby dostosować kontrola dostępu dla nich użytkownik może również być Azure AD ról aplikacji autoryzacji. Przejrzyj w tym [przykładzie aplikacji sieci Web — RoleClaims-DotNet](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) dotyczące tworzenia aplikacji używać tej funkcji.

3.w warunkowego dostępu dla aplikacji usługi Office 365 za pomocą Intune firmy Microsoft: Administratorzy umożliwia obsługę zasady urządzenia dostępu warunkowego zabezpieczania zasobów firmy przy jednoczesnym zezwalania na zgodny z urządzenia, aby uzyskać dostęp do usług pracowników przetwarzających informacje. Aby uzyskać więcej informacji zobacz [Zasady warunkowe urządzenia dostępu dla usług Office 365](active-directory-conditional-access-device-policies.md).

4. warunkowego dostępu dla aplikacji władz akredytacji bezpieczeństwa: [Ta funkcja](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) umożliwia skonfigurowanie reguły dostępu uwierzytelnianie wieloskładnikowe każdej aplikacji i możliwości zablokować dostęp do użytkowników spoza sieci zaufanych. Możesz zastosować reguły uwierzytelnianie wieloskładnikowe dla wszystkich użytkowników, które są przypisane do aplikacji lub tylko dla użytkowników w określonych grup zabezpieczeń. Użytkownicy mogą być wyłączone z wymaganie uwierzytelnianie wieloskładnikowe, jeśli uzyskują dostęp do aplikacji z adresu IP, który w wewnątrz organizacji w sieci.

W związku z opcjami kontroli dostępu za pomocą wielowarstwowego podejście, porównanie te opcje nie są stosowane do tego zadania. Upewnij się, że są używanie wszystkie opcje dostępne dla każdego scenariusza, który wymaga kontroli dostępu do zasobów.

## <a name="define-incident-response-options"></a>Umożliwia określenie opcji odpowiedzi wystąpieniu zdarzenia
Azure AD mogą pomóc IT tożsamości potencjalne ryzyko związane z zabezpieczeniami w środowisku przez monitorowanie aktywności użytkownika, IT mogą korzystać z Azure AD dostępu i zastosowania raportów możliwość uzyskania wgląd integralności i bezpieczeństwa katalogu organizacji. Przy użyciu tych informacji administrator systemu informatycznego może lepiej określić miejsce, w którym może znajdować się możliwe ryzyko związane z zabezpieczeniami, tak aby były odpowiednio można zaplanować ich eliminowania.  [Azure AD Premium subskrypcja](active-directory-get-started-premium.md) obejmuje zestaw raportów zabezpieczeń, które umożliwiają IT w celu uzyskania tych informacji. [Azure AD raporty](active-directory-view-access-usage-reports.md) są posortowane, tak jak pokazano poniżej:

- **Raporty anomalii**: zawierać znak wydarzeń znajdujące się anomalous. Celem jest sprawić, że takie aktywność i umożliwiają mieć możliwość określenia, czy zdarzenie jest podejrzany.
- **Raport zintegrowanej aplikacji**: zawiera spostrzeżeń jak aplikacje w chmurze są używane w Twojej organizacji. Azure Active Directory oferuje integrację z tysiące aplikacje w chmurze.
- **Raporty o błędach**: wskazują błędy, które mogą wystąpić podczas inicjowania obsługi administracyjnej konta do aplikacji zewnętrznych.
- **Raporty dotyczące użytkownika**: wyświetlanie urządzenia i logowania w dane działania dla określonego użytkownika.
- **Dzienniki aktywności**: zawierają wszystkie inspekcji zdarzeń w ostatnich 24 godzin, ostatnich 7 dni lub ostatnich 30 dni, a także zmian aktywności grup i resetowanie i rejestracji aktywności hasło.

>[AZURE.TIP]
Raport [użytkownika z poświadczeń przecieku](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) jest inny raport, który pomaga zespołu odpowiedzi zdarzeniem pracy nad sprawą.  Ten raport wyświetla zgodne wyniki między wykaz przecieku poświadczenia i dzierżawy usługi.

Inne ważne wbudowana Azure AD, który może być używany podczas analizy odpowiedzi zdarzenia w raportach i są:

- **Działania do resetowania hasła**: Podaj administrator spostrzeżeń jak aktywnie resetowania hasła jest używany w organizacji.
- **Aktywności rejestracji do resetowania hasła**: zawiera wniosków, do których użytkownicy zarejestrowano ich metody resetowania hasła, które metody zostało zaznaczone.
- **Działanie grupowe**: zawiera historii zmian do grupy (ex: użytkownicy dodane lub usunięte) które zostały zapoczątkowane w panelu programu Access.

Oprócz możliwości raportowania core dostępny w Azure AD Premium, które można użyć w trakcie odpowiedzi zdarzeniem dochodzenia IT mogą również korzystać z raportu inspekcji w celu uzyskania informacji, takich jak:

- Zmiany w członkostwo w roli (ex: użytkownik dodany do roli administratora globalnego)
- Poświadczeń aktualizacji (ex: zmiany hasła)
- Zarządzania domeną (ex: zweryfikować domenę niestandardową, usunięcie domeny)
- Dodawanie lub usuwanie aplikacji
- Zarządzanie użytkownikami (ex: Dodawanie, usuwanie, aktualizowanie użytkownika)
- Dodawanie lub usuwanie licencji

Ponieważ opcje zdarzenia odpowiedź użyj metody wielowarstwowego, porównanie te opcje nie są stosowane do tego zadania. Upewnij się, korzystają wszystkie opcje dostępne dla każdego scenariusza, który należy użyć funkcji raportowania Azure AD w ramach procesu zdarzenia odpowiedzi Twojej firmy.

## <a name="next-steps"></a>Następne kroki
[Określanie hybrydowych zadania związane z zarządzaniem tożsamości](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)


## <a name="see-also"></a>Zobacz też
[Omówienie zagadnienia projektu](active-directory-hybrid-identity-design-considerations-overview.md)
