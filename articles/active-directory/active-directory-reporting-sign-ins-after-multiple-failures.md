<properties
    pageTitle="Logowanie dodatki po wielu błędów"
    description="Raport, który wskazuje użytkowników, którzy mają Zalogowano pomyślnie po wielu następujących po sobie Niepowodzenie logowania w prób."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-after-multiple-failures"></a>Dodatki logowania po wielu błędów
Ten raport wskazuje użytkowników, którzy mają Zalogowano pomyślnie po wielu następujących po sobie Niepowodzenie logowania w prób. Możliwe przyczyny są:

- Użytkownik miał zapomniane hasło</li><li>Użytkownik znajduje się ofiarą pomyślnego hasła odgadnięcie atakami wymusić atakiem

Wyniki z tego raportu zostanie wyświetlona liczbę następujących po sobie logowania niepomyślne wprowadzone przed pomyślnego logowania i sygnatura czasowa skojarzone z pierwszym pomyślnego logowania.

**Ustawienia raportu**: w prób, które musi przypadać przed mogą być wyświetlane w raporcie można skonfigurować minimalną liczbę następujących po sobie logowania nie powiodło się. Po wprowadzeniu zmian tego ustawienia jest należy zauważyć, że te zmiany nie zostanie zastosowany do wszelkich istniejących logowania nie powiodło się dodatki które obecnie są wyświetlane w istniejącym raporcie. Jednak będą one stosowane do wszystkich przyszłych rejestrowaniu. Zmiany do tego raportu mogą zostać wprowadzone tylko przez administratorów licencjonowana.


![Logowanie dodatki po wielu błędów](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)
