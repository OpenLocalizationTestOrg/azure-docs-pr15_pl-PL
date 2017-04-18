<properties
    pageTitle="Nieprawidłowe działanie logowania"
    description="Raport, który zawiera Zaloguj dodatki, które zostały określone jako anomalous przez naszych maszynowego uczenia algorytmów."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="gchander"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="irregular-sign-in-activity"></a>Nieprawidłowe działanie logowania

O nieregularnym kształcie dodatki logowania to te, które zostały określone przez naszych maszynowego uczenia algorytmy, na podstawie warunku "niemożliwe podróży" połączone znakiem anomalous w lokalizacji i urządzenia. Może to oznaczać, że haker pomyślnie zalogował się przy użyciu tego konta.
Wyślemy powiadomienie e-mail do administratorów globalnych w razie wystąpienia możemy 10 lub więcej anomalous logowania zdarzeń zakresu 30 dni lub mniej. Pamiętaj uwzględnić aad-alerts-noreply@mail.windowsazure.com na liście bezpiecznych nadawców.
