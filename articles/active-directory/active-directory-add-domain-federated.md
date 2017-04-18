<properties
    pageTitle="Dodawanie niestandardowej nazwy domeny oraz konfigurowanie federacyjnych logowania jednokrotnego usługi Azure Active Directory | Microsoft Azure"
    description="Jak dodać firmy nazw domen do usługi Azure Active Directory i jak skonfigurować federacyjnych logowania jednokrotnego między usługi Azure Active Directory i rozwiązanie Federacji lokalnej."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Dodawanie niestandardowej nazwy domeny do usługi Azure Active Directory

Można skonfigurować niestandardową nazwę domeny, takich jak "contoso.com", dzięki czemu użytkownicy w contoso.com mają federacyjnych pojedynczego logowania jednokrotnego doświadczenie z siecią firmową. Jeśli masz już Active Directory Federation Services (AD FS) lub serwer federacyjny różnych uruchomiony w sieci firmowej, można skonfigurować Azure AD, aby użyć niestandardowej nazwy domeny przy użyciu narzędzia narzędzie Azure AD Connect. Można także wdrożyć nowe środowisko usług AD FS za pomocą narzędzie Azure AD Connect i skonfigurować którego dla federacyjnych rejestracji jednokrotnej do Azure AD.

Jeśli nie masz i nie zamierzasz wdrożyć usług AD FS lub innym serwerze federacyjnym, wykonaj następujące instrukcje: [Dodawanie niestandardowej nazwy domeny do usługi Azure Active Directory](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Dodawanie niestandardowej nazwy domeny do usługi katalogowej

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com/) przy użyciu konta użytkownika, który jest administratorem globalnym katalogu Azure AD.

2. W **Usłudze Active Directory**Otwórz katalog i wybierz kartę **domeny** .

3. Na pasku poleceń wybierz pozycję **Dodaj**, a następnie wprowadź nazwę domeny niestandardowej, na przykład "contoso.com". Pamiętaj uwzględnić .com, .net lub innego rozszerzenia najwyższego poziomu.

4. Zaznacz pole wyboru **należy zaplanować konfigurowanie tej domeny dla rejestracji jednokrotnej z mojej lokalnej usługi Active Directory** .

5. Wybierz pozycję **Dodaj**.

Uruchom narzędzie Azure AD Connect uzyskać wpisu DNS, używające Azure AD weryfikowanie domeny. Zostanie wyświetlony wpis DNS w kroku **Azure AD domeny** w kreatorze. Co może wyświetlać tego kroku kreatora wygląda [w tych instrukcjach](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Jeśli nie masz narzędzie Azure AD Connect, możesz [pobrać ją tutaj](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Dodaj wpis DNS u rejestratora nazw domen dla domeny

Następnym krokiem, aby użyć niestandardowej nazwy domeny z usługą Azure Active Directory jest aktualizacja plik strefy DNS dla domeny. Dzięki temu Azure AD w celu zweryfikowania, że Twoja organizacja właścicielem nazwy domeny niestandardowej.

1. Zaloguj się do witryny sieci Web dla rejestratora nazw domen za nazwę domeny. Jeśli nie masz dostępu, aby wykonać tę czynność, poproś osobom lub zespołom w organizacji, kto ma dostęp do wykonania — krok 2 oraz informujące po jego zakończeniu.

2. Plik strefy DNS dla domeny, można zaktualizować, dodając wpis DNS uzyskanych od Azure AD. Ten wpis DNS umożliwia Azure AD weryfikowanie własności domeny. Wpis DNS nie powoduje zmiany dowolnego zachowania, na przykład routingu poczty lub Internecie.

Aby uzyskać pomoc dotyczącą ten krok przeczytaj [instrukcje dotyczące dodawania wpis DNS u popularnych rejestratorów DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Sprawdź nazwę domeny z usługą Azure Active Directory

Po dodaniu wpis DNS możesz przystąpić do weryfikacji nazwy domeny z usługą Azure Active Directory.

Weryfikowanie domeny, wybierz przycisk **Dalej** w kroku kreatora Azure AD Connect **Azure AD domeny** . Azure AD wyszuka wpis DNS w pliku strefy DNS dla domeny. Gdy rekordy DNS zostały rozpropagowane Azure AD tylko Sprawdź nazwę domeny. Propagowanie często trwa tylko sekund, ale czasami może potrwać godzinę lub więcej. Jeśli weryfikacja nie działa po raz pierwszy, spróbuj ponownie później.

Następnie kontynuować pozostałe kroki w Kreatorze Azure AD Connect. To zsynchronizuje użytkowników z serwera AD systemu Windows Azure AD. Synchronizowani użytkownicy w domenie, który skonfigurowano do Federacji będzie mógł uzyskać federacyjnych pojedynczego logowania jednokrotnego środowisko z siecią firmową Azure AD.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli nie może zweryfikować niestandardowej nazwy domeny, wykonaj następujące czynności. Firma Microsoft będzie zaczynać najbardziej typowych i pracy w dół do najmniej typowych.

1.  **Poczekaj godziny**. Rekordy DNS, musisz propagowanie przed Azure AD może zweryfikować domenę. Może wymagać godzinę lub więcej.

2.  **Upewnij się wprowadzono rekordu DNS, oraz czy jest poprawny**. Wykonać ten krok w witrynie sieci Web dla rejestratora nazw domen dla domeny. Azure AD nie może zweryfikować nazwy domeny, jeśli wpis DNS nie znajduje się w pliku strefy DNS lub jeśli nie ma dokładnego odpowiednika wpisem DNS tego Azure AD ile. Jeśli nie masz dostępu do zaktualizuj rekordy DNS dla domeny u rejestratora nazw domen, udostępnianie wpis DNS innym osobom lub zespołom w organizacji, kto ma dostęp i poproś ich, aby dodać wpis DNS.

3.  **Usuwanie nazwy domeny z innego katalogu Azure AD**. Nazwa domeny można weryfikować w jednym katalogu. Jeśli nazwa domeny wcześniej została zweryfikowana w innym katalogu, muszą zostać usunięte przed może zostać sprawdzony w katalogu nowy. Aby uzyskać informacje o usuwaniu nazw domen, przeczytaj [Zarządzanie niestandardowe nazwy domen](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Dodać więcej nazw domeny niestandardowej

Jeśli organizacja korzysta z wielu nazwy domeny niestandardowej, na przykład "contoso.com" i "contosobank.com", możesz dodać je maksymalnie 900 nazw domen. Wykonaj te same kroki w tym artykule, aby dodać każdej z nazw domen.

## <a name="next-steps"></a>Następne kroki

-   [Zarządzanie nazwami domen niestandardowych](active-directory-add-manage-domain-names.md)
-   [Więcej informacji na temat pojęć związanych z zarządzaniem domeny w Azure AD](active-directory-add-domain-concepts.md)
-   [Umożliwia wyświetlenie Twojej firmie jest związane ze znakowaniem, użytkownicy się zalogować](active-directory-add-company-branding.md)
-   [Zarządzanie nazwami domen w Azure AD przy użyciu programu PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
