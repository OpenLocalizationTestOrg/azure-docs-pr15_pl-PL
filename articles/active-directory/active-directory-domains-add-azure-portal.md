<properties
    pageTitle="Dodawanie niestandardowej nazwy domeny do usługi Azure Active Directory Podgląd | Microsoft Azure"
    description="Jak dodać firmy nazw domen do usługi Azure Active Directory i jak Sprawdź nazwę domeny."
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
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="curtand"/>

# <a name="add-a-custom-domain-name-to-azure-active-directory-preview"></a>Dodawanie niestandardowej nazwy domeny do usługi Azure Active Directory preview

> [AZURE.SELECTOR]
- [Azure portal](active-directory-domains-add-azure-portal.md)
- [Portal Azure klasyczny](active-directory-add-domain.md)

Po skonfigurowaniu nazw domen, które Twoja organizacja korzysta z czynność biznesowych i użytkowników Zaloguj się do tej samej sieci firmowej za pomocą swojej nazwy domeny firmy. Przy użyciu podglądu usługi Azure Active Directory (Azure AD), można dodać nazwę domeny firmy do także Azure AD. [Co to jest w podglądzie?](active-directory-preview-explainer.md) Dzięki temu można przypisać nazwy użytkownika w katalogu, które zaznajomieni wśród użytkowników, takich jak ‘alice@contoso.com.’ proces jest proste:

1. Dodawanie niestandardowej nazwy domeny do usługi katalogowej
2. Dodawanie wpisu DNS dla nazwy domeny u rejestratora nazw domen
3. Sprawdź nazwę domeny niestandardowej w Azure AD

## <a name="how-do-i-add-a-domain-name"></a>Jak dodać nazwy domeny?

1.  Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2.  Wybierz pozycję **więcej usług**, wprowadź **Usługi Azure Active Directory** w polu tekstowym, a następnie naciśnij **klawisz Enter**.

    ![Otwieranie, zarządzanie użytkownikami](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Na karta ***nazwy katalogu*** wybierz **nazw domen**.

4. Na karta ** *nazwy katalogu* - nazwami domen** wybierz polecenie **Dodaj** .

  ![Wybranie polecenia Dodaj](./media/active-directory-domains-add-azure-portal/add-command.png)

5. Na karta **nazwy domeny** wprowadź nazwę domeny niestandardowej w polu, na przykład "contoso.com", a następnie wybierz pozycję **Dodaj domenę**. Pamiętaj uwzględnić .com, .net lub innego rozszerzenia najwyższego poziomu.

6. Na karta ***nazwa_domeny*** (czyli kartę, która zostanie otwarta zawierającą nową nazwę domeny w tytule) Uzyskaj informacje DNS używaną Azure AD do weryfikacji, że Twoja organizacja właścicielem nazwy domeny niestandardowej.

  ![Uzyskaj informacje o dołączaniu do DNS](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Teraz, gdy dodano nazwę domeny, Azure AD należy sprawdzić, czy Twoja organizacja właścicielem nazwy domeny. Przed Azure AD może wykonywać weryfikacji, należy dodać wpis DNS w pliku strefy DNS dla nazwy domeny. To zadanie jest wykonywane w witrynie sieci Web dla rejestratorem nazwy domeny dla nazwy domeny.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Dodaj wpis DNS u rejestratora nazw domen dla domeny

Następnym krokiem, aby użyć niestandardowej nazwy domeny z usługą Azure Active Directory jest aktualizacja plik strefy DNS dla domeny. Dzięki temu Azure AD w celu zweryfikowania, że Twoja organizacja właścicielem nazwy domeny niestandardowej.

1.  Zaloguj się do rejestratora nazw domen dla domeny. Jeśli nie masz dostępu, aby zaktualizować wpis DNS, poproś osobom lub zespołom, kto ma dostęp do wykonania — krok 2 oraz informujące po jego zakończeniu.

2.  Plik strefy DNS dla domeny, można zaktualizować, dodając wpis DNS uzyskanych od Azure AD. Ten wpis DNS umożliwia Azure AD weryfikowanie własności domeny. Wpis DNS nie powoduje zmiany dowolnego zachowania, na przykład routingu poczty lub Internecie.

Aby uzyskać pomoc dotyczącą dodawania wpis DNS zapoznaj się z [instrukcjami dodawania wpis DNS u popularnych rejestratorów DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Sprawdź nazwę domeny z usługą Azure Active Directory

Po dodaniu wpis DNS możesz przystąpić do weryfikacji nazwy domeny z usługą Azure Active Directory.

Nazwa domeny można weryfikować tylko wtedy, gdy masz propagowane rekordy DNS. Propagowanie tej często trwa tylko sekund, ale czasami może potrwać godzinę lub więcej. Jeśli weryfikacja nie działa po raz pierwszy, spróbuj ponownie później.

1.  Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta, który jest administratorem globalnym katalogu.

2.  Wybierz pozycję **Przeglądaj**, wprowadź Zarządzanie użytkownikami w polu tekstowym, a następnie naciśnij **klawisz Enter**.

    ![Otwieranie, zarządzanie użytkownikami](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Na karta **Zarządzanie użytkownikami - nazwami domen** wybierz nazwę niezweryfikowany domeny, którą chcesz zweryfikować.

4. Na karta ***nazwa_domeny*** (czyli kartę, która zostanie otwarta zawierającą nową nazwę domeny w tytule) wybierz pozycję **Weryfikuj** , aby ukończyć weryfikacji.

Teraz można [przypisać nazwy użytkowników, które obejmują niestandardowej nazwy domeny](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli nie może zweryfikować niestandardowej nazwy domeny, wykonaj następujące czynności. Firma Microsoft będzie zaczynać najbardziej typowych i pracy w dół do najmniej typowych.

1.  **Poczekaj godziny**. Rekordy DNS, musisz propagowanie przed Azure AD może zweryfikować domenę. Może wymagać godzinę lub więcej.

2.  **Upewnij się wprowadzono rekordu DNS, oraz czy jest poprawny**. Wykonać ten krok w witrynie sieci Web dla rejestratora nazw domen dla domeny. Azure AD nie może zweryfikować nazwy domeny, jeśli wpis DNS nie znajduje się w pliku strefy DNS lub jeśli nie ma dokładnego odpowiednika wpisem DNS tego Azure AD ile. Jeśli nie masz dostępu do zaktualizuj rekordy DNS dla domeny u rejestratora nazw domen, udostępnianie wpis DNS innym osobom lub zespołom w organizacji, kto ma dostęp i poproś ich, aby dodać wpis DNS.

3.  **Usuwanie nazwy domeny z innego katalogu Azure AD**. Nazwa domeny można weryfikować w jednym katalogu. Jeśli nazwa domeny wcześniej została zweryfikowana w innym katalogu, muszą zostać usunięte przed może zostać sprawdzony w katalogu nowy. Aby uzyskać informacje o usuwaniu nazw domen, przeczytaj [Zarządzanie niestandardowe nazwy domen](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Dodać więcej nazw domeny niestandardowej

Jeśli organizacja korzysta z wielu nazwy domeny niestandardowej, na przykład "contoso.com" i "contosobank.com", możesz dodać je maksymalnie 900 nazw domen. Wykonaj te same kroki w tym artykule, aby dodać każdej z nazw domen.

## <a name="next-steps"></a>Następne kroki

[Zarządzanie nazwami domen niestandardowych](active-directory-domains-manage-azure-portal.md)
