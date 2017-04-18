<properties 
   pageTitle="Dodawanie usługi Azure Active Directory za pomocą połączenia usług w programie Visual Studio | Microsoft Azure"
   description="Dodawanie usługi Azure Active Directory przy użyciu okna dialogowego Visual Studio Dodaj połączenie usługi"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="active-directory"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Dodawanie usługi Azure Active Directory za pomocą połączenia usług w programie Visual Studio 

##<a name="overview"></a>Omówienie
Korzystając z usługi Azure Active Directory (Azure AD), mogą obsługiwać pojedynczego logowania jednokrotnego (SSO) dla aplikacji sieci web programu ASP.NET MVC lub AD uwierzytelniania w usługach interfejs API sieci Web. Azure AD uwierzytelniania użytkowników można użyć ich konta z Azure AD nawiązywania połączenia z aplikacji sieci web. Między innymi zwiększone bezpieczeństwo danych korzyści Azure AD uwierzytelnianie za pomocą interfejsu API sieci Web, gdy Uwidacznianie interfejs API z aplikacji sieci web. Dzięki Azure AD nie trzeba zarządzania systemem osobnego uwierzytelniania z zarządzaniem konta i użytkownika.

## <a name="supported-project-types"></a>Typy obsługiwanego projektów

Okno dialogowe połączenie usługi umożliwia nawiązywanie połączenia z Azure AD w następujących typów projektu.

- Projekty MVC programu ASP.NET

- Projekty interfejsu API sieci Web programu ASP.NET


### <a name="connect-to-azure-ad-using-the-connected-services-dialog"></a>Nawiązywanie połączenia z Azure AD przy użyciu okna dialogowego usługami

1. Upewnij się, że masz konto usługi Azure. Jeśli nie masz konta usługi Azure, można Załóż [bezpłatnej wersji próbnej](http://go.microsoft.com/fwlink/?LinkId=518146).

1. W programie Visual Studio Otwórz menu skrótów węzła **odwołania** w projekcie i wybierz pozycję **Dodaj usługi połączone**.
1. Wybierz pozycję **Azure AD uwierzytelniania** , a następnie wybierz pozycję **Konfiguruj**.

    ![Wybierz pozycję Dodaj uwierzytelniania Azure AD](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Na pierwszej stronie **Konfigurowanie Azure AD uwierzytelniania**zaznacz **Konfigurowanie logowania jednokrotnego przy użyciu Azure AD**.

    Jeśli projekt jest skonfigurowany z inną konfigurację uwierzytelniania, Kreator ostrzega, że kontynuowanie wyłączy poprzednią konfigurację.

    ![Konfigurowanie Azure AD w Kreatorze](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1.  Na drugiej stronie wybierz domenę z listy rozwijanej **domeny** . Na liście domen zawiera wszystkie dostępne domeny przez kont wymienionych w oknie dialogowym Ustawienia kont. Alternatywnie można wprowadzić nazwę domeny, jeśli nie możesz znaleźć, którego szukasz, takich jak mydomain.onmicrosoft.com. Można wybrać opcję, aby utworzyć nową aplikację Azure AD lub za pomocą ustawień z istniejącej aplikacji Azure AD. 

    ![Konfigurowanie Azure AD w Kreatorze](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)


1. Na trzeciej stronie kreatora upewnij się, że **więcej danych katalogu** jest zaznaczone. Kreator wypełni **tajny klienta**. 

    ![Konfigurowanie Azure AD w Kreatorze](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Wybierz przycisk **Zakończ** . Okno dialogowe dodaje kod konfiguracji konieczne i odwołania umożliwiające projektu uwierzytelniania Azure AD. Domeny AD można wyświetlić na [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Przejrzyj stronę wprowadzenie wyświetlana w przeglądarce pomysłów na następne kroki i stronę co się stało, aby zobaczyć, jak modyfikacji projektu. Jeśli chcesz sprawdzić, czy wszystko działało, otwórz jeden z plików konfiguracyjnych zmienione i sprawdź, czy ustawienia wymienionych w co się stało z istnieją. Na przykład głównym web.config w projekcie ASP.NET MVC ustawienia zostaną dodane:

        <appSettings> 
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:AADInstance" value="https://login.windows.net/" />
            <add key="ida:Domain" value="Your selected domain" />
            <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
            <add key="ida:PostLogoutRedirectUri" value="The default redirect URI from the project" />
        </appSettings>

## <a name="how-your-project-is-modified"></a>Jak zmienić projektu

Po uruchomieniu kreatora program Visual Studio dodaje Azure AD i skojarzone odwołania do projektu. Konfiguracja i kod pliki z projektem są również zmodyfikowany w celu dodania obsługę Azure AD. Określone modyfikacje Visual Studio powoduje, że zależy od typu projektu. Aby uzyskać szczegółowe informacje na temat jak zmienić ASP.NET MVC projektów zobacz [Jakie projektów MVC stało —](http://go.microsoft.com/fwlink/p/?LinkID=513809). W przypadku projektów interfejs API sieci Web Zobacz, [co się stało z — projekty interfejsu API sieci Web](http://go.microsoft.com/fwlink/p/?LinkId=513810).

##<a name="next-steps"></a>Następne kroki

Zadawanie pytań i uzyskiwanie pomocy.

 - [Forum w witrynie MSDN: Azure AD](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)

 - [Dokumentacja Azure AD](https://azure.microsoft.com/documentation/services/active-directory/)

 - [Wpis w blogu: Wprowadzenie do Azure AD](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

