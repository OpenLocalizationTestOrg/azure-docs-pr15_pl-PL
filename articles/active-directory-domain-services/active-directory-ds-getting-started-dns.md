<properties
    pageTitle="Azure usługach domenowych AD: Ustawienia aktualizacji DNS Azure wirtualną sieć | Microsoft Azure"
    description="Wprowadzenie do usług domenowych Active Directory platformy Azure"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Azure usługach domenowych AD - ustawienia DNS aktualizacji dla Azure wirtualnej sieci

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Zadanie 4: Aktualizowanie ustawień DNS pod kątem Azure wirtualnej sieci
W poprzednich zadań konfiguracyjnych, pomyślnie włączono usługi Azure AD domeny dla usługi katalogowej. Następnego zadania jest zapewnienie, że komputery w wirtualnej sieci można łączyć i korzystanie z tych usług. Zaktualizuj ustawienia serwera DNS dla wirtualnych sieci, aby wskazywały dwa adresy IP, w których jest dostępna w wirtualnej sieci Azure usług domenowych AD.

> [AZURE.NOTE] Zanotuj adresów IP dla usługi Azure AD domeny wyświetlane na karcie **Konfigurowanie** katalogu, po włączeniu usługi Azure AD domena katalogu.

Wykonaj poniższe kroki konfiguracji, aby zaktualizować ustawienia serwera DNS dla wirtualnej sieci, w której są włączone usługi Azure AD domeny.

1. Przejdź do **portalu klasyczny Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Wybierz węzeł **sieci** w okienku po lewej stronie.

    ![Węzeł wirtualnych sieci](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. Na karcie **Wirtualnych sieci** wybierz wirtualnej sieci, w której są włączone usługi Azure AD domeny wyświetlić jego właściwości.

4. Kliknij kartę **Konfiguruj** .

    ![Węzeł wirtualnych sieci](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. W sekcji **serwerów DNS** wprowadź adresy IP usługi Azure AD domeny.

6. Upewnij się, wprowadzonego zarówno adresy IP, które były wyświetlane w sekcji **Domain Services** na karcie **Konfigurowanie** katalogu.

7. Aby zapisać ustawienia serwera DNS dla tej wirtualnej sieci, w okienku zadań u dołu strony kliknij przycisk **Zapisz** .

   ![Zaktualizuj ustawienia serwera DNS dla wirtualnej sieci.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Po zaktualizowaniu ustawienia serwera DNS dla wirtualnej sieci, może upłynąć trochę czasu, zanim maszyn wirtualnych w sieci, aby uzyskać zaktualizowane konfiguracji DNS. Jeśli maszyna wirtualna nie może połączyć się z domeną, możesz Opróżnij pamięć podręczną DNS (np. "ipconfig/flushdns") na komputerze wirtualnych. To polecenie wymusza odświeżenie ustawienia DNS na komputerze wirtualnych.


## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>Zadanie 5 - Włącz synchronizację haseł do usług domenowych AD Azure
Następnego zadania konfiguracji jest umożliwienie [synchronizacji haseł do usług domenowych AD Azure](active-directory-ds-getting-started-password-sync.md).
