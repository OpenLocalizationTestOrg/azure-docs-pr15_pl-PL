<properties
    pageTitle="Azure Active Directory B2C: Atrybuty niestandardowe | Microsoft Azure"
    description="Jak używać atrybuty niestandardowe w Azure Active Directory B2C zbieranie informacji na temat osób korzystających ze"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

#  <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: Zbieranie informacji na temat osób korzystających ze przy użyciu atrybuty niestandardowe

Katalogu B2C usługi Azure Active Directory (Azure AD) zawiera wbudowane zestaw informacji (atrybuty): imię, nazwisko, Miasto, kod pocztowy i inne atrybuty. Jednak wszystkich aplikacji dla klientów indywidualnych skierowaną ma unikatowe wymagania dotyczące atrybuty, jakie zebrać konsumentów. Z Azure AD B2C można rozszerzyć zestaw atrybutów przechowywanych dla każdego konta dla klientów indywidualnych. Możesz utworzyć atrybuty niestandardowe w [Azure portal](https://portal.azure.com/) i używać go w zapisów zasad, tak jak pokazano poniżej. Można także odczytywanie i zapisywanie następujące atrybuty za pomocą [interfejsu API Azure AD wykresu](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [AZURE.NOTE]
Atrybuty niestandardowe za pomocą [rozszerzenia schematu katalogu interfejsu API Azure AD wykresu](https://msdn.microsoft.com/library/azure/dn720459.aspx).

## <a name="create-a-custom-attribute"></a>Tworzenie niestandardowego atrybutu

1. [Wykonaj poniższe czynności, aby przejść do karta funkcje B2C portalu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknij przycisk **atrybuty użytkownika**.
3. Kliknij przycisk **+ Dodaj** w górnej części karta.
4. Podaj **nazwę** dla atrybutu niestandardowego (na przykład "ShoeSize") i opcjonalnie **Opis**. Kliknij przycisk **Utwórz**.

    > [AZURE.NOTE]
    Tylko "Ciąg" **Typ danych** jest obecnie dostępny.

Atrybut niestandardowy jest teraz dostępna na liście atrybutów **użytkownika**i do użycia w zapisów zasad.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Używanie atrybut niestandardowy w zapisów zasad

1. [Wykonaj poniższe czynności, aby przejść do karta funkcje B2C portalu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknij pozycję **zasady tworzenia konta**.
3. Kliknij pozycję zasady zapisów (na przykład "B2C_1_SiUp"), aby go otworzyć. Kliknij przycisk **Edytuj** w górnej części karta.
4. Kliknij **zapisów atrybuty** i wybierz atrybut niestandardowy (na przykład "ShoeSize"). Kliknij **przycisk OK**.
5. Kliknij **oświadczeń aplikacji** i wybierz atrybut niestandardowy. Kliknij **przycisk OK**.
6. Kliknij przycisk **Zapisz** w górnej części karta.

Za pomocą funkcji "Uruchom teraz" na zasady można sprawdzić obsługi dla klientów indywidualnych. Należy teraz widoczna na liście atrybutów zebrane podczas tworzenia konta dla klientów indywidualnych "ShoeSize" i ją wyświetlić w token wysyłane do aplikacji.

## <a name="notes"></a>Notatki

- Wraz z zapisów zasady atrybutami niestandardowymi można również w zapisów lub logowania zasady i edytowanie zasad profilu.
- Jest to znane ograniczenie atrybutami niestandardowymi. Jest tworzony tylko są one używane w wszelkie zasady, a nie, po dodaniu do listy atrybutów **użytkownika**po raz pierwszy.
