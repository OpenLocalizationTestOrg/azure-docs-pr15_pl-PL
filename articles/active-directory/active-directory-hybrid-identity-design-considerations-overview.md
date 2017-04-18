<properties
    pageTitle="Azure Active Directory hybrydowych tożsamości zagadnienia projektowe — omówienie | Microsoft Azure"
    description="Omówienie i zawartości mapy Podręcznik zagadnienia dotyczące projektowania hybrydowych tożsamości"
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

# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Zagadnienia projektowe tożsamości hybrydowego usługi Azure Active Directory

Urządzenia z systemem dla klientów indywidualnych są proliferating świata firmy i aplikacji (władz akredytacji bezpieczeństwa) oprogramowania jako usługi opartej na chmurze są łatwe do przyjęcia. Dzięki temu zachowywanie kontroli nad dostępem użytkowników aplikacji wewnętrznych platformach centrach danych i chmury może być trudne.  Rozwiązania dla tożsamości firmy Microsoft zakresu lokalnego i możliwości oparte na chmurze, tworzenie tożsamości jednego użytkownika, dla uwierzytelniania i autoryzacji wszystkie zasoby, niezależnie od lokalizacji. Nazywamy tożsamości hybrydowych. Istnieje inny projekt i opcje konfiguracji dla tożsamości hybrydowych za pomocą rozwiązań firmy Microsoft, a w niektórych przypadkach może być trudno określić, najlepiej kombinację potrzeb danej organizacji. Ten przewodnik zagadnienia dotyczące projektowania tożsamości hybrydowych pomoże Ci Aby dowiedzieć się, jak zaprojektować rozwiązanie tożsamości hybrydowe, który najlepiej odpowiada firmy i technologii w firmie dla Twojej organizacji.  Ten przewodnik zawiera szczegółowe serii kroki i zadania, które można wykonać, aby ułatwiają projektowanie hybrydowych rozwiązania tożsamości spełniającego wymagania dotyczące organizacji. W całym kroki i zadania przewodnik przedstawi odpowiednich technologii i opcje funkcji dostępnych dla organizacji spotkania funkcjonalności i jakość usług (na przykład dostępność, skalowalność, wydajność, zarządzania i bezpieczeństwo) poziomu. W szczególności hybrydowych tożsamości projektu zagadnienia przewodnik cele mają odpowiedzieć na następujące pytania: 

- Jakie pytania należy do pytania i odpowiedzi na dysku hybrydowego tożsamości specyficzne dla domeny technologii lub problem czy najlepiej spełnia wymagania Moje?
- Jakie sekwencji czynności wykonania zaprojektować rozwiązanie tożsamości hybrydowego dla domeny technologii lub problemu 
- Jakie hybrydowych tożsamości technologii i konfiguracji opcje są dostępne może pomóc w mojej wymagań? Co to są korzystnych rozwiązań jedną z tych opcji, aby można było zaznaczyć najlepszym rozwiązaniem dla mojej firmy?


## <a name="who-is-this-guide-intended-for"></a>Kto jest przeznaczony ten przewodnik?
 CIO, CITO, dyrektora tożsamości architektów, Enterprise Architects i architektów IT odpowiedzialny za projektowanie rozwiązania hybrydowego tożsamości dla średnich i dużych organizacjach.

## <a name="how-can-this-guide-help-you"></a>Jak ten przewodnik pomoże Ci? 
Aby dowiedzieć się, jak zaprojektować rozwiązanie tożsamości hybrydowego można zintegrować z systemu zarządzania tożsamości oparte na chmurze z bieżącego lokalnego rozwiązania tożsamości, można użyć tego przewodnika. Poniższa grafiki przedstawia przykład hybrydowych rozwiązanie tożsamości, które umożliwia administratorom Zarządzaj, aby zintegrować ich bieżącego rozwiązania usługi Active Directory systemu Windows Server znajdującej się lokalnego przy użyciu usługi Microsoft Azure Active Directory aby umożliwić użytkownikom w aplikacjach znajduje się w chmurze i lokalnych za pomocą pojedynczego logowania jednokrotnego (SSO).

![](./media/hybrid-id-design-considerations/hybridID-example.png)


Przykładem jest używanie usług w chmurze do integracji z funkcjami lokalnego w celu udostępniania jednego środowiska do procesu uwierzytelniania użytkownika końcowego i, aby ułatwić rozwiązanie tożsamości hybrydowego jest ilustracji powyżej IT zarządzanie tych zasobów. Chociaż może być bardzo typowy scenariusz, projekt tożsamości hybrydowych każdej organizacji jest mogą być inne niż przykładzie przedstawionym na rysunku 1 z powodu odmienne wymagania. Ten przewodnik zawiera szereg kroki i zadania, które można wykonać, aby Projektowanie rozwiązania tożsamości hybrydowego, która spełnia wymagania dotyczące organizacji. Całym następujące kroki i zadania przewodnika wyświetlane odpowiednich technologii i funkcji dostępnych na spotkania funkcjonalne i wymagań poziomu jakości usługi dla Twojej organizacji.

**Założenia**: mają doświadczenie z systemem Windows Server, Active Directory Domain Services i usługi Azure Active Directory. W tym dokumencie przyjęto założenie, że szukasz jak te rozwiązania można potrzeb samodzielnie lub w rozwiązaniu zintegrowanym.

## <a name="design-considerations-overview"></a>Omówienie zagadnienia projektu
Niniejszy dokument zawiera zestaw kroki i zadania, które można wykonać, aby zaprojektować rozwiązanie tożsamości hybrydowe, które najlepiej odpowiada potrzebom. Czynności są prezentowane w sekwencji uporządkowanej. Zagadnienia, które informacje w dalszych krokach może być konieczna zmiana decyzji, wprowadzone w poprzednich krokach, jednak z powodu nakładania projekt opcje. Co próba alert potencjalne konflikty projektu w całym dokumencie. 

Zostanie obciążony projektu czy najlepiej spełnia wymagań dotyczących dopiero po iteracji kroki tyle razy, stosownie do potrzeb wdrażanie wszystkich zagadnienia w tym dokumencie. 

| Faza tożsamości hybrydowego                                             | Lista tematów                                                                                                                                                                                       |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Określanie wymagania tożsamości                                   | [Określanie potrzeb biznesowych](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Określenie wymagań dotyczących synchronizacji katalogów](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Określanie wymagania dotyczące uwierzytelniania wieloskładnikowego](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Definiowanie strategii wdrażania tożsamości hybrydowego](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)                       |
| Planowanie dotyczące zwiększania zabezpieczeń danych za pośrednictwem rozwiązanie silnych tożsamości | [Określanie wymagania dotyczące ochrony danych](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Określenie wymagań dotyczących zarządzania zawartością](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Określanie wymagania kontroli dostępu](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Określanie wymagania odpowiedzi wystąpieniu zdarzenia](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Definiowanie strategię ochrony danych](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)  |
| Plan cyklu życia tożsamości hybrydowego                                | [Określanie hybrydowych zadania związane z zarządzaniem tożsamości](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Zarządzanie synchronizacją](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Określanie strategii wdrażania zarządzania tożsamości hybrydowego](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |     


##<a name="download-this-guide"></a>Pobierz ten przewodnik
Możesz pobrać wersję pdf przewodnika zagadnienia projektowe tożsamości hybrydowego z [galerii Technet](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

                                                             
