<properties
   pageTitle="Publikowanie aplikacji dla poszczególnych użytkowników w ramach zbioru Azure RemoteApp (wersja Preview) | Microsoft Azure"
   description="Dowiedz się, jak opublikować aplikacje dla poszczególnych użytkowników, a nie w zależności od grupy w Azure RemoteApp."
   services="remoteapp-preview"
   documentationCenter=""
   authors="piotrci"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="piotrci"/>

# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Publikowanie aplikacji dla poszczególnych użytkowników w ramach zbioru Azure RemoteApp (wersja Preview)

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

W tym artykule wyjaśniono, jak publikować aplikacje dla poszczególnych użytkowników w ramach zbioru Azure RemoteApp. Nowe funkcje w Azure RemoteApp to jest obecnie w "prywatne preview" i jest dostępny tylko do wybrania pilotażowe na potrzeby obliczeń.

Pierwotnie Azure RemoteApp włączone tylko jednym ze sposobów "publikowania" aplikacji: administrator może publikować aplikacje z obrazu i będą widoczne dla wszystkich użytkowników w kolekcji.

Typowy scenariusz jest zawiera wiele aplikacji w jeden obraz i wdrażanie jednej kolekcji, aby zmniejszyć koszty zarządzania. Oftentimes nie wszystkie aplikacje są odpowiednie dla wszystkich użytkowników — Administratorzy wolisz publikowanie aplikacji dla poszczególnych użytkowników, nie widzą niepotrzebne aplikacje w zastosowaniach kanału.

Jest to teraz możliwe w RemoteApp Azure — obecnie jako funkcji podglądu ograniczony. Oto krótkie podsumowanie nowe funkcje:

1. Kolekcja można ustawić w jednym z dwóch trybów:
 
  - oryginalna "Tryb zbioru", gdzie można zobaczyć wszystkich użytkowników w zbiorze wszystkie opublikowane aplikacje. To jest domyślny tryb.
  - Nowy "aplikacji tryb", miejsce, w którym użytkownicy widzą tylko aplikacje, które zostały jawnie przypisane do nich

2. W momencie tryb aplikacji można włączyć tylko przy użyciu poleceń cmdlet środowiska RemoteApp PowerShell Azure.

  - Gdy ustawiony tryb aplikacji, przypisywanie użytkownika w kolekcji nie mogą być zarządzane za pośrednictwem portalu Azure. Przypisywanie użytkownika ma można zarządzać za pomocą poleceń cmdlet środowiska PowerShell.

3. Użytkownicy będą widzieć tylko aplikacji opublikowanych bezpośrednio do nich. Jednak nadal jest możliwe dla użytkownika w celu uruchamiania aplikacji dostępne na obrazie, uzyskiwanie do nich dostępu bezpośrednio w systemie operacyjnym.
  - Ta funkcja nie umożliwia bezpieczne blokowania aplikacji; jedynie ogranicza widoczności w aplikacji kanału.
  - Jeśli potrzebujesz wyodrębnienia użytkowników z aplikacji, będzie konieczne oddzielne kolekcje za pomocą tego.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Jak uzyskać poleceń cmdlet środowiska RemoteApp PowerShell Azure

Aby wypróbować nowe funkcje podglądu, należy użyć poleceń cmdlet programu PowerShell Azure. Obecnie nie jest możliwe, aby włączyć tryb publikowania nowej aplikacji za pomocą portalu zarządzania Azure.

Najpierw upewnij się, że masz zainstalowany [modułu programu PowerShell usługi Azure](../powershell-install-configure.md) .

Następnie uruchom konsolę programu PowerShell w trybie administratora i uruchom następujące polecenie cmdlet:

        Add-AzureAccount

Ją wyświetli monit o podanie nazwy Azure użytkownika i hasła. Po zalogowaniu się można uruchomić polecenia cmdlet Azure RemoteApp dla subskrypcji Azure.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Jak sprawdzić, które trybu a zbioru

Uruchom następujące polecenie cmdlet:

        Get-AzureRemoteAppCollection <collectionName>

![Zaznacz pole wyboru trybu zbioru](./media/remoteapp-perapp/araacllelvel.png)

Właściwość AclLevel może mieć następujące wartości:

- Kolekcja: oryginalny publikowania tryb. Wszyscy użytkownicy widzą wszystkie opublikowane aplikacje.
- Aplikacja: nowy publikowania tryb. Użytkownicy widzą tylko aplikacje opublikowany bezpośrednio do nich.

## <a name="how-to-switch-to-application-publishing-mode"></a>Jak przełączyć się do trybu publikowania aplikacji

Uruchom następujące polecenie cmdlet:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Stan publikowania aplikacji zostaną zachowane: początkowo wszyscy użytkownicy będą widoczne wszystkie oryginalny opublikowanych aplikacji.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Jak wyświetlić użytkowników, którzy mogą przeglądać określonej aplikacji

Uruchom następujące polecenie cmdlet:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Ta lista zawiera wszystkich użytkowników, którzy mogą przeglądać aplikacji.

Uwaga: Można wyświetlić aliasy aplikacji (nazywane "alias aplikacji" w powyższej składni), uruchamiając Get AzureRemoteAppProgram - nazwa_kolekcji <collectionName>.

## <a name="how-to-assign-an-application-to-a-user"></a>Jak przypisać aplikacji do użytkownika

Uruchom następujące polecenie cmdlet:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Użytkownik zostanie wyświetlona aplikacja w kliencie Azure RemoteApp i będzie mógł połączyć się z nim.

## <a name="how-to-remove-an-application-from-a-user"></a>Jak usunąć aplikację z użytkownikiem

Uruchom następujące polecenie cmdlet:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Wyrażenie opinii
Dziękujemy za na opinie i sugestie dotyczące tej funkcji podglądu. Wypełnij [ankietę](http://www.instant.ly/s/FDdrb) , aby poinformować nas o tym, co sądzisz.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Nie mieli możliwość wypróbować tę funkcję preview?
Jeśli użytkownik nie uczestniczyły w podglądzie jeszcze, użyj tej [ankiety](http://www.instant.ly/s/AY83p) żądania dostępu.
