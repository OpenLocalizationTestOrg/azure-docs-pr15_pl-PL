<properties
    pageTitle="Zarządzanie profilami Menedżer ruchu Azure | Microsoft Azure"
    description="Ten artykuł ułatwia tworzenie, wyłączanie, włączanie, usuwanie i wyświetlanie historii profilu Menedżera ruch Azure."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="manage-an-azure-traffic-manager-profile"></a>Zarządzanie profilu Menedżer ruchu Azure

Profile Menedżera ruch metodami routingu ruchu do kontrolowania dystrybucji ruch do usług w chmurze lub punkty końcowe witryny sieci Web. W tym artykule wyjaśniono, jak tworzyć i zarządzać nimi te profile.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Tworzenie profilu Menedżer ruchu za pomocą szybkiego tworzenia

Można szybko utworzyć profilu Menedżer ruchu za pomocą szybkiego tworzenia w portalu klasyczny Azure. Szybkie tworzenie umożliwia tworzenie profilów przy użyciu ustawień konfiguracji podstawowej. Nie można jednak używać szybkie tworzenie dla ustawień, takich jak zestaw punkty końcowe (usług w chmurze i witrynami sieci Web), aby pracy awaryjnej metody routingu ruchu pracy awaryjnej lub ustawienia monitorowania. Po utworzeniu profilu, możesz skonfigurować następujące ustawienia w portalu klasyczny Azure. Menedżer ruchu obsługuje maksymalnie 200 punkty końcowe dla profilu. Jednak większość scenariuszy zastosowania wymaga tylko kilku punktów końcowych.

### <a name="to-create-a-traffic-manager-profile"></a>Aby utworzyć profil Menedżer ruchu

1. **Wdrażanie usługi usług w chmurze i witrynami sieci Web w środowisku produkcyjnym.** Aby uzyskać więcej informacji na temat usług w chmurze zobacz [Usług w chmurze](http://go.microsoft.com/fwlink/p/?LinkId=314074). Aby uzyskać więcej informacji na temat witryn sieci Web zobacz [witryn sieci Web](http://go.microsoft.com/fwlink/p/?LinkId=393327).

2. **Zaloguj się do portalu klasyczny Azure.** Kliknij przycisk **Nowy** w lewym dolnym rogu portalu, kliknij przycisk **usług sieciowych > Menedżer ruchu**, a następnie kliknij przycisk **Szybkie tworzenie** , aby rozpocząć konfigurowanie profilu.
3. **Konfigurowanie prefiks DNS.** Nadaj profil Menedżera ruch unikatowe DNS Nazwa prefiksu. Można określić prefiks dla nazwy domeny Menedżer ruchu.
4. **Wybierz subskrypcję.** Wybierz odpowiednie subskrypcję Azure. Każdy profil jest skojarzony z jednym subskrypcji. Jeśli masz tylko jedną subskrypcję, ta opcja jest niedostępna.
5. **Wybierz metodę routingu ruchu.** Wybierz metodę routingu ruchu w **ruchu routingu zasad**. Aby uzyskać więcej informacji na temat metod routingu ruchu zobacz [metody routingu ruchu o Menedżer ruchu](traffic-manager-routing-methods.md).
6. **Kliknij "Utwórz", aby utworzyć profil**. Po zakończeniu konfiguracji profilu możesz znaleźć swój profil w okienku Menedżer ruchu w portalu klasyczny Azure.
7. **Konfigurowanie punktów końcowych, monitorowanie i dodatkowe ustawienia w portalu klasyczny Azure.** Tylko za pomocą szybkiego tworzenia konfiguruje podstawowe ustawienia. Jest to konieczne skonfigurować dodatkowe ustawienia, takie jak lista punkty końcowe i kolejności pracy awaryjnej punktu końcowego.


## <a name="disable-enable-or-delete-a-profile"></a>Wyłączanie, włączanie i usuwanie profilu

Można wyłączyć istniejącego profilu, tak aby Menedżer ruch nie odwołuje się żądania użytkowników do skonfigurowanych punktów końcowych. Po wyłączeniu profilu Menedżera ruch profil i informacje zawarte w profilu pozostaną niezmienione i może być edytowany w interfejsie Menedżera ruch.  Odwołania Wznów po ponownym włączeniu profilu. Po utworzeniu profilu Menedżera ruchu w portalu klasyczny Azure jest automatycznie włączana. Jeśli zdecydujesz, że profil, który nie jest już potrzebna, możesz go usunąć.

### <a name="to-disable-a-profile"></a>Aby wyłączyć profilu

1. Jeśli korzystasz z niestandardowej nazwy domeny, należy zmienić rekord CNAME na serwerze DNS w sieci Internet, które nie są już wskazuje do swojego profilu Menedżer ruchu.
2. Ruch przestaje, są kierowane do punktów końcowych za pomocą ustawień profilu Menedżer ruchu.
3. Wybierz profil, który chcesz wyłączyć. Na stronie Menedżera ruch Wyróżnij profilu, klikając pozycję w kolumnie obok nazwę profilu. Notatkę, klikając jej nazwę profilu lub strzałkę obok nazwy zostanie wyświetlona na stronie Ustawienia profilu.
4. Po wybraniu profilu, kliknij przycisk **Wyłącz** w dolnej części strony.

### <a name="to-enable-a-profile"></a>Aby włączyć profil

1. Wybierz profil, który chcesz wyłączyć. Na stronie Menedżera ruch Wyróżnij profilu, klikając pozycję w kolumnie obok nazwę profilu. Notatkę, klikając jej nazwę profilu lub strzałkę obok nazwy zostanie wyświetlona na stronie Ustawienia profilu.
2. Po wybraniu profilu, kliknij pozycję **Włącz** w dolnej części strony.
3. Jeśli korzystasz z niestandardowej nazwy domeny, należy utworzyć rekordu CNAME na serwerze DNS w sieci Internet, wskaż nazwę domeny swojego profilu Menedżer ruchu.
4. Ruch jest ponownie przekierowywany do punktów końcowych.

### <a name="to-delete-a-profile"></a>Aby usunąć profil

1. Należy się upewnić, że rekord zasobu DNS na serwerze DNS w sieci Internet nie jest już używa rekord zasobu CNAME, która kieruje do nazwy domeny profilu Menedżer ruchu.
2. Wybierz profil, który chcesz wyłączyć. Na stronie Menedżera ruch Wyróżnij profilu, klikając pozycję w kolumnie obok nazwę profilu. Notatkę, klikając jej nazwę profilu lub strzałkę obok nazwy zostanie wyświetlona na stronie Ustawienia profilu.
3. Po wybraniu profilu, kliknij przycisk **Usuń** w dolnej części strony.

## <a name="view-traffic-manager-profile-change-history"></a>Historia zmian profilu Menedżer ruchu widoku

Możesz wyświetlać historii zmian dla swojego profilu Menedżer ruchu w portalu klasyczny Azure w usługach zarządzania.

### <a name="to-view-your-traffic-manager-change-history"></a>Aby wyświetlić historię zmian Menedżera ruchu

1. W okienku po lewej stronie portalu klasyczny Azure kliknij **Usługi zarządzania**.
2. Na stronie usługi zarządzania kliknij pozycję **Dzienniki operacji**.
3. Na stronie dzienniki operacji można filtrować wyświetlanie historii zmian dla swojego profilu Menedżer ruchu. Po wybraniu opcji filtrowania, kliknij znacznik wyboru, aby wyświetlić wyniki.

   - Aby wyświetlić zmiany dla wszystkich profilów, wybierz subskrypcję i przedział czasu, a następnie wybierz pozycję **Menedżer ruchu** z menu skrótów **typu** .
   - Aby filtrować według nazwa profilu, wpisz nazwę profilu w polu **Nazwa usługi** lub wybierz ją z menu skrótów.
   - Aby wyświetlić szczegóły dla poszczególnych zmian, zaznacz wiersz z zmianę, którą chcesz wyświetlić, a następnie kliknij **Szczegóły** w dolnej części strony. W okienku **Szczegółów operacji** możesz wyświetlić reprezentacji XML obiekt interfejs API, który został utworzony lub zaktualizowany w ramach operacji.

## <a name="next-steps"></a>Następne kroki

- [Dodawanie punktu końcowego](traffic-manager-endpoints.md)
- [Konfigurowanie routingu metody pracy awaryjnej](traffic-manager-configure-failover-routing-method.md)
- [Konfigurowanie metody routingu okrężnego](traffic-manager-configure-round-robin-routing-method.md)
- [Konfigurowanie metody routingu wydajności](traffic-manager-configure-performance-routing-method.md)
- [Wskaż nazwę domeny Menedżer ruchu domeny internetowej firmy](traffic-manager-point-internet-domain.md)
- [Stan obniżonej wydajności rozwiązywania problemów Menedżer ruchu](traffic-manager-troubleshooting-degraded.md)