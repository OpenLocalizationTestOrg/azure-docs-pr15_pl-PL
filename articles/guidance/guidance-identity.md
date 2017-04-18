<properties
   pageTitle="Zarządzanie tożsamościami platformy Azure | Microsoft Azure"
   description="W tym miejscu wyjaśniono i porównanie różnych metod zarządzania tożsamości w systemach hybrydowe, obejmujące obramowanie na lokalnej chmury z Azure."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="telmosampaio"/>
   
# <a name="managing-identity-in-azure"></a>Zarządzanie tożsamościami platformy Azure

W większości systemów przedsiębiorstwa w systemie Windows będą dostępne usługi zarządzania tożsamości aplikacji za pomocą Active Directory (AD). AD sprawdza się dobrze w środowisku lokalnym, ale podczas rozszerzanie infrastruktury sieciowej w chmurze jest dostępnych kilka ważnych decyzji nawiązać dotyczące sposobu zarządzania tożsamości. Należy rozszerzyć domeny lokalnej przydadzą się maszyny wirtualne w chmurze? Czy utworzyć nowy domenami w chmurze, a jeśli tak, jak? Należy zaimplementować własne las w chmurze lub powinien możesz wykorzystać z Azure Active Directory (AAD)?

W tym artykule opisano niektóre często używane opcje dla spotkania problemów powodowanych w tym scenariuszu i pomaga określić, najlepiej rozwiązania, które nie spełnia określonych wymagań, zgodnie z własnymi potrzebami.

## <a name="using-azure-active-directory"></a>Za pomocą usługi Azure Active Directory

AAD służy do tworzenia domeny AD w chmurze i połączyć go z lokalnego hasło. AAD umożliwia konfigurowanie logowania jednokrotnego (SSO) dla użytkowników korzystających z aplikacji dostępnym w chmurze.

[! [0]][0]

AAD jest proste sposób implementacji domeny zabezpieczeń w chmurze. Jest ona wykorzystać wiele aplikacji firmy Microsoft, takich jak usługi Microsoft Office 365. 

Korzyści wynikające z używania AAD:

- Istnieje potrzeba obsługa infrastruktury usługi Active Directory w chmurze. AAD całkowicie oraz zarządzania obsługiwany przez firmę Microsoft.

- AAD zawiera te same informacje tożsamości, który jest dostępny w lokalnej.

- Uwierzytelnianie może wystąpić w Azure, zmniejszając w aplikacjach zewnętrznych i użytkownicy mogą się kontaktować z domeną lokalnego.

Kwestie do rozważenia podczas korzystania z AAD:

- Tożsamość usługi są ograniczone do użytkowników i grup. Istnieje możliwość uwierzytelniania kont usługi i komputera.

- Należy skonfigurować połączenie z domeny lokalnej, aby katalog AAD synchronizowane. 

- Jesteś odpowiedzialny za publikowanie aplikacji, które użytkownicy mogą uzyskiwać dostęp w chmurze za pośrednictwem AAD.

Aby uzyskać szczegółowe informacje, przeczytaj [Wdrażania usługi Azure Active Directory][implementing-aad].

## <a name="using-active-directory-in-the-cloud-joined-to-an-on-premises-forest"></a>Za pomocą usługi Active Directory w chmurze dołączony do lokalnego las

Można udostępniać lokalne usług AD Directory (AD DS), ale w scenariuszu hybrydowe, gdzie znajdują się elementy aplikacji w Azure może być bardziej efektywne, aby odtworzyć tę funkcję i repozytorium AD w chmurze. Tej metody można ograniczyć opóźnienia spowodowane przez wysłanie uwierzytelniania i żądań autoryzacji lokalne z chmury do uruchamiania lokalnego w usługach AD DS. 

[! [1]][1]

Ta metoda wymaga Tworzenie własnej domeny w chmurze, a następnie dołączyć go do las lokalnego. Możesz utworzyć maszyny wirtualne do obsługi usług AD DS.

Korzyści wynikające z używania z innej domeny w chmurze:

- Udostępnia możliwość uwierzytelniania użytkowników, usługi i komputerów konta lokalnego i w chmurze.

- Zapewnia dostęp do tej samej informacji tożsamości, który jest dostępny w lokalnej.

- Istnieje potrzeba Zarządzanie osobnych las AD; domeny w chmurze mogą należeć do las lokalnego.

- Możesz zastosować zasady grupy zdefiniowane przez lokalnego obiekty zasad grupy do domeny w chmurze.

Zagadnienia dotyczące korzystania z innej domeny w chmurze:

- Należy utworzyć i zarządzać nimi, własny serwer usług AD DS domeny w chmurze.

- Czasami może występować pewne opóźnienie synchronizacji między serwerami domeny w chmurze i serwerach lokalnego.

Aby uzyskać informacje na temat konfigurowania tej architektury, zobacz [Rozszerzanie Active Directory katalogów usług (DODAJE) Azure][extending-adds].

## <a name="using-active-directory-with-a-separate-forest"></a>Za pomocą usługi Active Directory przy użyciu oddzielnych las

Organizacja, która jest uruchamiany Active Directory (AD) lokalnego może być las obejmujące wiele różnych domenach. Można używać domeny o podanie izolacji między funkcjonalności obszarów, które muszą być przechowywane oddzielnie, prawdopodobnie ze względów bezpieczeństwa, ale możesz udostępnić informacje między domenami przez ustanowienie relacji zaufania.

[! [2]][2]

Organizacji, która korzysta z różnych domenach można korzystać z Azure przy przenoszeniu co najmniej jedną z tych domen w osobnym las w chmurze. Możesz też organizacji może chcesz zachować wszystkie zasoby chmury logicznie różnią się od tych przechowywanych w lokalnej i przechowywanie informacji o zasobach w chmurze w własnego katalogu jako część las również przechowywanych w chmurze.

Korzyści wynikające z używania oddzielnych las w chmurze:

- Można zaimplementować tożsamości lokalnych i oddzielić tylko do Azure tożsamości.

- Istnieje potrzeba replikacji ze źródeł lokalnych las AD Azure, zmniejszając skutki opóźnień sieci.

Zagadnienia:

- Uwierzytelnianie dla tożsamości lokalnych w chmurze wykonuje dodatkowe sieci *przeskoków* do lokalnego Serwery AD.

- Wymaga wykonania własny serwer usług AD DS i las w chmurze i ustanawiać relacje zaufania odpowiednie między lasami.

Dokument [tworzenia las zasobu usługi Active Directory katalogu (DODAJE) platformy Azure] [ adds-forest-in-azure] opisano, jak zaimplementować tej metody bardziej szczegółowo.

## <a name="using-active-directory-federation-services-adfs-with-azure"></a>Za pomocą usługi Active Directory Federation Services (ADFS) z platformy Azure

ADFS może zostać uruchomiony lokalnego, ale w scenariuszu hybrydowych lokalizację aplikacji platformy Azure może być bardziej efektywne zaimplementować tę funkcję w chmurze, tak jak pokazano poniżej.

[! [3]][3]

Ta architektura jest szczególnie przydatne dla:

- Rozwiązania, które są używane federacyjnych autoryzacji udostępniania aplikacji sieci web organizacji partnera.

- Systemy, które obsługują dostęp za pomocą przeglądarki sieci web z spoza organizacji zapory.

- Systemy, które umożliwiają dostęp do aplikacji sieci web za pomocą połączenia z autoryzowanych urządzeń zewnętrznych, takich jak komputerów zdalnych, notesów i innych urządzeń przenośnych. 

Korzyści wynikające z używania usług ADFS organizacji z Azure:

- Mogą korzystać z aplikacji obsługujących roszczeń.

- Udostępnia możliwość zaufania partnerów zewnętrznych do uwierzytelniania.

- Zapewnia zgodność z dużego zestawu protokołów uwierzytelniania.

Zagadnienia dotyczące korzystania z usług ADFS organizacji z Azure:

- Wymaga wykonania własne serwery DODAJE, ADFS i serwer Proxy aplikacji sieci Web usług ADFS w chmurze.

- Ta architektura może być łatwiejsze.

Aby uzyskać szczegółowe informacje, przeczytaj [Implementacji Active Directory Federation Services (ADFS) platformy Azure][adfs-in-azure].

## <a name="next-steps"></a>Następne kroki

Zasoby poniżej wyjaśniono, jak wdrażać architektury opisane w tym artykule.

- [Implementacji usługi Azure Active Directory][implementing-aad]
- [Rozszerzanie usługi Active Directory (DODAJE) Azure][extending-adds]
- [Tworzenie las zasobu usługi Active Directory katalogu (DODAJE) platformy Azure][adds-forest-in-azure]
- [Wdrażanie usług federacyjnych Active Directory (ADFS) w Azure][adfs-in-azure]

<!-- Links -->
[0]: ./media/guidance-identity/figure1.png "Architektura tożsamości chmurze przy użyciu usługi Azure Active Directory"
[1]: ./media/guidance-identity/figure2.png "Zabezpieczanie hybrydowych architektury sieci z usługą Active Directory"
[2]: ./media/guidance-identity/figure3.png "Zabezpieczanie hybrydowych architektury sieci przy użyciu oddzielnych AD domen i lasów"
[3]: ./media/guidance-identity/figure4.png "Zabezpieczanie architektury sieci hybrydowego z usługami ADFS"
[implementing-aad]: ./guidance-identity-aad.md
[extending-adds]: ./guidance-identity-adds-extend-domain.md
[adds-forest-in-azure]: ./guidance-identity-adds-resource-forest.md
[adfs-in-azure]: ./guidance-identity-adfs.md