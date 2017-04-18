<properties
   pageTitle="Konfigurowanie Menedżer ruchu zaokrąglanie metody routingu ruchu robin | Microsoft Azure"
   description="W tym artykule pomoże Ci skonfigurować okrężnego równoważenia obciążenia dla punktów końcowych Menedżer ruchu."
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

# <a name="configure-round-robin-routing-method"></a>Konfigurowanie okrężnego metody routingu

Deseń typowe metody routingu ruchu jest zawierają zestaw identyczne punkty końcowe, które obejmują usług w chmurze i witrynami sieci Web, i przesyłać dane na każdej z nich w sposób okrężny. Poniższe kroki w konspekcie, jak skonfigurować Menedżer ruchu w celu wykonania tego typu metody routingu ruchu. Aby uzyskać więcej informacji na temat metod routingu ruch innego zobacz [metody routingu ruchu o Menedżer ruchu](traffic-manager-routing-methods.md).

>[AZURE.NOTE] Azure witryn sieci Web zawiera już równoważenia funkcji witryn sieci Web w centrum danych (region) obciążenia okrężny. Menedżer ruchu umożliwia określenie metody routingu ruch okrężny witryn sieci Web w różnych centrach danych.

## <a name="routing-traffic-equally-round-robin-across-a-set-of-endpoints"></a>Routing ruchu jednakowo (round robin) przez zbiór punkty końcowe:

1. W portalu klasyczny Azure, w okienku po lewej stronie kliknij ikonę **Menedżer ruchu** , aby otworzyć okienko Menedżer ruchu. Jeśli profil Menedżera ruch nie został jeszcze utworzony, zobacz [Zarządzanie profilami Menedżer ruchu](traffic-manager-manage-profiles.md) czynności aby utworzyć podstawowy profil Menedżera ruch.
2. W portalu klasyczny Azure, w okienku Menedżer ruchu Zlokalizuj profil Menedżer ruchu, zawierający ustawienia, które chcesz zmodyfikować, a następnie kliknij strzałkę na prawo od nazwy profilu. Spowoduje to otwarcie strony ustawień profilu.
3. Na stronie profilu kliknij **punkty końcowe** w górnej części strony i sprawdź, czy istnieją punkty końcowe usługi, które mają zostać uwzględnione w konfiguracji. Instrukcje dotyczące dodawania lub usuwania punktów końcowych zobacz [Zarządzanie punkty końcowe w Menedżerze ruch](traffic-manager-endpoints.md).
4. Na stronie profilu kliknij przycisk **Konfiguruj** u góry ekranu, aby otworzyć stronę konfiguracji.
5. **Metoda routingu ruchu ustawienia**upewnij się, że używana jest metoda routingu ruchu **okrężnego**. Jeśli nie jest dostępne, kliknij pozycję **okrężnego** na liście rozwijanej.
6. Sprawdź, czy **Ustawienia monitorowania** są odpowiednio skonfigurowane. Monitorowania gwarantuje, że punkty końcowe, które są w trybie offline nie są wysyłane ruch. W celu monitorowania punktów końcowych, należy określić ścieżkę i nazwę pliku. Należy zauważyć, że dalej ukośnik "/" jest prawidłowym wpisem ścieżki względne i oznacza, że plik znajduje się w katalogu głównym (ustawienie domyślne). Aby uzyskać więcej informacji dotyczących monitorowania zobacz [Temat monitorowania Menedżer ruchu](traffic-manager-monitoring.md).
7. Po zakończeniu zmiany w konfiguracji, kliknij pozycję **Zapisz** u dołu strony.
8. Przetestuj zmiany w konfiguracji. Aby uzyskać więcej informacji zobacz [Sprawdzanie ustawienia Menedżer ruchu](traffic-manager-testing-settings.md).
9. Po instalacji i pracy profilu Menedżer ruchu edytowanie rekordu DNS na serwerze DNS autorytatywne wskaż nazwę domeny firmy Menedżer ruchu nazwy domeny. Aby uzyskać więcej informacji na temat wykonywania tego zadania zobacz [punkt domeny internetowej firmy w domenie menedżer ruchu](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Następne kroki


[Wskaż domenę Menedżer ruchu domeny internetowej firmy](traffic-manager-point-internet-domain.md)

[Metody routingu Menedżer ruchu](traffic-manager-routing-methods.md)

[Konfigurowanie routingu metody pracy awaryjnej](traffic-manager-configure-failover-routing-method.md)

[Konfigurowanie metody routingu wydajności](traffic-manager-configure-performance-routing-method.md)

[Stan obniżonej wydajności rozwiązywania problemów Menedżer ruchu](traffic-manager-troubleshooting-degraded.md)

[Ruch Menedżer — wyłączyć, Włącz lub usuwanie profilu](disable-enable-or-delete-a-profile.md)

[Menedżer ruch — wyłączanie lub włączanie punktu końcowego](disable-or-enable-an-endpoint.md)

