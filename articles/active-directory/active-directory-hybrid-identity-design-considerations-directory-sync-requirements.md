<properties
    pageTitle="Azure Active Directory hybrydowych tożsamości zagadnienia projektowe - określenie wymagań synchronizacji katalogów | Microsoft Azure"
    description="Identyfikowanie, jakie wymagania są wymagane przez wszystkich użytkowników między synchronizowanie on = pomieszczeń i chmury dla przedsiębiorstwa."
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

# <a name="determine-directory-synchronization-requirements"></a>Określenie wymagań dotyczących synchronizacji katalogów
Synchronizacja jest udostępnianie tożsamości w chmurze oparte na jego tożsamości lokalnych użytkowników. Czy używają zsynchronizowaną konta dla uwierzytelniania lub federacyjnych, użytkownicy nadal muszą mieć tożsamości w chmurze.  Ta tożsamość będzie konieczne należy zachować i okresowo aktualizowane.  Aktualizacje może mieć wiele form, przed zmianami tytułu do zmiany hasła.  

Zacznij od obliczonego organizacji lokalnej tożsamości rozwiązanie i użytkownika wymagań. Ocena ważne jest, aby zdefiniować wymagania techniczne dotyczące sposobu tworzenia i przechowywane w chmurze tożsamości użytkownika.  W przypadku większości organizacji usługi Active Directory jest lokalnego i będą katalogu lokalnego, których użytkownicy będą przez synchronizowane z, jednak w niektórych przypadkach nie będzie to wielkość liter.  

Upewnij się, że na następujące pytania:


- Czy masz jeden las AD, wielokrotności lub Brak?
 - Ile katalogi Azure AD będzie można się synchronizacji?
 
    1. Używasz filtrowania?
    2. Czy masz wiele serwerów Azure AD Connect planowana?
  
- Obecnie masz synchronizacji narzędzia lokalnego?
  - Jeśli tak, czy użytkownicy, jeśli użytkownicy mają wirtualnego katalogu/integracji tożsamości?
- Czy masz wszelkie inne katalogiem lokalnie, które chcesz zsynchronizować (np. katalogu LDAP, HR bazy danych, itp)?
  - Czy mają być wykonując dowolną GALSync?
  - Co to jest bieżący stan UPN w organizacji? 
  - Czy masz innego katalogu, w którym uwierzytelniania użytkowników?
  - Firma używa programu Microsoft Exchange?
    - Czy jest planowana o wdrożenia hybrydowego programu exchange?

Teraz, gdy masz uzyskać ogólny obraz o wymaganiach synchronizacji, należy określić, które narzędzie jest poprawny tych wymagań.  Firma Microsoft udostępnia kilka narzędzi do realizacji integracji katalogów i synchronizacji.  Zobacz [tabelę narzędzia integracji katalogu tożsamości hybrydowych](active-directory-hybrid-identity-design-considerations-tools-comparison.md) Aby uzyskać więcej informacji. 
   
Teraz, gdy masz z wymaganiami synchronizacji i narzędzia, które będzie to zrobić dla swojej firmy, musisz być aplikacje, które używają tych usług katalogowych. Ocena ważne jest, aby zdefiniować wymagania techniczne, aby zintegrować te aplikacje w chmurze. Upewnij się, że na następujące pytania:

- Zostaną przeniesione te aplikacje w chmurze i używanie katalogu?
- Czy istnieją specjalne atrybuty, które muszą zostać zsynchronizowane w chmurze, aby te aplikacje można używać ich pomyślnie?
- Te aplikacje musisz ponownie w celu zapisania skorzystać z chmury auth?
- Te aplikacje nadal będą live lokalnego, gdy użytkownicy uzyskiwać do nich dostęp przy użyciu tożsamości chmurze?

Trzeba także określić synchronizacji katalogów wymagania i ograniczenia zabezpieczeń. Ocena ważne jest, aby uzyskać listę wymagań, które będą potrzebne w celu tworzenia i obsługi tożsamości użytkownika w chmurze. Upewnij się, że na następujące pytania:

- Miejsce, w którym będą umieszczane na serwerze synchronizacji?
- Będzie sprzężone domeny?
- Serwer będzie znajdować się w sieci z ograniczeniami za zaporą, takich jak strefy Zdemilitaryzowanej?
  - Będzie można otworzyć portów zapory wymagane do obsługi synchronizacji?
- Czy masz planu odzyskiwania danych na serwerze synchronizacji?
- Czy masz konto z odpowiednimi uprawnieniami dla wszystkich lasów, którą chcesz zsynchronizować z?
 - Jeśli firma nie wie, odpowiedzi na to pytanie, zapoznaj się z sekcją "Uprawnień do synchronizacji haseł" w artykule [Instalowanie usługi Azure do synchronizacji Active Directory](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) i stwierdzić, czy masz już konto z tych uprawnień, albo jeśli chcesz utworzyć.
- Jeśli masz las mutli synchronizacji to serwer synchronizacji można uzyskać dostęp do każdego las?
 
>[AZURE.NOTE]
Pamiętaj sporządzać notatki każdej odpowiedzi i opis uzasadnienie odpowiedź. [Określanie wymagania zdarzenia odpowiedzi](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) będą wyświetlane przez dostępne opcje. Przez o odpowiedzi na te pytania, która zostanie wybrana wybranej opcji najlepiej pasuje Twoja firma wymaga.

## <a name="next-steps"></a>Następne kroki
[Określanie wymagania dotyczące uwierzytelniania wieloskładnikowego](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Zobacz też
[Omówienie zagadnienia projektu](active-directory-hybrid-identity-design-considerations-overview.md)
