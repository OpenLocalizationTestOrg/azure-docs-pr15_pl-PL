<properties
    pageTitle="Sterowanie ruchu aplikacji Azure sieci web przy użyciu Menedżera ruch Azure"
    description="Ten artykuł zawiera informacje podsumowujące dla Menedżera ruch Azure pod kątem aplikacji Azure web."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Sterowanie ruchu aplikacji Azure sieci web przy użyciu Menedżera ruch Azure

> [AZURE.NOTE] Ten artykuł zawiera informacje podsumowujące Microsoft Azure ruch Manager pod kątem Azure aplikacji usługi sieci Web. Więcej informacji na temat Azure ruch Menedżera sam można znaleźć na stronie łącza na końcu tego artykułu.

## <a name="introduction"></a>Wprowadzenie
Menedżer ruchu Azure umożliwia sterowanie rozkład żądania od klientów sieci web do aplikacji sieci web w usłudze Azure aplikacji. Po dodaniu punkty końcowe aplikacji sieci web do profilu Menedżer ruchu Azure Menedżer ruchu Azure rejestruje informacje o stan aplikacji sieci web (uruchomiony, przestał lub usunięte) tak, aby można określić, które te punkty końcowe otrzymujących ruch.

## <a name="load-balancing-methods"></a>Metody równoważenia obciążenia
Azure Menedżer ruchu używa trzech metod równoważenia obciążenia różnych. Są one opisane na poniższej liście, jak odnoszą się do aplikacji sieci web Azure.

* **Pracy awaryjnej**: Jeśli masz klony aplikacji sieci web w różnych regionów można skonfigurować jedną aplikację sieci web do obsługi całego ruchu klienta sieci web za pomocą tej metody można użyć i skonfigurować innej aplikacji sieci web w innym regionie do obsługi ruch w przypadku pierwszej aplikacji sieci web jest niedostępny.

* **Okrężnego**: Jeśli masz klony aplikacji sieci web w różnych regionów umożliwia ta metoda dystrybucji ruchu jednakowo w aplikacjach sieci web w różnych regionach.

* **Wydajność**: metody wydajności rozdziela ruch na podstawie najkrótszego czasu wysyłającego klientom. Metoda wydajności można używać w przypadku aplikacji sieci web, w tym samym regionie lub w rożnych regionów.

##<a name="web-apps-and-traffic-manager-profiles"></a>Aplikacje sieci Web i ruch Menedżer profilów
Aby skonfigurować kontrolę nad ruchu w aplikacji sieci web, tworzenie profilu w Azure Menedżer ruchu że przy użyciu jednej z tych trzech załadować równoważenia metod opisanych wcześniej, a następnie dodaj punkty końcowe (w tym przypadku aplikacji sieci web) dla których chcesz kontrolować ruch do profilu. Stan aplikacji sieci web (działać, zatrzymać lub usunięte) jest regularnie przekazywane do profilu, aby Menedżer ruchu Azure może kierować ruch odpowiednio.

Gdy Menedżer ruchu Azure za pomocą Azure, należy pamiętać następujące punkty:

* Wdrożeniach web app tylko w ramach tego samego regionu aplikacje sieci Web zawiera już awaryjnego i funkcji karuzeli bez uwzględniania tryb aplikacji sieci web.

* W przypadku wdrożeń w tym samym regionie korzystających z aplikacji Web Apps w połączeniu z innej usłudze w chmurze Azure można połączyć oba typy punkty końcowe, aby włączyć scenariuszy hybrydowego.

* Można określić tylko jeden punkt końcowy aplikacji sieci web rozbiciu na regiony w profilu. Po wybraniu aplikacji sieci web jako punkt końcowy dla jednego regionu pozostałe aplikacje sieci web w danym regionie stają się niedostępne w przypadku zaznaczenia tego profilu.

* Punkty końcowe aplikacji sieci web, określone w profilu Menedżera ruch Azure pojawią się w sekcji **Nazw domen** na stronie Konfigurowanie aplikacji sieci web w profilu, ale nie będzie można konfigurować.

* Po dodaniu aplikacji sieci web do profilu, **Adres URL witryny** na pulpicie nawigacyjnym strony portalu aplikacji sieci web będzie wyświetlany adres URL domeny niestandardowej aplikacji sieci web, jeśli masz skonfigurowane jedną. W przeciwnym wypadku Wyświetl adres URL profilu Menedżer ruchu (na przykład `contoso.trafficmgr.com`). Nazwę domeny bezpośredni aplikacji sieci web i adres URL Menedżera ruch będą widoczne na stronie Konfigurowanie aplikacji sieci web w sekcji **Nazw domen** .

* Nazwy domeny niestandardowej działa zgodnie z oczekiwaniami, ale oprócz dodawania ich do aplikacji sieci web, musisz również skonfigurować mapę DNS, aby wskazywały adres URL Menedżera ruch. Aby uzyskać informacje dotyczące konfigurowania domeny niestandardowej dla aplikacji sieci Azure web zobacz [Konfigurowanie niestandardowej nazwy domeny dla Azure witryny sieci web](web-sites-custom-domain-name.md).

* Można dodać tylko aplikacjami sieci web, które są w trybie standardowym do profilu Menedżer ruchu Azure.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać koncepcyjny i techniczne omówienie Menedżer ruchu Azure zobacz [Omówienie Menedżera ruch](../traffic-manager/traffic-manager-overview.md).

Aby uzyskać więcej informacji o korzystaniu z Menedżera ruchu z aplikacjami sieci Web Zobacz wpisów blogu [Przy użyciu Menedżera ruch Azure z witryny sieci Web Azure](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) i [Menedżer ruchu Azure teraz można zintegrować z witryny sieci Web Azure](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).
