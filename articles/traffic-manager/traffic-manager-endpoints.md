<properties
   pageTitle="Zarządzanie punkty końcowe w Menedżerze ruch Azure | Microsoft Azure"
   description="W tym artykule pomoże Ci Dodawanie, usuwanie, włączanie i wyłączanie punkty końcowe za pomocą Menedżera ruch Azure."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="add-disable-enable-or-delete-endpoints"></a>Dodawanie, wyłączanie, włączanie i usuwanie punktów końcowych

Funkcja aplikacji sieci Web w usłudze Azure aplikacji zawiera już awaryjnego i funkcji kierowania ruch okrężny witryn sieci Web w centrum danych, bez względu na sposób witryny sieci Web. Menedżer ruchu Azure umożliwia określenie awaryjnego i ruch okrężny rozsyłanie do witryny sieci Web i chmury usługi w różnych centrach danych. Najpierw należy zapewnić tej funkcji jest dodać punkt końcowy usługi lub witryny sieci Web chmury do Menedżera ruch.

>[AZURE.NOTE] Nie można dodać lokalizacji zewnętrznych lub profile Menedżera ruch jako punkty końcowe przy użyciu portalu klasyczny Azure. Należy użyć interfejsu API usługi REST [Tworzenie definicji](http://go.microsoft.com/fwlink/p/?LinkId=400772) lub środowiska Windows PowerShell [AzureTrafficManagerEndpoint Dodaj](http://go.microsoft.com/fwlink/p/?LinkId=400774).

Można także wyłączyć poszczególnych punktów końcowych, należące do profilu Menedżer ruchu. Punkty końcowe obejmują zarówno usług w chmurze, jak i witrynami sieci Web. Wyłączanie punktu końcowego opuszcza je jako część profilu, ale profil działa tak, jakby punkt końcowy nie znajduje się w nim. Ta akcja jest bardzo przydatne w przypadku tymczasowe usuwanie punktu końcowego, który znajduje się w trybie konserwacji lub rozmieszczany. Gdy punkt końcowy zostanie uruchomione ponownie, może być włączona.

>[AZURE.NOTE] Wyłączenie punktu końcowego ma nic, aby zrobić z stanu rozmieszczania platformy Azure. Punkt końcowy prawidłowy pozostanie w górę i może odbierać ruch nawet wtedy, gdy wyłączona w Menedżerze ruch. Ponadto wyłączenie punktu końcowego w profilu nie wpływa na jego stan w innym profilu.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Aby dodać punkt końcowy usługi lub witryny sieci Web chmury


1. W okienku Menedżer ruchu w portalu klasyczny Azure Zlokalizuj profil Menedżer ruchu, zawierający ustawienia punktu końcowego, które chcesz zmodyfikować, a następnie kliknij strzałkę na prawo od nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
2. W górnej części strony kliknij pozycję **punkty końcowe** , aby wyświetlić punkty końcowe, które już są częścią konfiguracji systemu.
3. U dołu strony kliknij przycisk **Dodaj** , aby uzyskać dostęp do strony **Dodawanie punktów końcowych usługi** . Domyślnie na stronie Lista usług w chmurze w obszarze **Punkty końcowe usługi**.
4. Dla usług w chmurze wybierz z usługami w chmurze na liście, aby umożliwić ich jako punkty końcowe dla tego profilu. Wyczyszczenie nazwa usługi cloud powoduje usunięcie go z listy punktów końcowych.
5. Witryn sieci Web kliknij listy rozwijanej **Typ usługi** , a następnie wybierz **aplikację sieci Web**.
6. Na liście, aby dodać je jako punkty końcowe dla tego profilu wybierz pozycję witryny sieci Web. Wyczyszczenie nazwę witryny sieci Web powoduje usunięcie go z listy punktów końcowych. Zauważ, że możesz wybrać tylko jednej witryny sieci Web na Azure centrum danych (region). Jeśli wybierzesz witrynę sieci Web w centrum danych, obsługującym wielu witryn sieci Web po zaznaczeniu pierwszej witryny sieci Web, inne osoby, w tym samym centrum danych stają się niedostępne w przypadku zaznaczenia. Należy również zauważyć, że są wyświetlane tylko standardowe witryny sieci Web.
7. Po wybraniu punkty końcowe dla tego profilu kliknij znacznik wyboru w prawym dolnym, aby zapisać zmiany.

>[AZURE.NOTE] Jeśli korzystasz z metody routingu ruchu *pracy awaryjnej* po Dodawanie lub usuwanie punktu końcowego, pamiętaj dostosować liście priorytet pracy awaryjnej na stronie Konfiguracja, aby odzwierciedlała kolejności pracy awaryjnej odpowiedni dla danej konfiguracji. Aby uzyskać więcej informacji zobacz [Konfigurowanie pracy awaryjnej routingu ruchu](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Aby wyłączyć punktu końcowego

1. W okienku Menedżer ruchu w portalu klasyczny Azure zlokalizować profilu Menedżer ruchu, który zawiera ustawienia punktu końcowego, które chcesz zmodyfikować, a następnie kliknij strzałkę na prawo od nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
2. W górnej części strony kliknij pozycję **punkty końcowe** , aby wyświetlić punkty końcowe, które znajdują się w konfiguracji.
3. Kliknij punkt końcowy, który chcesz wyłączyć, a następnie kliknij **Wyłącz** u dołu strony.
4. Ruch przestanie ułożony do punktu końcowego według DNS Time-to-Live (TTL) skonfigurowane dla nazwy domeny Menedżer ruchu. Na stronie Konfiguracja profilu Menedżera ruch można zmienić wartość TTL.

## <a name="to-enable-an-endpoint"></a>Aby włączyć punktu końcowego

1. W okienku Menedżer ruchu w portalu klasyczny Azure Zlokalizuj profil Menedżer ruchu, zawierający ustawienia punktu końcowego, które chcesz zmodyfikować, a następnie kliknij strzałkę na prawo od nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
2. W górnej części strony kliknij pozycję **punkty końcowe** , aby wyświetlić punkty końcowe, które znajdują się w konfiguracji.
3. Kliknij punkt końcowy, który chcesz włączyć, a następnie kliknij **Włącz** u dołu strony.
4. Ruch rozpocznie się ułożony z usługą ponownie jako podyktowanego w profilu.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Aby usunąć punkt końcowy usługi lub witryny sieci Web chmury


1. W okienku Menedżer ruchu w portalu klasyczny Azure zlokalizować profilu Menedżer ruchu, który zawiera ustawienia punktu końcowego, które chcesz zmodyfikować, a następnie kliknij strzałkę na prawo od nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
2. W górnej części strony kliknij pozycję **punkty końcowe** , aby wyświetlić punkty końcowe, które już są częścią konfiguracji systemu.
3. Na stronie punkty końcowe kliknij nazwę punktu końcowego, który chcesz usunąć z profilu.
4. U dołu strony kliknij przycisk **Usuń**.

>[AZURE.NOTE] Nie można usunąć lokalizacji zewnętrznych lub profile Menedżera ruch jako punkty końcowe przy użyciu portalu klasyczny Azure. Należy użyć programu Windows PowerShell. Aby uzyskać więcej informacji zobacz [Usuwanie AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx).

## <a name="next-steps"></a>Następne kroki

- [Konfigurowanie routingu metody pracy awaryjnej](traffic-manager-configure-failover-routing-method.md)
- [Konfigurowanie metody routingu okrężnego](traffic-manager-configure-round-robin-routing-method.md)
- [Konfigurowanie metody routingu wydajności](traffic-manager-configure-performance-routing-method.md)
- [Stan obniżonej wydajności rozwiązywania problemów Menedżer ruchu](traffic-manager-troubleshooting-degraded.md)
- [Operacje na Menedżer ruchu (REST interfejsu API informacje)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
