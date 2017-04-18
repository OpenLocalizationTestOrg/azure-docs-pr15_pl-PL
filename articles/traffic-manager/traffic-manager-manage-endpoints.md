<properties
    pageTitle="Zarządzanie punkty końcowe w Menedżerze ruch Azure | Microsoft Azure"
    description="W tym artykule pomoże Ci Dodawanie, usuwanie, włączanie i wyłączanie punkty końcowe za pomocą Menedżera ruch Azure."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="add-disable-enable-or-delete-endpoints"></a>Dodawanie, wyłączanie, włączanie i usuwanie punktów końcowych

Funkcja aplikacji sieci Web w usłudze Azure aplikacji zawiera już awaryjnego i funkcji kierowania ruch okrężny witryn sieci Web w centrum danych, bez względu na sposób witryny sieci Web. Menedżer ruchu Azure umożliwia określenie awaryjnego i ruch okrężny rozsyłanie do witryny sieci Web i chmury usługi w różnych centrach danych. Najpierw należy zapewnić tej funkcji jest dodać punkt końcowy usługi lub witryny sieci Web chmury do Menedżera ruch.

>[AZURE.NOTE]  W tym artykule wyjaśniono, jak korzystać z portalu klasyczny. Klasyczny portalu Azure obsługuje tylko tworzenie i przypisywanie usług w chmurze i aplikacje sieci Web jako punkty końcowe. Nowy [Azure portal](https://portal.azure.com) to preferowany interfejs.

Można także wyłączyć poszczególnych punktów końcowych, należące do profilu Menedżer ruchu. Wyłączanie punktu końcowego opuszcza je jako część profilu, ale profil działa tak, jakby punkt końcowy nie znajduje się w nim. Ta akcja jest przydatne w przypadku tymczasowe usuwanie punktu końcowego, który znajduje się w trybie konserwacji lub rozmieszczany. Gdy punkt końcowy zostanie uruchomione ponownie, może być włączona.

>[AZURE.NOTE] Wyłączenie punktu końcowego ma nic, aby zrobić z stanu rozmieszczania platformy Azure. Punkt końcowy prawidłowy pozostaje w górę i może odbierać ruch nawet wtedy, gdy wyłączona w Menedżerze ruch. Ponadto wyłączenie punktu końcowego w profilu nie wpływa na jego stan w innym profilu.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Aby dodać punkt końcowy usługi lub witryny sieci Web chmury

1. W okienku Menedżer ruchu w portalu klasyczny Azure Zlokalizuj profil Menedżer ruchu, zawierający ustawienia punktu końcowego, które chcesz zmodyfikować. Aby otworzyć stronę ustawienia, kliknij strzałkę po prawej stronie nazwę profilu.
2. W górnej części strony kliknij pozycję **punkty końcowe** , aby wyświetlić punkty końcowe, które już są częścią konfiguracji systemu.
3. U dołu strony kliknij przycisk **Dodaj** , aby uzyskać dostęp do strony **Dodawanie punktów końcowych usługi** . Domyślnie na stronie Lista usług w chmurze w obszarze **Punkty końcowe usługi**.
4. Dla usług w chmurze wybierz z usługami w chmurze na liście, aby dodać je jako punkty końcowe dla tego profilu. Wyczyszczenie nazwa usługi cloud powoduje usunięcie go z listy punktów końcowych.
5. Witryn sieci Web kliknij listy rozwijanej **Typ usługi** , a następnie wybierz **aplikację sieci Web**.
6. Na liście, aby dodać je jako punkty końcowe dla tego profilu wybierz pozycję witryny sieci Web. Wyczyszczenie nazwę witryny sieci Web powoduje usunięcie go z listy punktów końcowych. Możesz wybrać tylko jedną witrynę sieci Web na Azure centrum danych (region). Po zaznaczeniu pierwszej witryny sieci Web innych witryn sieci Web w tym samym centrum danych stają się niedostępne w przypadku zaznaczenia. Należy również zauważyć, że są wyświetlane tylko standardowe witryny sieci Web.
7. Po wybraniu punkty końcowe dla tego profilu kliknij znacznik wyboru w prawym dolnym, aby zapisać zmiany.

>[AZURE.NOTE] Po dodaniu lub usunąć punktu końcowego z profilu przy użyciu metody routingu ruchu *pracy awaryjnej* liście priorytet pracy awaryjnej może nie być uporządkowane one, jak chcesz. Możesz dostosować kolejność na liście priorytet pracy awaryjnej na stronie Konfiguracja. Aby uzyskać więcej informacji zobacz [Konfigurowanie pracy awaryjnej routingu ruchu](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Aby wyłączyć punktu końcowego

1. W okienku Menedżer ruchu w portalu klasyczny Azure Zlokalizuj profil Menedżer ruchu, zawierający ustawienia punktu końcowego, które chcesz zmodyfikować. Aby otworzyć stronę ustawienia, kliknij strzałkę po prawej stronie nazwę profilu.
2. W górnej części strony kliknij pozycję **punkty końcowe** , aby wyświetlić punkty końcowe, które znajdują się w konfiguracji.
3. Kliknij punkt końcowy, który chcesz wyłączyć, a następnie kliknij **Wyłącz** u dołu strony.
4. Klienci nadal chcesz wysyłać ruch do punktu końcowego na czas trwania czas wygaśnięcia (TTL). Możesz zmienić wartość TTL na stronie Konfiguracja profilu Menedżera ruch.

## <a name="to-enable-an-endpoint"></a>Aby włączyć punktu końcowego

1. W okienku Menedżer ruchu w portalu klasyczny Azure Zlokalizuj profil Menedżer ruchu, zawierający ustawienia punktu końcowego, które chcesz zmodyfikować. Aby otworzyć stronę ustawienia, kliknij strzałkę po prawej stronie nazwę profilu.
2. W górnej części strony kliknij pozycję **punkty końcowe** , aby wyświetlić punkty końcowe, które znajdują się w konfiguracji.
3. Kliknij punkt końcowy, który chcesz włączyć, a następnie kliknij **Włącz** u dołu strony.
4. Klienci są kierowane do enabled punktu końcowego, zgodnie z ustawieniem profilu.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Aby usunąć punkt końcowy usługi lub witryny sieci Web chmury

1. W okienku Menedżer ruchu w portalu klasyczny Azure Zlokalizuj profil Menedżer ruchu, zawierający ustawienia punktu końcowego, które chcesz zmodyfikować. Aby otworzyć stronę ustawienia, kliknij strzałkę po prawej stronie nazwę profilu.
2. W górnej części strony kliknij pozycję **punkty końcowe** , aby wyświetlić punkty końcowe, które już są częścią konfiguracji systemu.
3. Na stronie punkty końcowe kliknij nazwę punktu końcowego, który chcesz usunąć z profilu.
4. U dołu strony kliknij przycisk **Usuń**.

## <a name="next-steps"></a>Następne kroki

* [Zarządzanie profilami Menedżer ruchu](traffic-manager-manage-profiles.md)
* [Konfigurowanie metod routingu](traffic-manager-configure-routing-method.md)
* [Stan obniżonej wydajności rozwiązywania problemów Menedżer ruchu](traffic-manager-troubleshooting-degraded.md)
* [Zagadnienia dotyczące wydajności Menedżer ruchu](traffic-manager-performance-considerations.md)
* [Operacje na Menedżer ruchu (REST interfejsu API informacje)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
