<properties
   pageTitle="Narzędzie Azure AD Connect: Funkcje w podglądzie | Microsoft Azure"
   description="W tym temacie opisano w większej liczby funkcji szczegóły, które są w podglądzie Azure AD Connect."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/27/2016"
   ms.author="billmath"/>

# <a name="more-details-about-features-in-preview"></a>Więcej informacji o funkcjach w wersji preview
W tym temacie opisano, jak używać funkcji obecnie w podglądzie.

## <a name="group-writeback"></a>Grupa wystornowaniem
Opcja zapisu grupy w opcjonalne funkcje pozwoli do wystornowania **Grupami usługi Office 365** na las z serwerem Exchange jest zainstalowany. To jest grupa, które zawsze format zarządzany w chmurze. Jeśli masz lokalne programu Exchange, a następnie można napisać ponownie te grupy do lokalnego tak użytkownicy z lokalnej skrzynki pocztowej programu Exchange można wysyłać i odbierać wiadomości e-mail z tych grup.

Więcej informacji na temat grupami usługi Office 365 i sposobach ich używania można znaleźć [w tym miejscu](http://aka.ms/O365g).

Ta grupa zostanie przedstawione w postaci grupy dystrybucyjnej w lokalnym programie usług AD DS. Serwer programu Exchange lokalnego muszą być na aktualizacji skumulowanej programu Exchange 2013 8 (wydane w marca 2015 r) lub 2016 Exchange rozpoznanie nowy typ grupy.

**Notatki podczas podglądu**

- Atrybut książki adresowej jest obecnie pusta w podglądzie. Bez ten atrybut grupy nie będą widoczne na globalnej liście adresowej. Najłatwiejszym sposobem wypełnić dany atrybut jest użyć polecenia cmdlet programu PowerShell usługi Exchange `update-recipient`.
- Tylko lasy ze schematem Exchange są prawidłowe elementów docelowych dla grup. Jeśli wykryto nie programu Exchange, zapisu grupa nie będzie można włączyć.
- Obecnie obsługiwane są tylko jeden las wdrożenia organizacji programu Exchange. Jeśli masz więcej niż jedno programu Exchange organizacji lokalnej, będzie konieczne lokalnego rozwiązania GALSync grup pojawiały się w swojej innych lasów.
- Funkcja zapisu grupy nie obsługuje obecnie grupami zabezpieczeń i grupami dystrybucyjnymi.

>[AZURE.NOTE] Subskrypcję usługi Azure AD Premium jest wymagana do zapisu grupy.

## <a name="user-writeback"></a>Wystornowaniem użytkownika
> [AZURE.IMPORTANT] Usunięto przy użyciu funkcji podglądu użytkownika zapisu w aktualizacji 2015 sierpnia Azure AD Connect. Jeśli zostanie włączona, należy wyłączyć tę funkcję.

## <a name="next-steps"></a>Następne kroki
Kontynuowanie [instalacji niestandardowej: narzędzie Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](active-directory-aadconnect.md).
