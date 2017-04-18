<properties
   pageTitle="Sposób wykonania recenzji za pomocą programu access | Microsoft Azure"
   description="Po rozpoczęciu przeglądu dostępu w programie Azure AD uprzywilejowanych Zarządzanie tożsamościami Dowiedz się, jak jego wykonaniem i wyświetlanie wyników"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Jak przeprowadzić przegląd dostępu w programie Azure AD uprzywilejowanych Zarządzanie tożsamościami


Administratorzy uprzywilejowanych roli przeglądać uprzywilejowanych dostępu raz [Przegląd zabezpieczeń został uruchomiony](active-directory-privileged-identity-management-how-to-start-security-review.md). Azure AD uprawnieniach tożsamości Zarządzanie zostanie automatycznie Wyślij wiadomość e-mail, monitowania użytkowników, aby zapoznać się z ich dostęp. Jeśli użytkownik nie wyświetlony wiadomości e-mail, możesz wysłać je instrukcje w [sposób wykonywania Przegląd zabezpieczeń](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Po okresu Recenzja zabezpieczeń lub wszystkich użytkowników zakończą ich własny przeglądanie, postępuj zgodnie z instrukcjami w tym artykule Zarządzanie Przegląd oraz wyświetlić wyniki.

## <a name="manage-security-reviews"></a>Zarządzanie przeglądy zabezpieczeń

1. Przejdź do [portalu Azure](https://portal.azure.com/) i wybierz aplikację **Azure AD uprzywilejowanych Zarządzanie tożsamościami** do pulpitu nawigacyjnego.
2. Zaznacz sekcję, **program Access sprawdza** pulpitu nawigacyjnego.
3. Wybierz opcję Recenzja programu access, którą chcesz zarządzać.

Na karta szczegółów Recenzja programu access istnieje numer opcje służące do zarządzania przeglądu.

![Przyciski Recenzja PIM dostęp — zrzut ekranu][1]

### <a name="remind"></a>Przypomnij

Jeśli przeglądu programu access jest skonfigurowana, dzięki czemu użytkownicy przeglądać się, przycisk **Przypomnij** wysyła powiadomienia. 

### <a name="stop"></a>Zatrzymywanie

Wszystkie recenzje access ma daty zakończenia, ale za pomocą przycisku **Zatrzymaj** do pracy nad nią wcześnie. Wszyscy użytkownicy jeszcze nie zostały przejrzane przy tym razem, nie będzie mógł po zatrzymaniu Przegląd. Nie można uruchomić ponownie przeglądu po jest został zatrzymany.

### <a name="apply"></a>Stosowanie

Po zakończeniu recenzji za pomocą programu access, albo ponieważ osiągnięto daty zakończenia lub zatrzymania go ręcznie, przycisk **Zastosuj** wykonuje wyniku przeglądu. Jeśli na stronie Przegląd odmowa dostępu użytkownika, to krok, który spowoduje usunięcie ich przypisanie roli.  

### <a name="export"></a>Eksportowanie

Jeśli chcesz zastosować wyniki przeglądu zabezpieczeń ręcznie, możesz wyeksportować Przegląd. Przycisk **Eksportuj** rozpocznie się pobieranie pliku CSV. Możesz zarządzać wyników w programie Excel lub inne programy, które otwierają plików CSV.

### <a name="delete"></a>Usuwanie

Jeśli nie jesteś w dalszej Przegląd, usuń ją. Przycisk **Usuń** usuwa przegląd z poziomu aplikacji PIM.

> [AZURE.IMPORTANT] Nie będzie się ostrzeżenie przed usuwania, dlatego należy pamiętać, że chcesz usunąć przeglądu.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
