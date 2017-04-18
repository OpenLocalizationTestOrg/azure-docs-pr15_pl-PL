<properties
   pageTitle="Tworzenie kapitału w portalu usługi | Microsoft Azure"
   description="W tym artykule opisano sposób tworzenia nowej aplikacji usługi Active Directory i wystawcy usługi, który może być używany z kontrola dostępu oparta na rolach w Menedżerze zasobów Azure zarządzać dostępem do zasobów."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# <a name="use-portal-to-create-active-directory-application-and-service-principal-that-can-access-resources"></a>Tworzenie aplikacji usługi Active Directory i wystawcy usługi, która ma dostęp do zasobów za pomocą portalu

> [AZURE.SELECTOR]
- [Programu PowerShell](resource-group-authenticate-service-principal.md)
- [Polecenie Azure](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Jeśli masz aplikację, która musi uzyskać dostęp do lub modyfikować zasobów, należy skonfigurować aplikację usługi Active Directory (AD) i przypisywanie uprawnień wymaganych do niego. W tym temacie przedstawiono sposób wykonywania tych kroków za pośrednictwem portalu. Obecnie należy użyć portalu klasyczny do utworzenia nowej aplikacji usługi Active Directory, a następnie przejdź do portalu Azure, aby przypisać rolę do aplikacji. 

> [AZURE.NOTE] Kroki opisane w tym temacie mają zastosowanie tylko podczas tworzenia aplikacji AD przy użyciu **portalu klasyczny** . **Jeśli używasz Azure portal do tworzenia aplikacji AD tych czynności nie powiedzie się.** 
>
> Użytkownik może ułatwić do skonfigurowania aplikacji AD i usługi kapitału za pomocą [programu PowerShell](resource-group-authenticate-service-principal.md) lub [Interfejsu wiersza polecenia Azure](resource-group-authenticate-service-principal-cli.md), zwłaszcza jeśli chcesz używać certyfikatu uwierzytelniania. W tym temacie nie pokazuje jak za pomocą certyfikatu.

Aby uzyskać wyjaśnienie pojęć związanych z usługą Active Directory zobacz [obiekty aplikacji i usług kapitału](./active-directory/active-directory-application-objects.md). Aby uzyskać więcej informacji na temat uwierzytelniania usługi Active Directory zobacz [Scenariusze uwierzytelniania Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Aby uzyskać szczegółowe instrukcje na integrowanie aplikacji Azure zarządzania zasobami zobacz [Przewodnik po dewelopera do autoryzacji przy użyciu interfejsu API Menedżera zasobów Azure](resource-manager-api-authentication.md).

## <a name="create-an-active-directory-application"></a>Tworzenie aplikacji usługi Active Directory

1. Zaloguj się do swojego konta Azure za pomocą [portalu klasyczny](https://manage.windowsazure.com/). 

2. Upewnij się, że wiesz, domyślne usługi Active Directory dla subskrypcji. Można tylko udzielić dostępu dla aplikacji w tym samym katalogu co subskrypcji. Wybierz pozycję **Ustawienia** , a następnie odszukaj nazwę katalogu skojarzonego z subskrypcją usługi.  Aby uzyskać więcej informacji zobacz [jak Azure subskrypcje są skojarzone z usługi Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).
   
     ![Znajdowanie katalogu](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. Wybierz pozycję **Usługi Active Directory** w okienku po lewej stronie.

     ![Wybierz pozycję usługi Active Directory](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Wybierz pozycję usługi Active Directory, który ma być używany do tworzenia aplikacji. Jeśli masz więcej niż jeden usługi Active Directory, utworzyć aplikację w domyślnym katalogu dla subskrypcji.   

     ![Wybierz katalog](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. Aby wyświetlić aplikacje w katalogu, wybierz pozycję **aplikacje**.

     ![Wyświetl aplikacje](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Jeśli aplikacji nie utworzono w katalogu przed, zobacz podobnej na poniższej ilustracji. Wybierz pozycję **Dodaj APLIKACJĘ**

     ![Dodawanie aplikacji](./media/resource-group-create-service-principal-portal/create-application.png)

     Lub kliknij przycisk **Dodaj** w dolnym okienku.

     ![Dodawanie](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Wybierz typ aplikacji, którą chcesz utworzyć. Ten samouczek wybierz pozycję **Dodaj aplikację, którą rozwija się w mojej organizacji**. 

     ![Nowa aplikacja](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Podaj nazwę aplikacji, a następnie wybierz typ aplikacji, którą chcesz utworzyć. Ten samouczek tworzenie **Interfejs API sieci WEB i/lub aplikacji sieci WEB** i kliknij przycisk Dalej. Jeśli wybierzesz **NATYWNEJ aplikacji KLIENCKIEJ**, pozostałe kroki w tym artykule nie będą zgodne środowiska pracy użytkownika.

     ![Nazwa aplikacji](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Wypełnij właściwości aplikacji. Dla **Adresu URL logowania na**przekazać go do witryny sieci web, który opisuje aplikacja. Istnienia witryny sieci web nie jest sprawdzany. W przypadku **Aplikacji identyfikator URI**zapewnić identyfikator URI, który identyfikuje aplikację.

     ![właściwości aplikacji](./media/resource-group-create-service-principal-portal/app-properties.png)

Utworzono aplikacji.

## <a name="get-client-id-and-authentication-key"></a>Uzyskiwanie klucza uwierzytelniania i identyfikator klienta

Podczas logowania programowy, potrzebny jest identyfikator aplikacji. Jeśli aplikacja działa w obszarze własnej poświadczenia, należy również klucz uwierzytelniania.

1. Wybierz kartę **Konfiguruj** , aby skonfigurować hasło aplikacji.

     ![Konfigurowanie aplikacji](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Skopiuj **identyfikator klienta**.
  
     ![Identyfikator klienta](./media/resource-group-create-service-principal-portal/client-id.png)

3. Jeśli aplikacja działa w obszarze własnej poświadczeń, przewiń w dół do sekcji **klawiszy** i wybierz jakim chcesz hasło jest nieprawidłowy.

     ![klawisze](./media/resource-group-create-service-principal-portal/create-key.png)

4. Wybierz pozycję **Zapisz** , aby utworzyć klucza.

     ![Zapisywanie](./media/resource-group-create-service-principal-portal/save-icon.png)

     Zostanie wyświetlona zapisany klucz i można go skopiować. Nie będą mogli pobrać później klucza więc skopiuj go teraz.

     ![zapisane klucza](./media/resource-group-create-service-principal-portal/save-key.png)

## <a name="get-tenant-id"></a>Pobierz identyfikator dzierżawy

Podczas logowania programowo, należy przekazać identyfikator dzierżawy z żądania uwierzytelniania. Dla aplikacji sieci Web i aplikacjach interfejsu API sieci Web można pobrać identyfikator dzierżawy, wybierając **Widok punkty końcowe** w dolnej części ekranu i pobieranie identyfikatora, jak pokazano na poniższej ilustracji.  

   ![Identyfikator dzierżawy](./media/resource-group-create-service-principal-portal/save-tenant.png)

Można także pobrać identyfikator dzierżawy przy użyciu programu PowerShell:

    Get-AzureRmSubscription

Lub polecenie Azure:

    azure account show --json

## <a name="set-delegated-permissions"></a>Ustawianie delegowane uprawnień

Jeśli aplikacja uzyskuje dostęp do zasobów w imieniu zalogowany użytkownik, musi udzielić aplikacji delegowanych uprawnień dostępu do innych aplikacji. Udostępnić w sekcji **uprawnienia do innych aplikacji** na karcie **Konfiguruj** . Domyślnie delegowanych uprawnień jest już włączone dla usługi Azure Active Directory. Pozostaw to uprawnienie delegowanej bez zmian.

1. Wybierz **aplikację, Dodaj**.

2. Z listy wybierz **Interfejs API systemu Windows Azure usługi zarządzania**. Następnie wybierz ikonę ukończone.

      ![Wybierz aplikację](./media/resource-group-create-service-principal-portal/select-app.png)

3. Na liście rozwijanej delegowanych uprawnień zaznacz **Zarządzanie usługą Azure programu Access jako organizacji**.

      ![Wybierz uprawnienia](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. Zapisz zmiany.

## <a name="assign-application-to-role"></a>Przypisywanie aplikacji do roli

Jeśli aplikacja działa w osobnym poświadczeń, należy przypisać aplikacji do roli. Zdecyduj, których rola reprezentuje odpowiednich uprawnień dla aplikacji. Aby uzyskać informacje o dostępnych rolach, zobacz [RBAC: wbudowana role](./active-directory/role-based-access-built-in-roles.md). 

Aby przypisać rolę do aplikacji, możesz masz odpowiednich uprawnień. W szczególności, musisz mieć `Microsoft.Authorization/*/Write` za pośrednictwem rola [właściciel](./active-directory/role-based-access-built-in-roles.md#owner) lub [Administrator dostępu użytkowników](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) uprawnienia dostępu. Rola współautora nie ma dostępu poprawne.

Możesz ustawić zakresu na poziomie subskrypcji, grupa zasobów lub zasobu. Uprawnienia są dziedziczone niższych poziomów zakresu. Na przykład dodawanie aplikacji do roli czytnika dla grupy zasobów oznacza, że można przeczytać, grupa zasobów i wszystkie zasoby, które zawiera.

1. Aby przypisać tę aplikację do roli, przełączanie z portalu Klasyczny [Azure portal](https://portal.azure.com).

1. Sprawdź swoje uprawnienia, aby upewnić się, że możesz przypisać wystawcy usługi roli. Wybierz pozycję **Moje uprawnienia** dla Twojego konta.

    ![Wybierz pozycję Moje uprawnienia](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. Wyświetlanie przypisane uprawnienia dla Twojego konta. Jak już wspomniano, musi należeć do ról właściciel lub Administrator dostępu użytkowników lub dostosowane rolę zapewniającej zapisu dla Microsoft.Authorization. Poniższa ilustracja przedstawia konta, które przydzielono do roli współautora dla subskrypcji, która nie jest odpowiednie uprawnienia, aby przypisać aplikację do roli.

    ![Pokaż Moje uprawnienia](./media/resource-group-create-service-principal-portal/show-permissions.png)

     Jeśli nie masz odpowiednich uprawnień, aby udzielić dostępu do aplikacji, należy albo żądanie, że administrator subskrypcji doda Cię do roli administratora dostępu użytkowników lub poprosić administratora udziela dostępu do aplikacji.

1. Przejdź do poziomu zakresu, które chcesz przypisać tę aplikację do. W celu przypisania roli w zakresie subskrypcji, wybierz pozycję **Subskrypcje**.

     ![Wybierz subskrypcję](./media/resource-group-create-service-principal-portal/select-subscription.png)

     Wybierz pozycję danej subskrypcji, aby przypisać tę aplikację do.

     ![Wybierz subskrypcję dla przydziału](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

     Wybierz ikonę **programu Access** w prawym górnym rogu.

     ![Wybierz pozycję dostęp](./media/resource-group-create-service-principal-portal/select-access.png)
     
     Lub w celu przypisania roli w zakresie grupa zasobów, przejdź do grupy zasobów. Karta Grupa zasobów zaznacz **Kontrola dostępu**.

     ![Wybierz użytkowników](./media/resource-group-create-service-principal-portal/select-users.png)

     Poniższe kroki są takie same dla dowolnego zakresu.

2. Wybierz pozycję **Dodaj**.

     ![Wybierz pozycję Dodaj](./media/resource-group-create-service-principal-portal/select-add.png)

3. Wybierz rolę, **czytnik** (lub niezależnie od ich roli chcesz przypisać aplikacji).

     ![Wybierz rolę](./media/resource-group-create-service-principal-portal/select-role.png)

4. Gdy zostanie wyświetlony pierwszy listy użytkowników, które można dodać do roli, nie zobaczą aplikacji. Będziesz widzieć tylko użytkownicy i grupy.

     ![Pokaż użytkowników](./media/resource-group-create-service-principal-portal/show-users.png)

5. Aby znaleźć aplikację, należy wyszukać go. Zacznij wpisywać nazwę aplikacji, a następnie zmieni się na liście dostępnych opcji. Wybierz aplikację, gdy zostanie wyświetlony na liście.

     ![Przypisz do roli](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Wybierz **rekord** do końca przypisywanie roli. Powinien zostać wyświetlony na liście aplikacji zastosowań przypisane do ról dla grupy zasobów.


Aby uzyskać więcej informacji o przypisywaniu użytkowników i aplikacje do ról za pośrednictwem portalu zobacz [Używanie przypisania roli zarządzać dostępem do zasobów Azure subskrypcji](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal).

## <a name="sample-applications"></a>Przykładowe aplikacje

Następujące aplikacje przykładowych pokazano, jak logować się jako wystawcy usługi.

**.NET**

- [Wdrażanie SSH włączone maszyn wirtualnych za pomocą szablonu z .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Zarządzanie Azure zasobów i grup zasobów program .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Wprowadzenie do zasobów — wdrażanie przy użyciu szablonu Azure Menedżera zasobów — w Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Wprowadzenie do zasobów — Zarządzanie grupa zasobów — w Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Wdrażanie SSH włączone maszyn wirtualnych za pomocą szablonu w Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Zarządzanie Azure zasobów i grup zasobów z Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Wdrażanie SSH włączone maszyn wirtualnych za pomocą szablonu w Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Zarządzanie Azure zasobów i grup zasobów z Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Dopiskiem**

- [Wdrażanie SSH włączone maszyn wirtualnych za pomocą szablonu w dopiskiem](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Zarządzanie Azure zasobów i grup zasobów z dopiskiem](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje o Określanie zasad zabezpieczeń, zobacz [Kontrola dostępu oparta na rolach Azure](./active-directory/role-based-access-control-configure.md).  
- Aby wyświetlić pokaz wideo te kroki zobacz [Włączanie programowe zarządzanie zasobu Azure z usługą Azure Active Directory](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory).

