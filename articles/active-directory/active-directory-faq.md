<properties
    pageTitle="Często zadawane pytania dotyczące usługi Azure Active Directory | Microsoft Azure"
    description="Azure Active Directory — często zadawane pytania zawierającego odpowiedzi na pytania w połączeniu z uzyskiwaniem dostępu do Azure i usługi Azure Active Directory hasło zarządzania i aplikacji programu access."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="markusvi"/>

# <a name="azure-active-directory-faq"></a>Często zadawane pytania dotyczące usługi Azure Active Directory

Azure Active Directory jest pełna tożsamości jako rozwiązanie usługi (IDaaS), które obejmuje wszystkie aspekty tożsamości, zarządzanie dostępem i zabezpieczeń.


Aby uzyskać więcej informacji, zobacz [Co to jest Azure Active Directory?](active-directory-whatis.md).



## <a name="accessing-azure-and-azure-active-directory"></a>Uzyskiwanie dostępu do Azure i Azure Active Directory


**P: Dlaczego otrzymuję "znaleziono żadnych subskrypcji" podczas próby otworzenia Azure AD w portalu klasyczny Azure (https://manage.windowsazure.com)?**

**A:** Uzyskiwanie dostępu do portalu klasyczny Azure wymaga poszczególnym użytkownikom uprawnień w subskrypcji usługi Azure. Jeśli masz płatnej usługi Office 365 lub Azure AD przejdź do [http://aka.ms/accessAAD](http://aka.ms/accessAAD) kroku jednorazowej aktywacji, w przeciwnym razie należy aktywować pełny [Azure wersji próbnej](https://azure.microsoft.com/pricing/free-trial/) lub płatnej subskrypcji. 

Aby uzyskać więcej informacji zobacz:

- [W jaki sposób Azure subskrypcje są skojarzone z usługi Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- [Zarządzanie katalogu dla subskrypcji usługi Office 365, platformy Azure](active-directory-manage-o365-subscription.md)

---

**P: co to jest powiązanie Azure AD usługi Office 365 i Azure?**

**A:** Azure Active Directory oferuje typowe funkcje tożsamości i dostęp do wszystkich usług Microsoft online services. Czy korzystasz z usługi Office 365, Microsoft Azure, Intune lub innych osób, już używasz Azure AD włączyć logowania jednokrotnego i uzyskać dostęp do zarządzania dla wszystkich tych usług. 

W rzeczywistości wszystkich użytkowników, dla których włączono dla usług Microsoft Online są określane jako kontami użytkowników w jedno lub więcej wystąpień Azure AD. Możesz włączyć te funkcje kont bezpłatnie Azure AD, takie jak dostęp do aplikacji w chmurze.
 
Ponadto Azure AD spłacona usług (np.: podstawowe Azure AD, Premium, EMS, itd.) uzupełnienia innych Online usług, takich jak usługi Office 365 i Microsoft Azure enterprise pełna skala rozwiązań zarządzania i zabezpieczenia.


---



## <a name="getting-started-with-hybrid-azure-ad"></a>Wprowadzenie do hybrydowego Azure AD


**P: jak połączyć Moje katalogu lokalnego do Azure AD**

**A:** Azure AD przy użyciu **Azure AD Connect**można nawiązać katalogu lokalnego. 

Aby uzyskać więcej szczegółowych informacji zobacz [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).


---

**P: jak skonfigurować logowania jednokrotnego między Moje katalogu lokalnego i aplikacje w chmurze?**

**A:** Musisz skonfigurować logowania jednokrotnego między katalogu lokalnego i Azure AD. Jak aplikacje w chmurze dostępu za pomocą Azure AD, usługa automatycznie dyski użytkownikom poprawnie uwierzytelnienia przy użyciu poświadczeń lokalnego.

Implementowania logowania jednokrotnego ze źródeł lokalnych można łatwo osiągnąć przy użyciu rozwiązań Federacji, na przykład ADFS lub przez skonfigurowanie synchronizacji mieszania haseł. Można łatwo rozmieścić obu opcji za pomocą Kreatora konfiguracji Azure AD Connect.
  

Aby uzyskać więcej szczegółowych informacji zobacz [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).
  

---

**P: czy Azure Active Directory zapewnia portalu dla użytkowników w mojej organizacji?**

**A:** Tak, usługi Azure Active Directory oferuje [Panel Azure AD dostępu](http://myapps.microsoft.com) dla użytkownika samoobsługowego i dostęp do aplikacji. Jeśli jesteś klientem usługi Office 365 można znaleźć wiele takie same możliwości w portalu usługi Office 365. 

Aby uzyskać więcej informacji zobacz [Wprowadzenie do Panel dostępu](active-directory-saas-access-panel-introduction.md). 



---

**P: czy Azure AD zalety zarządzania infrastrukturą mojej lokalnej?**

**A:** Tak, czy. Azure AD Premium edition umożliwia **Łączenie kondycji**. Azure AD łączenie kondycji ułatwia monitorowanie i lepszy wgląd w infrastrukturze tożsamości lokalnych i usługi synchronizacji.  

Aby uzyskać więcej informacji zobacz [Monitora sieci lokalnej usługi infrastruktury i synchronizacji tożsamości w chmurze](active-directory-aadconnect-health.md).  

---

## <a name="password-management"></a>Zarządzanie hasłami

**P: czy mogę używać hasła Azure AD zapisu z powrotem bez synchronizacji haseł? (CZYLI, chcę Azure AD SSPR za pomocą hasła zapisu z powrotem ale nie chcę, aby moje hasła zapisane w cloud?)**

**A:** Nie należy synchronizować haseł AD do Azure AD w celu umożliwienia zapisu Wstecz. W środowisku federacyjnym logowania jednokrotnego usługi Azure AD korzysta z katalogu lokalnego do uwierzytelnienia użytkownika. W tym scenariuszu nie jest wymagane hasło lokalnego mają być śledzone w Azure AD.

---

**P: jak długo trwa hasło ponownie w celu zapisania AD lokalnego?**

**A:** Hasło zapisu z powrotem działa w czasie rzeczywistym. 

Aby uzyskać więcej informacji zobacz [Wprowadzenie do zarządzania hasłami](active-directory-passwords-getting-started.md) 


---

**P: czy mogę używać hasła zapisu z powrotem przy użyciu hasła, które są zarządzane przez administratora**

**A:** Tak, jeśli masz hasła zapisu z powrotem włączyć operacje hasło wykonane przez administratora są zapisywane w środowisku lokalnym.  

Więcej odpowiedzi na hasło powiązane pytania, zobacz [Hasło zarządzania często zadawane pytania](active-directory-passwords-faq.md).

---

## <a name="application-access"></a>Dostęp do aplikacji


**P: gdzie można znaleźć listę aplikacji, które są wstępnie zintegrowane z usługą Azure Active Directory i możliwości?**

**A:** Azure AD obejmuje ponad 2600 wstępnie zintegrowanych aplikacji firmy Microsoft, dostawców usług aplikacji lub partnerów. Wszystkie wstępnie zintegrowanych aplikacji obsługuje logowania jednokrotnego. Logowania jednokrotnego umożliwia organizacji poświadczenia, aby uzyskać dostęp do aplikacji. Niektóre aplikacje obsługują także automatycznego inicjowania obsługi administracyjnej oraz Anuluj inicjowania obsługi administracyjnej

Aby uzyskać pełną listę wstępnie zintegrowane aplikacje zobacz [Marketplace usługi Active Directory](https://azure.microsoft.com/marketplace/active-directory/).


---

**P: co się stanie, jeśli aplikacji, potrzebne jest rynku Azure AD?**

**A:** Dzięki usłudze Azure AD Premium można dodać i skonfigurować dowolną aplikację, którą chcesz. W zależności od możliwości aplikacji i preferencje można skonfigurować logowania jednokrotnego i automatycznego inicjowania obsługi administracyjnej.  

Aby uzyskać więcej informacji zobacz:

- [Konfigurowanie rejestracji jednokrotnej dla aplikacji, które nie znajdują się w galerii aplikacji usługi Azure Active Directory](active-directory-saas-custom-apps.md)
- [Aby włączyć automatyczne inicjowania obsługi administracyjnej użytkowników i grup z usługi Azure Active Directory do aplikacji przy użyciu SCIM](active-directory-scim-provisioning.md) 


---

**P: jak użytkownikom zalogować się do aplikacji przy użyciu usługi Azure Active Directory?**
 
**A:** Usługi Azure Active directory oferuje kilka możliwości użytkownikom wyświetlanie i dostęp do swoich aplikacji, takich jak:

- Panel dostępu Azure AD

- Uruchamianie aplikacji usługi Office 365

- Bezpośrednie logowania w aplikacji federacyjnych

- Głębokości łącza do federacyjnych, oparte na hasła lub istniejącej aplikacji

Aby uzyskać więcej informacji zobacz [Wdrażanie Azure AD zintegrowane aplikacje do użytkowników](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).


---

**P: co to są różne sposoby usługi Azure Active Directory umożliwia uwierzytelnianie i rejestracji jednokrotnej dla aplikacji?**
 
**A:** Azure Active Directory obsługuje wiele standardowych protokoły dla uwierzytelniania i autoryzacji, takich jak SAML 2.0, łączenie OpenID OAuth 2.0 i federacyjnych. Azure AD obsługuje także archiwizowanie zawartości hasło i możliwości automatycznego logowania dla aplikacji, które obsługują tylko uwierzytelniania opartego na formularzach.  

Aby uzyskać więcej informacji zobacz:

- [Scenariusze uwierzytelniania Azure AD](active-directory-authentication-scenarios.md)

- [Protokoły uwierzytelniania usługi Active Directory](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [Jak pojedynczy logowania jednokrotnego z pracą usługi Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**P: czy mogę dodać aplikacje na komputerze jest uruchomiona w lokalnej?**

**A:** Azure AD serwera Proxy aplikacji umożliwia łatwe i bezpieczny dostęp do aplikacji sieci web w lokalnej, które możesz wybrać. Te aplikacje są dostępne w taki sam sposób uzyskujesz dostęp do aplikacji władz akredytacji bezpieczeństwa w usłudze Azure Active Directory. Istnieje potrzeba sieci VPN lub zmieniania infrastruktury sieciowej.  

Aby uzyskać więcej informacji zobacz [jak bezpieczny dostęp zdalny do aplikacji lokalnego](active-directory-application-proxy-get-started.md).


--- 

**P: jak wymaga MFA użytkownikom uzyskiwanie dostępu do określonej aplikacji?**

**A:** Za pomocą Azure AD dostępu warunkowego można przypisać zasady dostępu unikatowe dla każdej aplikacji. W przypadku zasad można wymagać MFA przez cały czas lub gdy użytkownicy nie jest podłączony do sieci lokalnej.  

Aby uzyskać więcej informacji zobacz [Zabezpieczanie dostępu do usługi Office 365 i innych aplikacji połączony z usługi Azure Active Directory](active-directory-conditional-access.md).


---

**P: co to jest automatyczne przypisywanie użytkowników w przypadku aplikacji władz akredytacji bezpieczeństwa?**

**A:** Azure Active Directory pozwala zautomatyzować tworzenie, konserwacja i usuwanie tożsamości użytkowników w wielu aplikacjach chmury popularne (władz akredytacji bezpieczeństwa). 

Aby uzyskać więcej informacji zobacz [Przypisywanie użytkowników zautomatyzować i Deprovisioning do władz akredytacji bezpieczeństwa aplikacji z usługą Azure Active Directory](active-directory-saas-app-provisioning.md)

---



