<properties
    pageTitle="Konfigurowanie metod routingu Menedżer ruchu | Microsoft Azure"
    description="W tym artykule wyjaśniono, jak konfiguruje różne metody routingu w Menedżerze ruchu"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-traffic-manager-routing-methods"></a>Konfigurowanie metod routingu Menedżer ruchu

Menedżer ruchu Azure oferuje trzy metody routingu, wpływających na sposób ruch jest kierowane do usług dostępnych punktów końcowych. Metoda routingu ruchu jest stosowana do każdej kwerendy DNS otrzymano w celu określenia, które punktu końcowego powinny być zwrócone w odpowiedzi DNS.

Dostępne są trzy metody routingu ruchu w Menedżerze ruchu:

- **Priority (priorytet):** Wybierz "Priorytet", jeśli chcesz używać punkt końcowy podstawowej usługi i podaj kopie zapasowe, w przypadku, gdy podstawowy jest niedostępny.
- **Ważona:** Wybierz pozycję "ważona" umożliwia rozpowszechnianie ruchu w zestawie punkty końcowe albo równomiernie lub według wagi, który możesz zdefiniować.
- **Wydajności:** Wybierz pozycję "Wydajności", gdy punkty końcowe w różnych miejscach i chcesz użytkownikom końcowym za pomocą punkt końcowy "najbliższy" w odniesieniu do najmniejszej czasu oczekiwania w sieci.

## <a name="configure-priority-routing-method"></a>Konfigurowanie metody routingu Priority (priorytet)

Bez względu na sposób witryny sieci Web witryny sieci Web Azure udostępnia już funkcji pracy awaryjnej witryn sieci Web w centrum danych (region). Menedżer ruchu miejsce pracy awaryjnej witryn sieci Web w różnych centrach danych.

Typowe deseń do przełączania awaryjnego usługi jest wysyłać ruch do podstawowej usługi i zawierają zestaw identyczne usługi tworzenia kopii zapasowych do przełączania awaryjnego. Poniższe kroki wyjaśniono, jak skonfigurować tej priorytetów pracy awaryjnej z usługami w chmurze Azure i witryny sieci Web:

1. W portalu klasyczny Azure, w okienku po lewej stronie kliknij ikonę **Menedżer ruchu** , aby otworzyć okienko Menedżer ruchu.
2. W okienku Menedżer ruchu w portalu klasyczny Azure Zlokalizuj profil Menedżer ruchu, zawierający ustawienia, które chcesz zmodyfikować. Aby otworzyć stronę ustawienia profilu, kliknij strzałkę po prawej stronie nazwę profilu.
3. Na stronie profilu kliknij pozycję **punkty końcowe** w górnej części strony. Upewnij się, że usług w chmurze i witrynami sieci Web, które mają zostać uwzględnione w konfiguracji znajdują.
4. U góry ekranu, aby otworzyć stronę konfiguracji, kliknij przycisk **Konfiguruj** .
5. W przypadku **Ustawienia metody routingu ruchu**Zweryfikuj metody routingu ruchu **pracy awaryjnej**. Jeśli nie jest dostępne, kliknij pozycję **pracy awaryjnej** z listy rozwijanej.
6. Aby **Liście priorytet pracy awaryjnej**można uporządkować pracy awaryjnej dla punktów końcowych. Po wybraniu metody routingu ruchu **pracy awaryjnej** ma znaczenie kolejność zaznaczonych punktów końcowych. Punkt końcowy podstawowego znajduje się na wierzchu. W górę i w dół strzałki w celu zmiany kolejności, zgodnie z potrzebami. Aby uzyskać informacje o konfigurowaniu priorytet przejęcia awaryjnego przy użyciu programu Windows PowerShell, zobacz [Ustawianie AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Sprawdź, czy **Ustawienia monitorowania** są odpowiednio skonfigurowane. Monitorowania gwarantuje, że punkty końcowe, które są w trybie offline nie są wysyłane ruch. Aby monitorować punkty końcowe, musisz określić ścieżkę i nazwę pliku. Ukośnik "/" jest prawidłowym wpisem względne ścieżki a oznacza, że plik znajduje się w katalogu głównym (ustawienie domyślne).
8. Po zakończeniu zmiany w konfiguracji, kliknij pozycję **Zapisz** u dołu strony.
9. Przetestuj zmiany w konfiguracji.
10. Po profilu Menedżer ruchu działa, edytowanie rekordu DNS na serwerze DNS autorytatywne wskaż nazwę domeny firmy Menedżer ruchu nazwy domeny.

## <a name="configure-weighted-routing-method"></a>Konfigurowanie ważonej metody routingu

Deseń typowe metody routingu ruchu jest zawierają zestaw identyczne punkty końcowe, które obejmują usług w chmurze i witrynami sieci Web, i przesyłać dane na każdej z nich w sposób okrężny. Następujące czynności w konspekcie, jak skonfigurować metody routingu ruchu na tego typu.

>[AZURE.NOTE] Azure witryn sieci Web Podaj już równoważenia funkcji witryn sieci Web w centrum danych (region) obciążenia okrężny. Menedżer ruchu umożliwia określenie metody routingu ruch okrężny witryn sieci Web w różnych centrach danych.

1. W portalu klasyczny Azure, w okienku po lewej stronie kliknij ikonę **Menedżer ruchu** , aby otworzyć okienko Menedżer ruchu.
2. W okienku Menedżer ruchu Zlokalizuj profil Menedżer ruchu, zawierający ustawienia, które chcesz zmodyfikować. Aby otworzyć stronę ustawienia profilu, kliknij strzałkę po prawej stronie nazwę profilu.
3. Na stronie profilu kliknij **punkty końcowe** w górnej części strony i sprawdź, czy istnieją punkty końcowe usługi, które mają zostać uwzględnione w konfiguracji.
4. Na stronie profilu kliknij przycisk **Konfiguruj** u góry ekranu, aby otworzyć stronę konfiguracji.
5. **Metoda routingu ruchu ustawienia**upewnij się, że używana jest metoda routingu ruchu **okrężnego**. Jeśli nie jest dostępne, kliknij pozycję **okrężnego** na liście rozwijanej.
6. Sprawdź, czy **Ustawienia monitorowania** są odpowiednio skonfigurowane. Monitorowania gwarantuje, że punkty końcowe, które są w trybie offline nie są wysyłane ruch. Aby monitorować punkty końcowe, musisz określić ścieżkę i nazwę pliku. Ukośnik "/" jest prawidłowym wpisem względne ścieżki a oznacza, że plik znajduje się w katalogu głównym (ustawienie domyślne).
7. Po zakończeniu zmiany w konfiguracji, kliknij pozycję **Zapisz** u dołu strony.
8. Przetestuj zmiany w konfiguracji.
9. Po profilu Menedżer ruchu działa, edytowanie rekordu DNS na serwerze DNS autorytatywne wskaż nazwę domeny firmy Menedżer ruchu nazwy domeny.

## <a name="configure-performance-traffic-routing-method"></a>Konfigurowanie metody routingu ruchu wydajności

Metody routingu ruchu wydajności umożliwia bezpośrednie ruch do punktu końcowego o najmniejszej opóźnienie z sieci klienta. Zazwyczaj centrum danych o najmniejszej opóźnienie jest najbardziej w odległości geograficznych. Ta metoda routingu ruch nie uwzględniania zmian w czasie rzeczywistym w konfiguracji sieci i ładowanie.

1. W portalu klasyczny Azure, w okienku po lewej stronie kliknij ikonę **Menedżer ruchu** , aby otworzyć okienko Menedżer ruchu.
2. W okienku Menedżer ruchu Zlokalizuj profil Menedżer ruchu, zawierający ustawienia, które chcesz zmodyfikować. Aby otworzyć stronę ustawienia profilu, kliknij strzałkę po prawej stronie nazwę profilu.
3. Na stronie profilu kliknij **punkty końcowe** w górnej części strony i sprawdź, czy istnieją punkty końcowe usługi, które mają zostać uwzględnione w konfiguracji.
4. Na stronie profilu kliknij przycisk **Konfiguruj** u góry ekranu, aby otworzyć stronę konfiguracji.
5. W przypadku **Ustawienia metody routingu ruchu**Zweryfikuj metody routingu ruchu * *wydajności*. Jeśli nie jest dostępne, kliknij przycisk * *wydajności** na liście rozwijanej.
6. Sprawdź, czy **Ustawienia monitorowania** są odpowiednio skonfigurowane. Monitorowania gwarantuje, że punkty końcowe, które są w trybie offline nie są wysyłane ruch. Aby monitorować punkty końcowe, musisz określić ścieżkę i nazwę pliku. Ukośnik "/" jest prawidłowym wpisem względne ścieżki a oznacza, że plik znajduje się w katalogu głównym (ustawienie domyślne).
7. Po zakończeniu zmiany w konfiguracji, kliknij pozycję **Zapisz** u dołu strony.
8. Przetestuj zmiany w konfiguracji.
9. Po profilu Menedżer ruchu działa, edytowanie rekordu DNS na serwerze DNS autorytatywne wskaż nazwę domeny firmy Menedżer ruchu nazwy domeny.

## <a name="next-steps"></a>Następne kroki

* [Zarządzanie profilami Menedżer ruchu](traffic-manager-manage-profiles.md)
* [Metody routingu Menedżer ruchu](traffic-manager-routing-methods.md)
* [Testowanie ustawień Menedżer ruchu](traffic-manager-testing-settings.md)
* [Wskaż domenę Menedżer ruchu domeny internetowej firmy](traffic-manager-point-internet-domain.md)
* [Zarządzanie punkty końcowe Menedżer ruchu](traffic-manager-manage-endpoints.md)
* [Stan obniżonej wydajności rozwiązywania problemów Menedżer ruchu](traffic-manager-troubleshooting-degraded.md)
