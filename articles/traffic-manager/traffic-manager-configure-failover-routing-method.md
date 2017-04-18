<properties
   pageTitle="Konfigurowanie metody routingu ruchu pracy awaryjnej Menedżer ruchu | Microsoft Azure"
   description="W tym artykule pomoże Ci skonfigurować metody routingu ruchu pracy awaryjnej w Menedżerze ruchu"
   services="traffic-manager"
   documentationCenter=""
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

# <a name="configure-failover-routing-method"></a>Konfigurowanie routingu metody pracy awaryjnej

Często organizacji chce udostępnić niezawodności dla swoich usług. Jest to, dostarczając usługi tworzenia kopii zapasowych w przypadku awarii usługi podstawowego. Typowe deseń do przełączania awaryjnego usługi jest udostępnia zestaw identyczne usług i wysyłać ruch do podstawowej usługi przy zachowaniu skonfigurowanej listy jedną lub więcej kopii zapasowej usług. Ten typ kopii zapasowej można skonfigurować z usługami w chmurze Azure i witrynami sieci Web, wykonując poniższe procedury.

Należy zauważyć, że Azure witryn sieci Web już zawiera ruch pracy awaryjnej routingu metody funkcji witryn sieci Web w centrum danych (nazywane także regionu), bez względu na sposób witryny sieci Web. Menedżer ruchu umożliwia określenie metody routingu ruchu pracy awaryjnej witryn sieci Web w różnych centrach danych.

## <a name="to-configure-failover-traffic-routing-method"></a>Aby skonfigurować metody routingu ruchu pracy awaryjnej:

1. W portalu klasyczny Azure, w okienku po lewej stronie kliknij ikonę **Menedżer ruchu** , aby otworzyć okienko Menedżer ruchu. Jeśli profil Menedżera ruch nie został jeszcze utworzony, zobacz [Zarządzanie profilami Menedżer ruchu](traffic-manager-manage-profiles.md) czynności aby utworzyć podstawowy profil Menedżera ruch.
2. W okienku Menedżer ruchu w portalu klasyczny Azure Zlokalizuj profil Menedżer ruchu, zawierający ustawienia, które chcesz zmodyfikować, a następnie kliknij strzałkę na prawo od nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
3. Na stronie profilu kliknij **punkty końcowe** w górnej części strony i sprawdź, czy oba usług w chmurze i witryn (punkty końcowe), które mają zostać uwzględnione w konfiguracji. Instrukcje dotyczące dodawania lub usuwania punktów końcowych zobacz [Zarządzanie punkty końcowe w Menedżerze ruch](traffic-manager-endpoints.md).
4. Na stronie profilu kliknij przycisk **Konfiguruj** u góry ekranu, aby otworzyć stronę konfiguracji.
5. W przypadku **Ustawienia metody routingu ruchu**Zweryfikuj metody routingu ruchu **pracy awaryjnej**. Jeśli nie jest dostępne, kliknij pozycję **pracy awaryjnej** z listy rozwijanej.
6. Aby **Liście priorytet pracy awaryjnej**można uporządkować pracy awaryjnej dla punktów końcowych. Po wybraniu metody routingu ruchu **pracy awaryjnej** ma znaczenie kolejność zaznaczonych punktów końcowych. Punkt końcowy podstawowego znajduje się na wierzchu. W górę i w dół strzałki w celu zmiany kolejności, zgodnie z potrzebami. Aby uzyskać informacje o konfigurowaniu priorytet przejęcia awaryjnego przy użyciu programu Windows PowerShell, zobacz [Ustawianie AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Sprawdź, czy **Ustawienia monitorowania** są odpowiednio skonfigurowane. Monitorowania gwarantuje, że punkty końcowe, które są w trybie offline nie są wysyłane ruch. W celu monitorowania punktów końcowych, należy określić ścieżkę i nazwę pliku. Należy zauważyć, że dalej ukośnik "/" jest prawidłowym wpisem ścieżki względne i oznacza, że plik znajduje się w katalogu głównym (ustawienie domyślne). Aby uzyskać więcej informacji dotyczących monitorowania zobacz [Monitorowanie Menedżer ruchu](traffic-manager-monitoring.md).
8. Po zakończeniu zmiany w konfiguracji, kliknij pozycję **Zapisz** u dołu strony.
9. Przetestuj zmiany w konfiguracji. Aby uzyskać więcej informacji, zobacz sekcję [Testowanie ustawienia Menedżer ruchu](traffic-manager-testing-settings.md) .
10. Po instalacji i pracy profilu Menedżer ruchu edytowanie rekordu DNS na serwerze DNS autorytatywne wskaż nazwę domeny firmy Menedżer ruchu nazwy domeny. Aby uzyskać więcej informacji na temat wykonywania tego zadania zobacz [punkt domeny internetowej firmy w domenie menedżer ruchu](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Następne kroki

[Wskaż domenę Menedżer ruchu domeny internetowej firmy](traffic-manager-point-internet-domain.md)

[Metody routingu Menedżer ruchu](traffic-manager-routing-methods.md)

[Konfigurowanie metody routingu okrężnego](traffic-manager-configure-round-robin-routing-method.md)

[Konfigurowanie metody routingu wydajności](traffic-manager-configure-performance-routing-method.md)

[Stan obniżonej wydajności rozwiązywania problemów Menedżer ruchu](traffic-manager-troubleshooting-degraded.md)

[Ruch Menedżer — wyłączyć, Włącz lub usuwanie profilu](disable-enable-or-delete-a-profile.md)

[Menedżer ruch — wyłączanie lub włączanie punktu końcowego](disable-or-enable-an-endpoint.md)

