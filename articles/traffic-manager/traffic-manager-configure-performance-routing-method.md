<properties
   pageTitle="Konfigurowanie metody routingu ruchu wydajności | Microsoft Azure"
   description="W tym artykule pomoże Ci skonfigurować metody routingu ruchu wydajności w Menedżerze ruchu"
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

# <a name="configure-performance-traffic-routing-method"></a>Konfigurowanie metody routingu ruchu wydajności

Aby skierować ruch dla usług w chmurze i witryn sieci Web (punkty końcowe) znajdują się w różnych centrach danych na całym świecie (nazywane także regiony), możesz wskazać ruch przychodzący do punktu końcowego o najmniejszej opóźnienie od żądającego klienta. Zazwyczaj centrum danych o najmniejszej opóźnienie odpowiada najbardziej w odległości geograficznych. Metoda routingu ruchu wydajności umożliwia rozpowszechnianie oparte na opóźnienie najniższych, ale nie można uwzględnić zmiany w czasie rzeczywistym w konfiguracji sieci lub załaduj. Aby uzyskać więcej informacji na temat metod routingu inny ruch, które Menedżer ruch Azure udostępnia zobacz [metody routingu ruchu ruch Manager — informacje](traffic-manager-routing-methods.md).

## <a name="route-traffic-based-on-lowest-latency-across-a-set-of-endpoints"></a>Ruch rozsyłania według najniższe opóźnienie w zestawie punkty końcowe:

1. W portalu klasyczny Azure, w okienku po lewej stronie kliknij ikonę **Menedżer ruchu** , aby otworzyć okienko Menedżer ruchu. Jeśli profil Menedżera ruch nie został jeszcze utworzony, zobacz [Zarządzanie profilami Menedżer ruchu](traffic-manager-manage-profiles.md) czynności aby utworzyć podstawowy profil Menedżera ruch.
2. W portalu klasyczny Azure, w okienku Menedżer ruchu Zlokalizuj profil Menedżer ruchu, zawierający ustawienia, które chcesz zmodyfikować, a następnie kliknij strzałkę na prawo od nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
3. Na stronie profilu kliknij **punkty końcowe** w górnej części strony i sprawdź, czy istnieją punkty końcowe usługi, które mają zostać uwzględnione w konfiguracji. Aby uzyskać instrukcje dodać lub usunąć punkty końcowe ze swojego profilu zobacz [Zarządzanie punkty końcowe w Menedżerze ruch](traffic-manager-endpoints.md).
4. Na stronie profilu kliknij przycisk **Konfiguruj** u góry ekranu, aby otworzyć stronę konfiguracji.
5. W przypadku **Ustawienia metody routingu ruchu**Zweryfikuj metody routingu ruchu * *wydajności*. Jeśli nie jest dostępne, kliknij przycisk * *wydajności** na liście rozwijanej.
6. Sprawdź, czy **Ustawienia monitorowania** są odpowiednio skonfigurowane. Monitorowania gwarantuje, że punkty końcowe, które są w trybie offline nie są wysyłane ruch. W celu monitorowania punktów końcowych, należy określić ścieżkę i nazwę pliku. Należy zauważyć, że dalej ukośnik "/" jest prawidłowym wpisem ścieżki względne i oznacza, że plik znajduje się w katalogu głównym (ustawienie domyślne). Aby uzyskać więcej informacji dotyczących monitorowania zobacz [Temat monitorowania Menedżer ruchu](traffic-manager-monitoring.md).
7. Po zakończeniu zmiany w konfiguracji, kliknij pozycję **Zapisz** u dołu strony.
8. Przetestuj zmiany w konfiguracji. Aby uzyskać więcej informacji zobacz [Sprawdzanie ustawienia Menedżer ruchu](traffic-manager-testing-settings.md).
9. Po instalacji i pracy profilu Menedżer ruchu edytowanie rekordu DNS na serwerze DNS autorytatywne wskaż nazwę domeny firmy Menedżer ruchu nazwy domeny. Aby uzyskać więcej informacji na temat wykonywania tego zadania zobacz [punkt domeny internetowej firmy w domenie menedżer ruchu](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Następne kroki


[Wskaż domenę Menedżer ruchu domeny internetowej firmy](traffic-manager-point-internet-domain.md)

[Metody routingu Menedżer ruchu](traffic-manager-routing-methods.md)

[Konfigurowanie routingu metody pracy awaryjnej](traffic-manager-configure-failover-routing-method.md)

[Konfigurowanie metody routingu okrężnego](traffic-manager-configure-round-robin-routing-method.md)

[Stan obniżonej wydajności rozwiązywania problemów Menedżer ruchu](traffic-manager-troubleshooting-degraded.md)

[Ruch Menedżer — wyłączyć, Włącz lub usuwanie profilu](disable-enable-or-delete-a-profile.md)

[Menedżer ruch — wyłączanie lub włączanie punktu końcowego](disable-or-enable-an-endpoint.md)

