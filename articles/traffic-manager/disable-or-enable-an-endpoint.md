<properties
   pageTitle="Wyłączanie lub włączanie punkt końcowy Menedżer ruchu | Microsoft Azure"
   description="W tym artykule pomoże wyłączyć lub włączyć punktów końcowych profilu Menedżer ruchu."
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

# <a name="disable-or-enable-a-traffic-manager-endpoint"></a>Wyłączanie lub włączanie punkt końcowy Menedżer ruchu

Można także wyłączyć poszczególnych punktów końcowych, należące do profilu Menedżer ruchu. Punkty końcowe obejmują zarówno usług w chmurze, jak i witrynami sieci Web. Wyłączanie punktu końcowego opuszcza je jako część profilu, ale profil działa tak, jakby punkt końcowy nie znajduje się w nim. Ta akcja jest bardzo przydatne w przypadku tymczasowe usuwanie punktu końcowego, który znajduje się w trybie konserwacji lub rozmieszczany. Gdy punkt końcowy zostanie uruchomione ponownie, może być włączona

>[AZURE.NOTE] **Wyłączenie punktu końcowego ma nic, aby zrobić z stanu rozmieszczania platformy Azure. Punkt końcowy prawidłowy pozostanie w górę i może odbierać ruch nawet wtedy, gdy wyłączona w Menedżerze ruch. Ponadto wyłączenie punktu końcowego w profilu nie wpływa na jego stan w innym profilu.**

## <a name="to-disable-an-endpoint"></a>Aby wyłączyć punktu końcowego

1. W okienku Menedżer ruchu w portalu klasyczny Azure Zlokalizuj profil Menedżer ruchu, zawierający ustawienia punktu końcowego, które chcesz zmodyfikować, a następnie kliknij strzałkę na prawo od nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
1. W górnej części strony kliknij pozycję **punkty końcowe** , aby wyświetlić punkty końcowe, które znajdują się w konfiguracji.
1. Kliknij punkt końcowy, który chcesz wyłączyć, a następnie kliknij **Wyłącz** u dołu strony.
1. Ruch przestanie ułożony do punktu końcowego według DNS Time-to-Live (TTL) skonfigurowane dla nazwy domeny Menedżer ruchu. Na stronie Konfiguracja profilu Menedżera ruch można zmienić wartość TTL.

## <a name="to-enable-an-endpoint"></a>Aby włączyć punktu końcowego


1. W okienku Menedżer ruchu w portalu klasyczny Azure Zlokalizuj profil Menedżer ruchu, zawierający ustawienia punktu końcowego, które chcesz zmodyfikować, a następnie kliknij strzałkę na prawo od nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
1. W górnej części strony kliknij pozycję **punkty końcowe** , aby wyświetlić punkty końcowe, które znajdują się w konfiguracji.
1. Kliknij punkt końcowy, który chcesz włączyć, a następnie kliknij **Włącz** u dołu strony.
1. Ruch rozpocznie się ułożony z usługą ponownie jako podyktowanego w profilu.

## <a name="next-steps"></a>Następne kroki

[Ruch Menedżer — wyłączyć, Włącz lub usuwanie profilu](disable-enable-or-delete-a-profile.md)

[Stan obniżonej wydajności rozwiązywania problemów Menedżer ruchu](traffic-manager-troubleshooting-degraded.md)

[Zagadnienia dotyczące wydajności Menedżer ruchu](traffic-manager-performance-considerations.md)