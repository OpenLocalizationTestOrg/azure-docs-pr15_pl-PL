<properties
    pageTitle="Zarządzanie dostępem do zasobów z grupami usługi Azure Active Directory | Microsoft Azure"
    description="Jak korzystać z grup usługi Azure Active Directory, aby zarządzać dostępem użytkowników do lokalnego i aplikacje w chmurze i zasoby."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="managing-access-to-resources-with-azure-active-directory-groups"></a>Zarządzanie dostępem do zasobów z grupami usługi Azure Active Directory

Azure Active Directory (Azure AD) jest pełna tożsamości i dostępu do zarządzania rozwiązaniem, które udostępnia zestaw funkcji do zarządzania dostęp do lokalnego i aplikacje w chmurze i zasobów, w tym usługi online firmy Microsoft, takich jak usługi Office 365 i świata SaaS innych niż Microsoft aplikacji. W tym artykule omówiono, ale jeśli chcesz rozpocząć korzystanie z grup Azure AD teraz, postępuj zgodnie z instrukcjami [Zarządzanie grupami zabezpieczeń w Azure AD](active-directory-accessmanagement-manage-groups.md). Jeśli chcesz zobaczyć, jak można używać programu PowerShell Zarządzanie grupami usługi Azure Active directory można znaleźć w więcej [poleceń cmdlet Podgląd usługi Azure Active Directory do zarządzania grupy](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).


> [AZURE.NOTE] Aby użyć usługi Azure Active Directory, potrzebne jest konto Azure. Jeśli nie masz konta, możesz [utworzyć bezpłatne konto Azure](https://azure.microsoft.com/pricing/free-trial/).


W Azure AD jedną z najważniejszych funkcji jest możliwość zarządzania dostępem do zasobów. Te zasoby może być częścią katalogu, tak jak w przypadku uprawnień do zarządzania obiektami za pośrednictwem ról w katalogu lub zasoby, które są zewnętrzne do katalogu, takich jak aplikacje władz akredytacji bezpieczeństwa, usług Azure i witryn programu SharePoint lub w wersji lokalnej zasoby. Istnieją cztery sposoby, które można przypisać użytkownikowi praw dostępu do zasobu:


1. Bezpośrednie przypisanie

    Użytkowników można przypisywać bezpośrednio do zasobu przez właściciela tego zasobu.

2. Członkostwo w grupach

    Grupy można przypisywać do zasobu przez właściciela zasobów i w ten sposób, udzielanie członków tej grupy dostępu do tego zasobu. Członkostwo w grupie zarządza następnie właściciela grupy. Efektywne właściciel zasobu deleguje uprawnień można przypisać użytkowników do ich zasobów do właściciela grupy.

3. Na podstawie reguł

    Właściciel zasobów umożliwia reguły express użytkowników, którzy mają być przydzielane dostępu do zasobu. Wynik reguły zależy od atrybuty używane w tej reguły i ich wartości dla określonych użytkowników, a w ten sposób właściciela zasobu efektywnie deleguje w prawo, aby zarządzać dostępem do ich zasobów w źródle autorytatywne atrybutów, które są używane w regule. Właściciel zasobu nadal zarządza samej reguły i określa atrybutów i wartości, które zapewniają dostęp do swoich zasobów.

4. Zewnętrznego urzędu

    Dostęp do zasobu pochodzi z zewnętrznego źródła; na przykład grupa, która jest synchronizowana z wiarygodnego źródła, takie jak katalogu lokalnego lub aplikacji władz akredytacji bezpieczeństwa, takie jak pracy. Właściciel zasobu przypisuje grupy, aby umożliwić dostęp do tego zasobu, a źródło zewnętrzne zarządza członków grupy.

  ![Omówienie diagram zarządzania programu access](./media/active-directory-access-management-groups/access-management-overview.png)


## <a name="watch-a-video-that-explains-access-management"></a>Obejrzyj klip wideo, w którym wyjaśniono, zarządzanie dostępem

Możesz obejrzeć krótki klip wideo, w którym przedstawiono więcej informacji na ten temat:

**Azure AD: Wprowadzenie do dynamicznego członkostwa dla grup**

> [AZURE.VIDEO azure-ad--introduction-to-dynamic-memberships-for-groups]

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Jak uzyskać dostęp do zarządzania w pracy usługi Azure Active Directory?
W środkowej części Azure AD rozwiązanie do zarządzania programu access jest grupy zabezpieczeń. Zarządzanie dostępem do zasobów za pomocą grupy zabezpieczeń jest znane modelu, który pozwala na sposób elastyczne i łatwo zrozumiałe umożliwić dostęp do zasobu w grupie użytkowników. Właściciel zasobów (lub administrator katalogu) można przypisać grupę, do niektórych uzyskać dostęp bezpośrednio do zasobów, które są właścicielami. Członkowie grupy zapewnia dostęp, a właściciel zasobu można delegować w prawo, aby zarządzać listą członków grupy do innej osoby, takie jak kierownik działu lub administratorem działu pomocy technicznej.

![Diagram zarządzania dostępu w usłudze Azure Active Directory](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

Właścicielem grupy można także udostępnić tę grupę dla wniosków dotyczących sklepu internetowego. W ten sposób użytkownik końcowy można wyszukiwać i znaleźć grupę i wprowadzić żądanie, aby dołączyć, skuteczne wyszukiwanie uprawnień dostępu do zasobów, które są zarządzane przez grupę. Właściciela grupy można ustawić grupy, dzięki czemu żądań dołączenia zatwierdza się automatycznie lub wymaganie zatwierdzenia przez właściciela grupy. Gdy użytkownik tworzy żądanie dołączenia do grupy, żądanie sprzężenia jest przesłane do właścicieli grupy. Jeśli jeden z właściciele zatwierdzi żądanie, żądaniu użytkownik jest powiadamiany i użytkownik jest dołączony do grupy. Jeśli jeden właścicieli uprawnienie żądania, żądaniu użytkownika jest powiadomienia, ale nie jest dołączony do grupy.


## <a name="getting-started-with-access-management"></a>Wprowadzenie do zarządzania programu access
Chcesz zacząć pracę? Należy spróbować się kilka podstawowych zadań, które można wykonywać przy użyciu grup Azure AD. Za pomocą tych funkcji do zapewnienia specjalistyczne dostępu do różnych grup dla różnych zasobów w Twojej organizacji. Poniżej przedstawiono listę podstawowe pierwsze kroki.

* [Tworzenie prostej reguły skonfigurowanie dynamiczne członkostwa w grupie](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)

* [Korzystanie z grupy do zarządzania dostęp do aplikacji władz akredytacji bezpieczeństwa](active-directory-accessmanagement-group-saasapps.md)

* [Udostępnianie grupie dla użytkownika końcowego Sklep internetowy](active-directory-accessmanagement-self-service-group-management.md)

* [Synchronizowanie grupy lokalnej Azure za pomocą narzędzie Azure AD Connect](active-directory-aadconnect.md)

* [Zarządzanie właścicielami grupy](active-directory-accessmanagement-managing-group-owners.md)


## <a name="next-steps-for-access-management"></a>Następne kroki w celu zarządzania dostępu
Teraz, gdy masz zrozumienie podstawy zarządzania dostępu w tym miejscu są pewne dodatkowe zaawansowane funkcje dostępne w usługi Azure Active Directory do zarządzania dostęp do aplikacji i zasobów.

* [Aby utworzyć reguły zaawansowane przy użyciu atrybutów](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [Zarządzanie grupami zabezpieczeń w Azure AD](active-directory-accessmanagement-manage-groups.md)

* [Definiowanie grup dedykowane w Azure AD](active-directory-accessmanagement-dedicated-groups.md)

* [Wykres interfejsu API odwołanie do grupy](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

* [Azure cmdlet usługi Active Directory do konfigurowania ustawień grupy](active-directory-accessmanagement-groups-settings-cmdlets.md)
