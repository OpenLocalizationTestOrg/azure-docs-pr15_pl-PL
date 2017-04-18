<properties
   pageTitle="Aktualizowanie kolekcji Azure RemoteApp | Microsoft Azure"
   description="Dowiedz się, jak zaktualizować kolekcji Azure RemoteApp"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="update-a-collection-in-azure-remoteapp"></a>Aktualizowanie zbioru w Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Pochodzą osobno, usługa po konieczna jest aktualizacja aplikacji lub obrazu w zbiorze Azure RemoteApp. Jeśli używasz obrazów dołączonych do subskrypcji Azure RemoteApp w chmurze lub hybrydowymi połączeniami kolekcji, wszystkie aktualizacje są obsługiwane przez RemoteApp Azure, więc można umieścić łatwe.

Jednak jeśli używasz obraz niestandardowy (że wbudowany od podstaw lub utworzony przez zmodyfikowanie jednej z naszych obrazy), to odpowiedzialnych za obraz i aplikacje. Jeśli musisz zaktualizować obrazu lub na dowolnej stronie aplikacje w nim, musisz utworzyć nową, zaktualizowaną wersję obrazu, a następnie zastąpić istniejącego obrazu w zbiorze ten nowy zaktualizowanego obrazu.

Tak jak przejściu informacji na temat aktualizacji kolekcji? Jest bardzo prosta:

1. Aktualizowanie obrazu, który został użyty w kolekcji. Stosowanie wszystkie poprawki i aktualizacje wymagane, a następnie zapisz prezentację pod nową nazwą.
2. [Przekazywanie](remoteapp-uploadimage.md) lub [Importowanie](remoteapp-image-on-azurevm.md) którego obraz, aby RemoteApp.
3. Teraz na stronie pobierania kliknij przycisk **Aktualizuj**.
4. Na liście **Obraz szablonu** , wybierz pozycję Nowy obraz.
4. W tym miejscu jest wymagana — należy określić, jak radzić sobie z użytkownicy, którzy używają aplikacji w kolekcji. Masz następujące możliwości:
    - **Udzielanie 60 minut po aktualizacji**. Zaraz po zainstalowaniu aktualizacji Azure RemoteApp zostanie wyświetlony komunikat do wszystkich aktywnych użytkowników informacją do zapisania swojej pracy, wyloguj się i zaloguj się ponownie. Po 60 minut aktywni użytkownicy, którzy nie mają wylogować będą automatycznie rejestrowane. Użytkownicy mogą od razu zalogować ponownie.
    - **Od razu zalogować użytkowników**. Zaraz po zainstalowaniu aktualizacji Wyloguj wszystkich użytkowników automatycznie bez ostrzeżenia. Jeśli wybierzesz tę opcję, użytkownicy mogą utracić dane. Jednak można ponownym aplikacja natychmiast.

1. Kliknij znacznik wyboru, aby rozpocząć aktualizację.
