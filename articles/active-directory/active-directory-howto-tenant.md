<properties
    pageTitle="Jak uzyskać dzierżawę Azure AD | Microsoft Azure"
    description="Jak uzyskać dzierżawę usługi Azure Active Directory rejestrowania i tworzenia aplikacji."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# <a name="how-to-get-an-azure-active-directory-tenant"></a>Jak uzyskać dzierżawę usługi Azure Active Directory

W usługi Azure Active Directory (Azure AD), [dzierżawy](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) jest przedstawiciela organizacji.  Jest to dedykowane wystąpienie usługi Azure AD organizacji otrzyma i właścicielem po zarejestrowaniu go do usługi w chmurze firmy Microsoft Azure, Intune firmy Microsoft lub usługi Office 365.  Każdy dzierżawy Azure AD jest odrębnych i są oddzielone od innych dzierżaw Azure AD.  

Dzierżawy zawiera użytkowników w firmie i informacje o ich - hasła, danych profilów użytkowników, uprawnień i tak dalej.  Zawiera również grupy, aplikacji i inne informacje dotyczące organizacji i zabezpieczeń.

Aby umożliwić użytkownikom Azure AD Zaloguj się do aplikacji, musisz zarejestrować aplikację w dzierżawie własny.  Publikowanie aplikacji w dzierżawie usługi Azure AD jest **naprawdę bezpłatne**.  W rzeczywistości większość projektantów utworzy kilka dzierżaw i aplikacje dla doświadczeń, rozwoju, przemieszczenia i testowania.  Opcjonalnie organizacji, w których konta w górę i używanie aplikacji możliwość zakupu licencji, jeśli chcą korzystać z funkcji zaawansowanych katalogu.

Tak jak przejściu Uzyskiwanie dzierżawy usługi Azure AD?  Proces może być mała Jeżeli różnych możesz:

- [Masz istniejącej subskrypcji usługi Office 365](#use-an-existing-office-365-subscription)
- [Masz istniejącej subskrypcji Azure skojarzone z Account firmy Microsoft](#use-an-msa-azure-subscription)
- [Masz istniejącej subskrypcji Azure skojarzonych za pomocą konta organizacji](#use-an-organizational-azure-subscription)
- [Żadna z powyższych mają i chcesz zacząć od podstaw](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Użyj istniejącej subskrypcji usługi Office 365
Jeśli masz istniejącej subskrypcji usługi Office 365, masz już dzierżawę Azure AD! Możesz zalogować się do [portalu Azure](https://portal.azure.com) za pomocą konta usługi Office 365 i rozpocząć korzystanie z narzędzia Azure AD.

## <a name="use-an-msa-azure-subscription"></a>Za pomocą subskrypcji usługi MSA Azure
Jeśli wcześniej utworzono konto Azure subskrypcji za pomocą Account Microsoft poszczególnych, masz już dzierżawy!  Po zalogowaniu się do [Portalu Azure](https://portal.azure.com)można będzie automatycznie być zalogowany do Twojej dzierżawy domyślne. Mogą używać dzierżawy odpowiednio do potrzeb -, ale być może zechcesz utworzyć konto administratora organizacji.

Aby to zrobić, wykonaj następujące kroki.  Alternatywnie możesz utworzyć nowej dzierżawy i tworzenie administrator w dzierżawy po podobny proces.

1.  Zaloguj się do usługi [Azure Portal](https://portal.azure.com) za pomocą konta poszczególnych
2.  Przejdź do sekcji "Usługi Azure Active Directory" portalu (znajdujący się na pasku nawigacji z lewej strony w obszarze **Więcej usług**)
3.  Możesz automatycznie powinny być zalogowani do "Domyślny katalog", jeśli nie można przełączać katalogów, klikając nazwę konta w prawym górnym rogu.
4.  W sekcji **Szybkie zadania** wybierz pozycję **Dodaj użytkownika**.
5.  W formularzu Dodawanie wprowadź następujące informacje:

    - Nazwa: (wybierz odpowiednią wartość)
    - Nazwa użytkownika: (Wybierz nazwę użytkownika dla tego administratora)
    - Profil: (wpisz odpowiednie wartości dla imię, ostatni nazwa, stanowisko i dział)
    - Rola: Administrator globalny

6.  Po rozwiązaniu formularzu Dodawanie użytkownika i odbierać hasło tymczasowe dla nowego użytkownika administracyjne Pamiętaj zarejestrować to hasło, jak należy się zalogować przy użyciu tego nowego użytkownika, aby zmienić hasło. Możesz też wysyłać hasło bezpośrednio do użytkownika, przy użyciu alternatywnych poczty e-mail.
7.  Kliknij polecenie **Utwórz** , aby utworzyć nowego użytkownika.
8.  Aby zmienić hasło tymczasowe, zaloguj się do [https://login.microsoftonline.com](https://login.microsoftonline.com) z tego konta użytkowników i zmienianie hasła na żądanie.


## <a name="use-an-organizational-azure-subscription"></a>Za pomocą subskrypcji usługi Azure organizacji
Jeśli wcześniej utworzono konto Azure subskrypcji za pomocą konta organizacji, masz już dzierżawy!  W [Azure Portal](https://portal.azure.com)należy odnaleźć dzierżawy po przejściu do "Więcej usług" i "Azure Active Directory."  Mogą używać dzierżawy odpowiednio do potrzeb. 


## <a name="start-from-scratch"></a>Tworzenie od podstaw
W przypadku wszystkich powyższych dziwny do Ciebie, nie martw się.  Po prostu odwiedź [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) do konta w usłudze Azure z nowej organizacji.  Po zakończeniu procesu będą mieć własne bardzo dzierżawy Azure AD przy użyciu nazwy domeny wybranej podczas logowania się.  W [Azure Portal](https://portal.azure.com)dzierżawy usługi można znaleźć, przechodząc do "Usługi Azure Active Directory" w funkcję po lewej

W ramach procesu tworzący konto w usłudze Azure trzeba będzie zapewnienie szczegóły karty kredytowej.  Przejściem z pewnością — możesz nie zostanie obciążona publikowanie aplikacji w Azure AD lub tworzenia nowych dzierżaw.
