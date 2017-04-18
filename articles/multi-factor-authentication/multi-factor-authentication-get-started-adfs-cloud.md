<properties
    pageTitle="Zapewnienie chmurze zasobów z Azure MFA i usług AD FS"
    description="To jest strona uwierzytelnianie wieloskładnikowe Azure opisujący sposób rozpocząć pracę z Azure MFA i usług AD FS w chmurze."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Zabezpieczanie zasobów w chmurze z uwierzytelnianie wieloskładnikowe Azure i usług AD FS

Jeśli Twoja organizacja federacji z usługą Azure Active Directory, użyj uwierzytelnianie wieloskładnikowe Azure lub usługi federacyjne Active Directory bezpiecznego zasoby, które są używane przez Azure AD. Skorzystaj z poniższych instrukcji zabezpieczania zasobów usługi Azure Active Directory za pomocą uwierzytelnianie wieloskładnikowe Azure lub usługi federacyjne Active Directory.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Zabezpieczanie zasobów Azure AD przy użyciu usług AD FS

Aby zabezpieczyć zasób chmury, najpierw włączyć konto użytkownika, a następnie skonfigurować regułę roszczeń. Wykonaj poniższą procedurę, aby wykonać kroki:

1. Aby włączyć konto za pomocą z instrukcjami podanymi w [uwierzytelnianie wieloskładnikowe włączania dla użytkowników](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) .
2. Uruchom AD FS Management console.
![Chmury](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)
3. Przejdź do **Polegaj strona zaufanie** i kliknij prawym przyciskiem myszy Polegaj strony zaufania. Wybierz pozycję **Edytuj reguły roszczeń...**
4. Kliknij przycisk **Dodaj regułę...**
5. Z listy rozwijanej zaznacz pole wyboru **Wyślij oświadczeń za pomocą reguły niestandardowe** , a następnie kliknij przycisk **Dalej**.
6. Wprowadź nazwę dla reguły roszczeń.
7. W obszarze reguły niestandardowe: Dodaj następujący tekst:

    ```
    => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");
    ```

    Odpowiedniego roszczenia:

    ```
    <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
    <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
    </saml:Attribute>
    ```

8. Kliknij **przycisk OK** , a następnie **Zakończ**. Zamknij AD FS Management console.

Następnie można wykonać logowanie się przy użyciu metody lokalnego (na przykład karty inteligentnej).

## <a name="trusted-ips-for-federated-users"></a>Zaufane adresy IP dla użytkowników federacyjnych
Adresy IP zaufanych Administratorzy na potrzeby weryfikacji dwuetapowej obejścia dla określonych adresów IP i dla federacyjnych użytkowników, którzy żądania pochodzące z poziomu własnej sieci intranet. W poniższych sekcjach opisano sposób konfigurowania zaufane adresy IP usługi Azure wieloskładnikowe uwierzytelniania z użytkownicy federacyjni i weryfikacja dwuetapowa obejścia podczas żądania pochodzi z w obrębie intranetu użytkownicy federacyjni. W tym celu należy konfigurowanie usług AD FS, aby użyć przekazującą lub filtrowanie szablonu wiadomości przychodzące roszczeń typu roszczeń w sieci firmowej.

W tym przykładzie użyto usługi Office 365 dla naszych Polegaj strona zaufanie.

### <a name="configure-the-ad-fs-claims-rules"></a>Konfigurowanie reguł oświadczeniach usług AD FS

Najpierw trzeba wykonać jest skonfigurowanie oświadczeniach usług AD FS. Zostanie utworzony oświadczeniach dwie reguły, typu roszczeń w sieci firmowej i dodatkowe jeden zachowywania naszych użytkowników zalogowany.

1. Otwórz program AD FS zarządzanie.
2. Po lewej stronie wybierz pozycję **Strona Polegaj zaufanie**.
3. Kliknij prawym przyciskiem myszy **Platformy tożsamości pakietu Microsoft Office 365** , a następnie wybierz pozycję **Edytuj roszczeń reguły...** 
 ![Chmury](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Na emisji Przekształć reguły, kliknij przycisk **Dodaj regułę.** 
 ![Chmury](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Na przekształcanie roszczeń reguły Kreatora dodawania zaznacz **Pomiń lub filtrowanie przychodzących roszczeń** z listy rozwijanej, a następnie kliknij przycisk **Dalej**.
![Chmury](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. W polu obok nazwy reguły roszczeń nazwę reguły. Na przykład: InsideCorpNet.
7. Z listy rozwijanej obok przychodzące rościć sobie typ wybierz pozycję **W sieci firmowej**.
![Chmury](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Kliknij przycisk **Zakończ**.
9. Na emisji Przekształć reguły kliknij przycisk **Dodaj regułę**.
10. Na przekształcanie roszczeń reguły Kreatora dodawania z listy rozwijanej wybierz **oświadczeń Wyślij za pomocą reguły niestandardowe** , a następnie kliknij przycisk **Dalej**.
11. W polu w obszarze Nazwa reguły roszczeń: wprowadź *Zachować użytkowników zalogowany*.
12. W polu Reguły niestandardowe wprowadź:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Chmury](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Kliknij przycisk **Zakończ**.
14. Kliknij przycisk **Zastosuj**.
15. Kliknij przycisk **Ok**.
16. Zamknij AD FS zarządzania.



### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Konfigurowanie Azure wieloskładnikowe uwierzytelniania zaufane adresy IP z użytkownikami federacyjnymi
Teraz, gdy roszczeń znajdują się w miejscu, możemy skonfigurować zaufanych adresy IP.

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com).
2. Po lewej stronie kliknij pozycję **Usługi Active Directory**.
3. W obszarze Katalog zaznacz katalogu, której chcesz skonfigurować zaufanych adresy IP.
4. W katalogu jest zaznaczona, kliknij przycisk **Konfiguruj**.
5. W sekcji uwierzytelnianie wieloskładnikowe kliknij pozycję **Ustawienia usługi Zarządzanie**.
6. Na stronie Ustawienia usługi w obszarze zaufanych adresy IP, wybierz pozycję **pominąć wieloskładnikowe uwierzytelnianie żądań federacyjnych użytkowników w mojej sieci intranet.** 
 ![Chmury](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Kliknij przycisk **Zapisz**.
8. Po zastosowaniu aktualizacji kliknij przycisk **Zamknij**.


To wszystko! W tym momencie federacyjnych użytkowników usługi Office 365 powinien mieć tylko ma być używana MFA roszczenia pochodzi spoza firmowej sieci intranet.
