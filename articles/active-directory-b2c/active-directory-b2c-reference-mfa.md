<properties
    pageTitle="B2C Azure Active Directory: Uwierzytelnianie wieloskładnikowe | Microsoft Azure"
    description="Jak włączyć uwierzytelnianie wieloskładnikowe w aplikacjach dla klientów indywidualnych skierowaną zabezpieczone Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory B2C: Włącz uwierzytelnianie wieloskładnikowe w aplikacjach dla klientów indywidualnych skierowaną

Azure Active Directory (Azure AD) B2C integruje się bezpośrednio z [Uwierzytelnianie wieloskładnikowe Azure](../multi-factor-authentication/multi-factor-authentication.md) , aby mogli dodawać drugiej warstwy zabezpieczeń do zapisywania i logowania w aplikacjach dla klientów indywidualnych skierowaną. I można to zrobić bez zapisywania jednego wiersza kodu. Obecnie obsługujemy weryfikacji wiadomość rozmowy telefonicznej i tekst. Jeśli utworzono już zasady zapisywania i logowania, możesz włączyć uwierzytelnianie wieloskładnikowe.

> [AZURE.NOTE]
Uwierzytelnianie wieloskładnikowe również może być włączone, podczas tworzenia zasad zapisów i logowania, nie tylko przez edytowanie istniejących zasad.

Ta funkcja ułatwia aplikacji obsługiwać scenariusze, takie jak:

- Nie wymaga uwierzytelniania wieloskładnikowego uzyskać dostęp do jednej aplikacji, ale potrzebna, aby uzyskać dostęp do innego. Na przykład konsumenta można zalogować się do aplikacji ubezpieczeń automatyczne za pomocą konta społecznościowych lub lokalnych, ale należy sprawdzić numer telefonu, zanim uzyskiwanie dostępu do domu aplikacji ubezpieczeń zarejestrowane w tym samym katalogu.
- Uwierzytelnianie wieloskładnikowe uzyskać dostęp do aplikacji na ogół nie wymagają, ale potrzebna, aby uzyskać dostęp do ważnych fragmentów w nim. Na przykład konsumenta zalogować się do aplikacji bankowych społecznościowe lub lokalne konto i sprawdź saldo konta, ale należy sprawdzić przed podjęciem przelewu, numer telefonu.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Modyfikowanie zapisów zasady, aby włączyć uwierzytelnianie wieloskładnikowe

1. [Wykonaj poniższe czynności, aby przejść do karta funkcje B2C portalu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknij pozycję **zasady tworzenia konta**.
3. Kliknij pozycję zasady zapisów (na przykład "B2C_1_SiUp"), aby go otworzyć.
4. Kliknij pozycję **uwierzytelnianie wieloskładnikowe** i włącz opcję **Stan** **włączone**. Kliknij **przycisk OK**.
5. Kliknij przycisk **Zapisz** w górnej części karta.

Za pomocą funkcji "Uruchom teraz" na zasady mogą zweryfikować obsługi dla klientów indywidualnych. Potwierdź następujące czynności:

Konto dla klientów indywidualnych otrzymuje utworzone w katalogu przed wystąpieniem kroku uwierzytelnianie wieloskładnikowe. W kroku konsumenta jest monit o Podaj numer telefonu i weryfikacji. Jeśli weryfikacja zakończy się pomyślnie, numer telefonu jest dołączone do konta dla klientów indywidualnych do późniejszego użycia. Nawet jeśli konsumenta anuluje lub wyeliminować za, dana osoba można poprosić o Sprawdź numer telefonu ponownie podczas następnego logowania (z uwierzytelnianie wieloskładnikowe włączone).

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Modyfikowanie zasady logowania, aby włączyć uwierzytelnianie wieloskładnikowe

1. [Wykonaj poniższe czynności, aby przejść do karta funkcje B2C portalu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknij pozycję **zasady logowania**.
3. Kliknij przycisk logowania zasad (na przykład "B2C_1_SiIn"), aby go otworzyć. Kliknij przycisk **Edytuj** w górnej części karta.
4. Kliknij pozycję **uwierzytelnianie wieloskładnikowe** i włącz opcję **Stan** **włączone**. Kliknij **przycisk OK**.
5. Kliknij przycisk **Zapisz** w górnej części karta.

Za pomocą funkcji "Uruchom teraz" na zasady można sprawdzić obsługi dla klientów indywidualnych. Potwierdź następujące czynności:

Gdy konsumenta znaki w (za pomocą konta społecznościowych lub lokalnego), jeśli numer telefonu zweryfikowanym jest dołączony do konta dla klientów indywidualnych, dana osoba jest monit w celu weryfikacji. Jeśli żaden numer telefonu jest podłączona, konsumenta jest monit o podanie jedną i weryfikacji. Na pomyślne weryfikacji numer telefonu jest powiązany z kontem dla klientów indywidualnych do późniejszego użycia.

## <a name="multi-factor-authentication-on-other-policies"></a>Uwierzytelnianie wieloskładnikowe w innych zasad

Zgodnie z opisem zasady zapisywania i logowania powyżej, jest również można włączyć uwierzytelnianie wieloskładnikowe w zapisów lub zasady logowania i hasła Resetowanie zasad. Będą dostępne wkrótce na edytowanie zasad profilu.
