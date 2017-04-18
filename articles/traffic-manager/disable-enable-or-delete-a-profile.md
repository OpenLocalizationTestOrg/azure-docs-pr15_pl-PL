<properties
   pageTitle="Wyłączanie, włączanie i usuwanie profilu Menedżer ruchu | Microsoft Azure"
   description="W tym artykule mogą pomóc w pracy z profili Menedżer ruchu."
   services="traffic-manager"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="disable-enable-or-delete-a-profile"></a>Wyłączanie, włączanie i usuwanie profilu


Można wyłączyć istniejącego profilu Menedżer ruchu, tak aby nie odniosą żądania użytkownika do jego skonfigurowanych punktów końcowych. Po wyłączeniu profilu Menedżer ruchu, sam profil i informacje zawarte w profilu pozostaną niezmienione i może być edytowany w interfejsie Menedżera ruch. Jeśli chcesz ponownie włączyć profilu, można w prosty sposób aby platformy Azure zostanie ponownie portal i odwołań. Po utworzeniu profilu Menedżera ruchu w Azure portal jest automatycznie włączana.

## <a name="to-disable-a-profile"></a>Aby wyłączyć profilu

1. Modyfikowanie rekordu zasobu DNS na serwerze DNS w sieci Internet, aby używać typu odpowiedniego rekordu i wskaźnik myszy na inną nazwę lub adres IP określonej lokalizacji w Internecie. Innymi słowy należy zmienić rekord zasobu DNS na serwerze DNS w sieci Internet, tak aby nie są już używane rekord zasobu CNAME, która kieruje do nazwy domeny profilu Menedżer ruchu.
1. Ruch przestanie, są kierowane do punktów końcowych za pomocą ustawień profilu Menedżer ruchu.
1. Wybierz profil, który chcesz wyłączyć. Wybranie profilu, na stronie Menedżera ruch Wyróżnij profilu, klikając pozycję w kolumnie obok nazwę profilu. Nie klikaj nazwę profilu lub strzałkę znajdującą się obok nazwy, jak to spowoduje przejście do strony ustawień profilu.
1. Po wybraniu profilu, kliknij przycisk Wyłącz w dolnej części strony.

## <a name="to-enable-a-profile"></a>Aby włączyć profil

1. Wybierz profil, który chcesz włączyć. Wybranie profilu, na stronie Menedżera ruch Wyróżnij profilu, klikając pozycję w kolumnie obok nazwę profilu. Nie klikaj nazwę profilu lub strzałkę znajdującą się obok nazwy, jak to spowoduje przejście do strony ustawień profilu.
1. Po wybraniu profilu, kliknij przycisk Włącz u dołu strony.
1. Modyfikowanie rekordu zasobu DNS na serwerze DNS w sieci Internet, aby użyć typu rekordu CNAME zamapuje nazwę domeny firmy na nazwę domeny swojego profilu Menedżer ruchu. Aby uzyskać więcej informacji zobacz [punkt domeny internetowej firmy w domenie menedżer ruchu](traffic-manager-point-internet-domain.md).
1. Ruch rozpocznie się zostanie ponownie skierowany do punktów końcowych.

## <a name="delete-a-profile"></a>Usuwanie profilu


1. Należy się upewnić, że rekord zasobu DNS na serwerze DNS w sieci Internet nie jest już używa rekord zasobu CNAME, która kieruje do nazwy domeny profilu Menedżer ruchu.
1. Wybierz profil, który chcesz usunąć. Wybranie profilu, na stronie Menedżera ruch wyróżnianie profilu
1. Kliknięcie w kolumnie obok profilu. Nie klikaj nazwę profilu lub strzałkę znajdującą się obok nazwy, jak to spowoduje przejście do strony ustawień profilu.
1. Po wybraniu profilu, kliknij przycisk Usuń w dolnej części strony.

## <a name="next-steps"></a>Następne kroki

[Menedżer ruch — wyłączanie lub włączanie punktu końcowego](disable-or-enable-an-endpoint.md)

[Konfigurowanie routingu metody pracy awaryjnej](traffic-manager-configure-failover-routing-method.md)

[Konfigurowanie metody routingu okrężnego](traffic-manager-configure-round-robin-routing-method.md)

[Konfigurowanie metody routingu wydajności](traffic-manager-configure-performance-routing-method.md)

[Stan obniżonej wydajności rozwiązywania problemów Menedżer ruchu](traffic-manager-troubleshooting-degraded.md)

