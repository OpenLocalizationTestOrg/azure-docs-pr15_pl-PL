<properties 
    pageTitle="Uwierzytelnianie systemu Windows i Azure serwera uwierzytelnianie wieloskładnikowe"
    description="Jest to strona uwierzytelnianie wieloskładnikowe Azure, który pomoże wdrażanie serwera uwierzytelnianie wieloskładnikowe Azure i uwierzytelniania systemu Windows."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a>Uwierzytelnianie systemu Windows i Azure serwera uwierzytelnianie wieloskładnikowe

W sekcji uwierzytelnianie systemu Windows umożliwia administratorowi Włączanie i konfigurowanie uwierzytelniania systemu Windows dla jednego lub więcej aplikacji.  Poniżej przedstawiono listę rzeczy do zapamiętania przed konfigurowania uwierzytelniania systemu Windows.

-  Uruchom ponownie komputer jest potrzebne przed Azure uwierzytelnianie wieloskładnikowe usługi terminalowe będzie aktywna.
-  Jeśli jest zaznaczone pole wyboru "Wymagaj uwierzytelnianie wieloskładnikowe Azure użytkownika dopasowania" i nie są na liście użytkowników, nie można zalogować się do komputera po Uruchom ponownie komputer.
-  Adresy IP zaufanych jest uzależniony od czy umożliwiają IP klienta z uwierzytelnianiem aplikacji. Obecnie usługi terminalowe jest obsługiwana.  







>[AZURE.NOTE]Ta funkcja nie jest obsługiwana z bezpiecznego usługami terminalowymi w systemie Windows Server 2012 R2.




## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a>Aby zabezpieczyć aplikację z uwierzytelniania systemu Windows, wykonaj poniższą procedurę.

1. W polu Serwer uwierzytelnianie wieloskładnikowe Azure kliknij ikonę uwierzytelniania systemu Windows.
![Uwierzytelnianie systemu Windows](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Zaznacz pole wyboru uwierzytelniania systemu Windows włączyć. Domyślnie to pole jest zaznaczone.
3. Karta aplikacji umożliwia administratorem, aby skonfigurować jedną lub więcej aplikacji dla uwierzytelniania systemu Windows.
4. Wybierz aplikację serwera lub — określ, czy aplikacja jest włączona. Kliknij przycisk OK.
5. Kliknij przycisk Dodaj. przycisk.
6. Na karcie Zaufane adresy IP umożliwia pominąć pochodzących z określonych adresy IP sesji Azure wieloskładnikowe uwierzytelniania dla systemu Windows. Na przykład jeśli pracowników za pomocą aplikacji pakietu office, jak i w domu, może być zdecydujesz, że nie chcesz telefonach dzwonienie uwierzytelniania wieloskładnikowego Azure przy jednoczesnym pakietu office. W tym należy określić podsieci pakietu office jako pozycję Zaufane adresy IP.
7. Kliknij przycisk Dodaj. przycisk.
8. Zaznacz pojedynczy IP, jeśli chcesz przejść na jeden adres IP.
9. Zaznacz zakres adresów IP, jeśli chcesz pominąć cały zakres adresów IP. Przykład 10.63.193.1-10.63.193.100.
10. Wybierz pozycję podsieci, jeśli chcesz określić zakres z adresy IP przy użyciu notacji podsieci. Wprowadź początkowy IP danej podsieci, a następnie wybierz odpowiednie maski z listy rozwijanej.
11. Kliknij przycisk OK.
