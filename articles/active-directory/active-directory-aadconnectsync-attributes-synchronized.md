<properties
    pageTitle="Azure AD Connect synchronizacją: atrybuty synchronizacji usługi Azure Active Directory | Microsoft Azure"
    description="Wyświetla listę atrybutów, które są synchronizowane z usługi Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Connect synchronizacją: atrybuty synchronizacji usługi Azure Active Directory
W tym temacie wymieniono atrybuty, które są synchronizowane przez narzędzie Azure AD Connect synchronizacji.  
Atrybuty są pogrupowane według powiązanych Azure AD aplikacji.

## <a name="attributes-to-synchronize"></a>Atrybuty do synchronizacji
Często zadawane pytania to, *Co to jest lista minimalne atrybutów do synchronizacji*. Domyślne i zalecane podejście jest, aby utrzymać domyślnych atrybutów, aby pełny GAL (globalna lista adresowa) może być skonstruowane w chmurze i wszystkie funkcje są pobierane za obciążeń pracą usługi Office 365. W niektórych przypadkach istnieją pewne atrybuty, które organizacji nie mają być synchronizowane z chmury ponieważ zawierają następujące atrybuty poufne lub danych (informacji umożliwiających identyfikację) osoby, na przykład w tym przykładzie:  
![nieprawidłowe atrybuty](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

W tym przypadku zaczynają się na liście atrybutów w tym temacie i identyfikowanie te atrybuty, które będzie zawierać liter lub dane osobowe i nie można zsynchronizować. Podczas instalacji przy użyciu [aplikacji Azure AD i filtrowanie atrybut](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering), usuń zaznaczenie opcji atrybutach.

>[AZURE.WARNING] Po usunięcie zaznaczenia atrybuty, należy zachować ostrożność i tylko Usuń zaznaczenie atrybutach naprawdę nie można zsynchronizować. Usuwając inne atrybuty może mieć negatywny wpływ na funkcje.

## <a name="office-365-proplus"></a>Usługa Office 365 ProPlus

| Nazwa atrybutu| Użytkownika| Komentarz |
| --- | :-: | --- |
| accountEnabled| X| Określa, czy konto jest włączone.|
| CN| X|  |
| displayName| X|  |
| objectSID| X| właściwości. AD identyfikator użytkownika używany do obsługi synchronizacji między Azure AD i AD.|
| pwdLastSet| X| właściwości. Umożliwia wiedzieć, kiedy powodować tokeny już wystawiony. Używana przez synchronizacji haseł i federacji.|
| sourceAnchor| X| właściwości. Obsługa relacji między DODAJE i Azure AD niezmienne identyfikator.|
| usageLocation| X| właściwości. Kraj użytkownika. Używany do przypisywania licencji.|
| userPrincipalName| X| UPN jest identyfikator logowania dla użytkownika. Najczęściej używane wartość tak samo jak [mail].|

## <a name="exchange-online"></a>Usługa Exchange Online

| Nazwa atrybutu| Użytkownika| Kontakt| Grupa| Komentarz |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Określa, czy konto jest włączone.|
| Asystent| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| co| X| X|  |  |
| Firma| X| X|  |  |
| countryCode| X| X|  |  |
| Dział| X| X|  |  |
| Opis| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| Imię| X| X|  |  |
| TelefonDomowy| X| X|  |  |
| informacje o| X| X| X| Ten atrybut nie jest obecnie używane dla grup.|
| Inicjały| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailNickname| X| X| X|  |
| mangedBy|  |  | X|  |
| Menedżer| X| X|  |  |
| element członkowski|  |  | X|  |
| urządzeń przenośnych| X| X|  |  |
| msDS HABSeniorityIndex| X| X| X|  |
| msDS PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| msExchAssistantName| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| Ten atrybut nie jest obecnie używane przez usługę Exchange Online. |
| msExchExtensionCustomAttribute2| X| X| X| Ten atrybut nie jest obecnie używane przez usługę Exchange Online. |
| msExchExtensionCustomAttribute3| X| X| X| Ten atrybut nie jest obecnie używane przez usługę Exchange Online. |
| msExchExtensionCustomAttribute4| X| X| X| Ten atrybut nie jest obecnie używane przez usługę Exchange Online. |
| msExchExtensionCustomAttribute5| X| X| X| Ten atrybut nie jest obecnie używane przez usługę Exchange Online. |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| msExchRecipientTypeDetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| msOrg IsOrganizational|  |  | X|  |
| objectSID| X|  | X| właściwości. AD identyfikator użytkownika używany do obsługi synchronizacji między Azure AD i AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| pager| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| kod_pocztowy| X| X|  |  |
| proxyAddresses| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | właściwości. Umożliwia wiedzieć, kiedy powodować tokeny już wystawiony. Używana przez synchronizacji haseł i federacji.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Pochodny od groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| właściwości. Obsługa relacji między DODAJE i Azure AD niezmienne identyfikator.|
| St| X| X|  |  |
| Adres| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Tytuł| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | właściwości. Kraj użytkownika. Używany do przypisywania licencji.|
| certyfikatu użytkownika| X| X|  |  |
| userPrincipalName| X|  |  | UPN jest identyfikator logowania dla użytkownika. Najczęściej używane wartość tak samo jak [mail].|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>Usługa SharePoint Online

| Nazwa atrybutu| Użytkownika| Kontakt| Grupa| Komentarz |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Określa, czy konto jest włączone.|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| co| X| X|  |  |
| Firma| X| X|  |  |
| countryCode| X| X|  |  |
| Dział| X| X|  |  |
| Opis| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| Imię| X| X|  |  |
| hideDLMembership|  |  | X|  |
| TelefonDomowy| X| X|  |  |
| informacje o| X| X| X|  |
| Inicjały| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| Poczta| X| X| X|  |
| mailnickname| X| X| X|  |
| managedBy|  |  | X|  |
| Menedżer| X| X|  |  |
| element członkowski|  |  | X|  |
| middleName| X| X|  |  |
| urządzeń przenośnych| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| właściwości. AD identyfikator użytkownika używany do obsługi synchronizacji między Azure AD i AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| pager| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| kod_pocztowy| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | właściwości. Umożliwia wiedzieć, kiedy powodować tokeny już wystawiony. Używana przez synchronizacji haseł i federacji.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Pochodny od groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| właściwości. Obsługa relacji między DODAJE i Azure AD niezmienne identyfikator.|
| St| X| X|  |  |
| Adres| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Tytuł| X| X|  |  |
| unauthOrig| X| X| X|  |
| adres URL| X| X|  |  |
| usageLocation| X|  |  | właściwości. Kraj użytkownika. Używany do przypisywania licencji.|
| userPrincipalName| X|  |  | UPN jest identyfikator logowania dla użytkownika. Najczęściej używane wartość tak samo jak [mail].|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Usługa Lync Online

| Nazwa atrybutu| Użytkownika| Kontakt| Grupa| Komentarz |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Określa, czy konto jest włączone.|
| c| X| X|  |  |
| CN| X|  | X|  |
| co| X| X|  |  |
| Firma| X| X|  |  |
| Dział| X| X|  |  |
| Opis| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| Imię| X| X|  |  |
| TelefonDomowy| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| Poczta| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| Menedżer| X| X|  |  |
| element członkowski|  |  | X|  |
| urządzeń przenośnych| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| msRTCSIP ApplicationOptions| X|  |  |  |
| msRTCSIP DeploymentLocator| X| X|  |  |
| Linia msRTCSIP| X| X|  |  |
| msRTCSIP OptionFlags| X| X|  |  |
| msRTCSIP OwnerUrn| X|  |  |  |
| msRTCSIP PrimaryUserAddress| X| X|  |  |
| msRTCSIP-UserEnabled| X| X|  |  |
| objectSID| X|  | X| właściwości. AD identyfikator użytkownika używany do obsługi synchronizacji między Azure AD i AD.|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| kod_pocztowy| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | właściwości. Umożliwia wiedzieć, kiedy powodować tokeny już wystawiony. Używana przez synchronizacji haseł i federacji.|
| securityEnabled|  |  | X| Pochodny od groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| właściwości. Obsługa relacji między DODAJE i Azure AD niezmienne identyfikator.|
| St| X| X|  |  |
| Adres| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| Tytuł| X| X|  |  |
| usageLocation| X|  |  | właściwości. Kraj użytkownika. Używany do przypisywania licencji.|
| userPrincipalName| X|  |  | UPN jest identyfikator logowania dla użytkownika. Najczęściej używane wartość tak samo jak [mail].|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure RMS

| Nazwa atrybutu| Użytkownika| Kontakt| Grupa| Komentarz |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Określa, czy konto jest włączone.|
| CN| X|  | X| Typowe nazwę lub alias. Najczęściej używane prefiks wartość [mail].|
| displayName| X| X| X| Ciąg, który odpowiada nazwie często są wyświetlane jako przyjazną nazwę (imię nazwisko).|
| Poczta| X| X| X| pełny adres e-mail.|
| element członkowski|  |  | X|  |
| objectSID| X|  | X| właściwości. AD identyfikator użytkownika używany do obsługi synchronizacji między Azure AD i AD.|
| proxyAddresses| X| X| X| właściwości. Używane przez Azure AD. Zawiera wszystkie dodatkowych adresów e-mail dla użytkownika.|
| pwdLastSet| X|  |  | właściwości. Umożliwia wiedzieć, kiedy powodować tokeny już wystawiony.|
| securityEnabled|  |  | X| Pochodny od groupType.|
| sourceAnchor| X| X| X| właściwości. Obsługa relacji między DODAJE i Azure AD niezmienne identyfikator.|
| usageLocation| X|  |  | właściwości. Kraj użytkownika. Używany do przypisywania licencji.|
| userPrincipalName| X|  |  | Ta nazwa UPN jest identyfikator logowania dla użytkownika. Najczęściej używane wartość tak samo jak [mail].|

## <a name="intune"></a>Intune

| Nazwa atrybutu| Użytkownika| Kontakt| Grupa| Komentarz |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Określa, czy konto jest włączone.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Opis| X| X| X|  |
| displayName| X| X| X|  |
| Poczta| X| X| X|  |
| mailnickname| X| X| X|  |
| element członkowski|  |  | X|  |
| objectSID| X|  | X| właściwości. AD identyfikator użytkownika używany do obsługi synchronizacji między Azure AD i AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | właściwości. Umożliwia wiedzieć, kiedy powodować tokeny już wystawiony. Używana przez synchronizacji haseł i federacji.|
| securityEnabled|  |  | X| Pochodny od groupType|
| sourceAnchor| X| X| X| właściwości. Obsługa relacji między DODAJE i Azure AD niezmienne identyfikator.|
| usageLocation| X|  |  | właściwości. Kraj użytkownika. Używany do przypisywania licencji.|
| userPrincipalName| X|  |  | UPN jest identyfikator logowania dla użytkownika. Najczęściej używane wartość tak samo jak [mail].|

## <a name="dynamics-crm"></a>Dynamics CRM

| Nazwa atrybutu| Użytkownika| Kontakt| Grupa| Komentarz |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Określa, czy konto jest włączone.|
| c| X| X|  |  |
| CN| X|  | X|  |
| co| X| X|  |  |
| Firma| X| X|  |  |
| countryCode| X| X|  |  |
| Opis| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| Imię| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| Menedżer| X| X|  |  |
| element członkowski|  |  | X|  |
| urządzeń przenośnych| X| X|  |  |
| objectSID| X|  | X| właściwości. AD identyfikator użytkownika używany do obsługi synchronizacji między Azure AD i AD.|
| physicalDeliveryOfficeName| X| X|  |  |
| kod_pocztowy| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | właściwości. Umożliwia wiedzieć, kiedy powodować tokeny już wystawiony. Używana przez synchronizacji haseł i federacji.|
| securityEnabled|  |  | X| Pochodny od groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| właściwości. Obsługa relacji między DODAJE i Azure AD niezmienne identyfikator.|
| St| X| X|  |  |
| Adres| X| X|  |  |
| telephoneNumber| X| X|  |  |
| Tytuł| X| X|  |  |
| usageLocation| X|  |  | właściwości. Kraj użytkownika. Używany do przypisywania licencji.|
| userPrincipalName| X|  |  | UPN jest identyfikator logowania dla użytkownika. Najczęściej używane wartość tak samo jak [mail].|

## <a name="3rd-party-applications"></a>3 aplikacji innych firm
Ta grupa jest zestaw atrybutów używane jako atrybuty minimalne wymagane ogólne obciążenie pracą lub aplikacji. Można uzyskać obciążenie pracą niewymienionych w innej sekcji lub aplikacji innych firm. Wyraźnie służy do wykonania poniższych czynności:

- Usługi Yammer (zużyte tylko użytkownika)
- [Hybrydowe między firmami (B2B) całej organizacji współpracy scenariusze oferowanych przez wszystkie zasoby, takie jak programu SharePoint](http://go.microsoft.com/fwlink/?LinkId=747036)

Ta grupa jest zestaw atrybutów, które mogą być używane, jeśli katalog Azure AD nie jest używana do obsługi usługi Office 365, Dynamics lub Intune. Ma niewielki zestaw atrybutów core.

| Nazwa atrybutu| Użytkownika| Kontakt| Grupa| Komentarz |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Określa, czy konto jest włączone.|
| CN| X|  | X|  |
| displayName| X| X| X|  |
| Imię| X| X|  |  |
| Poczta| X|  | X|  |
| managedBy|  |  | X|  |
| mailNickName| X| X| X|  |
| element członkowski|  |  | X|  |
| objectSID| X|  |  | właściwości. AD identyfikator użytkownika używany do obsługi synchronizacji między Azure AD i AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | właściwości. Umożliwia wiedzieć, kiedy powodować tokeny już wystawiony. Używana przez synchronizacji haseł i federacji.|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| właściwości. Obsługa relacji między DODAJE i Azure AD niezmienne identyfikator.|
| usageLocation| X|  |  | właściwości. Kraj użytkownika. Używany do przypisywania licencji.|
| userPrincipalName| X|  |  | UPN jest identyfikator logowania dla użytkownika. Najczęściej używane wartość tak samo jak [mail].|

## <a name="windows-10"></a>Windows 10
Computer(device) domeny systemu Windows 10 synchronizuje niektóre atrybuty Azure AD. Aby uzyskać więcej informacji dotyczących scenariuszy zobacz [napotkania urządzenia domeny Połącz, aby Azure AD dla systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md). Następujące atrybuty synchronizować je zawsze i systemu Windows 10 nie jest wyświetlana jako aplikację, którą możesz usunąć zaznaczenie. Komputer domeny systemu Windows 10 jest rozpoznawany przez certyfikatu użytkownika atrybut wypełnione.

| Nazwa atrybutu| Urządzenie| Komentarz |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| Wartość ustalony dla komputerów domeny. |
| displayName | X| |
| MS-DS-CreatorSID | X| Skrót registeredOwnerReference.|
| objectGUID | X| Skrót identyfikator urządzenia.|
| objectSID | X| Skrót onPremisesSecurityIdentifier.|
| system operacyjny | X| Skrót deviceOSType.|
| operatingSystemVersion | X| Skrót deviceOSVersion.|
| certyfikatu użytkownika | X| |

Te atrybuty dla **użytkownika** są oprócz innych aplikacji, który wybrano.  

| Nazwa atrybutu| Użytkownika| Komentarz |
| --- | :-: | --- |
| domainFQDN| X| Skrót NazwaDomenyDNS. Na przykład contoso.com.|
| domainNetBios| X| Skrót Nazwa_netbios. Na przykład CONTOSO.|

## <a name="exchange-hybrid-writeback"></a>Zapisu hybrydowe programu Exchange
Te atrybuty są zapisywane z usługi Azure Active Directory w lokalnej usługi Active Directory po wybraniu umożliwiające **wdrożenia hybrydowe programu Exchange**. W zależności od wersji programu Exchange może być synchronizowane mniej atrybutów.

| Nazwa atrybutu| Użytkownika| Kontakt| Grupa| Komentarz |
| --- | :-: | :-: | :-: | --- |
| msDS ExternalDirectoryObjectID| X|  |  | Pochodny od cloudAnchor w Azure AD. Ten atrybut jest nowego w usłudze Exchange 2016.|
| msExchArchiveStatus| X|  |  | Archiwum online: Umożliwia klientom archiwizuj poczty.|
| msExchBlockedSendersHash| X|  |  | Filtrowanie: Zapisuje ponownie lokalnego filtrowania i danych online nadawcy bezpiecznych i zablokowanych z klientów.|
| msExchSafeRecipientsHash| X|  |  | Filtrowanie: Zapisuje ponownie lokalnego filtrowania i danych online nadawcy bezpiecznych i zablokowanych z klientów.|
| msExchSafeSendersHash| X|  |  | Filtrowanie: Zapisuje ponownie lokalnego filtrowania i danych online nadawcy bezpiecznych i zablokowanych z klientów.|
| msExchUCVoiceMailSettings| X|  |  | Włączanie Unified Messaging (UM) — Poczta głosowa Online: używane przez program Microsoft Lync Server integration zasygnalizować programu Lync Server lokalnego czy użytkownik ma poczty głosowej w usługach online.|
| msExchUserHoldPolicies| X|  |  | Archiwum sporu prawnego: Umożliwia usług w chmurze, aby określić, którzy użytkownicy podlegają przytrzymaj sporu prawnego.|
| proxyAddresses| X| X| X| Tylko x500 adres z usługi Exchange Online została wstawiona.|

## <a name="device-writeback"></a>Urządzenie wystornowaniem
Obiekty urządzenia są tworzone w usłudze Active Directory. Tych obiektów może być dołączony do Azure AD urządzeń lub domeny komputerów systemu Windows 10.

| Nazwa atrybutu| Urządzenie| Komentarz |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| displayName | X| |
| nazwy domeny | X| |
| msDS CloudAnchor | X| |
| Identyfikator urządzenia msDS | X| |
| msDS DeviceObjectVersion | X| |
| msDS DeviceOSType | X| |
| msDS DeviceOSVersion | X| |
| msDS DevicePhysicalIDs | X| |
| msDS KeyCredentialLink | X| Tylko w przypadku systemu Windows Server 2016 AD schematu |
| msDS IsCompliant | X| |
| msDS IsEnabled | X| |
| msDS IsManaged | X| |
| msDS RegisteredOwner | X| |


## <a name="notes"></a>Notatki

- Podczas korzystania z alternatywny identyfikator lokalnego atrybut userPrincipalName są synchronizowane z onPremisesUserPrincipalName atrybut Azure AD. Atrybut alternatywny identyfikator, na przykład poczty, jest synchronizowana z userPrincipalName atrybut Azure AD.
- Na liście powyżej typ obiektu **użytkownika** dotyczy również programu typ obiektu **iNetOrgPerson**.

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak [zsynchronizować Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfiguracji.

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
