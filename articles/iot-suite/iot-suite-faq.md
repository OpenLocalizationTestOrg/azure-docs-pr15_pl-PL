<properties
  pageTitle="Pakiet Azure IoT — często zadawane pytania | Microsoft Azure"
  description="Często zadawane pytania dotyczące pakietu IoT"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="09/26/2016"
  ms.author="araguila"/>
   
# <a name="frequently-asked-questions-for-iot-suite"></a>Często zadawane pytania dotyczące pakietu IoT

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Jaka jest różnica między usunięcie grupy zasobów w portalu Azure i klikając polecenie Usuń na wstępnie rozwiązanie w azureiotsuite.com?

- Jeśli usuniesz wstępnie rozwiązanie w [azureiotsuite.com][lnk-azureiotsuite], możesz usunąć wszystkie zasoby, które Obsługa administracyjna została zainicjowana przy tworzeniu wstępnie rozwiązanie; Jeśli dodatkowe zasoby są dodawane do grupy zasobów, są usuwane są również. 

- Jeśli usuniesz grupa zasobów w [Azure portal][lnk-azure-portal], możesz usunąć tylko zasobów w danej grupy zasobów. należy również usunąć aplikację usługi Azure Active Directory skojarzone z wstępnie rozwiązanie w [portal Azure klasyczny][lnk-classic-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Ile wystąpień Centrum IoT umożliwia obsługę w subskrypcji? 

Dziesięć. Możesz utworzyć [bilet pomocy technicznej Azure] [ link-azuresupportticket] podnieść ten limit, ale domyślnie umożliwia tylko obsługę dziesięć koncentratory IoT na subskrypcję, zgodnie z zaleceniami w [Limity subskrypcji Azure][link-azuresublimits]. W wyniku od każdej wstępnie rozwiązanie postanowienia nowego koncentratora IoT umożliwia obsługę tylko do dziesięciu wstępnie rozwiązań w danej subskrypcji. 

### <a name="how-many-documentdb-instances-can-i-provision-in-a-subscription"></a>Ile wystąpień DocumentDB umożliwia obsługę w subskrypcji?

50. Możesz utworzyć [bilet pomocy technicznej Azure] [ link-azuresupportticket] podnieść ten limit, ale domyślnie umożliwia tylko obsługę pięćdziesiąt wystąpienia DocumentDB na subskrypcję. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Ile bezpłatne API mapy Bing umożliwia obsługę w subskrypcji?

Dwa. Możesz utworzyć tylko dwa wewnętrznych transakcje poziomu 1 mapy Bing w przypadku planów Enterprise Azure subskrypcji. Zdalny rozwiązanie monitorowania jest obsługi administracyjnej domyślnie z planem wewnętrznych transakcje poziomu 1. W wyniku umożliwia tylko obsługę do dwóch zdalnego monitorowania rozwiązań w subskrypcji bez żadnych modyfikacji.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Mam zdalnego monitorowania wdrażania rozwiązania z statyczną mapę, jak dodać interaktywne mapy Bing? 
1. Pobieranie programu interfejsu API mapy Bing QueryKey przedsiębiorstwa z [Azure portal][lnk-azure-portal]: 
 1. Przejdź do grupy zasobów, w przypadku usługi interfejsu API mapy Bing dla przedsiębiorstw w [Azure portal][lnk-azure-portal].
 2. Kliknij pozycję Ustawienia, następnie Key Management. 
 3. Można zauważyć dwa klucze: umożliwia i QueryKey. Skopiuj wartość dla QueryKey.

     > [AZURE.NOTE] Nie masz interfejs API mapy Bing dla konta Enterprise? Tworzyć [Azure portal] [ lnk-azure-portal] przez kliknięcie + nowy, wyszukując interfejsu API mapy Bing dla przedsiębiorstw i postępuj zgodnie z instrukcjami, aby utworzyć.

2. Rozwijane najnowszą kod z [Azure IoT-Remote-monitorowania][lnk-remote-monitoring-github].

3. Uruchom lokalnie lub w chmurze wdrożenia zgodnie z zaleceniami wiersza polecenia wdrożenia w folderze /docs/ w repozytorium. 

4. Po uruchomieniu lokalnie lub w chmurze wdrożenia, poszukaj w folderze głównym dla *. user.config plik utworzony w trakcie wdrożenia. Otwórz plik w edytorze tekstów. 

5. Zmień następujący wiersz, aby umieścić skopiowany z usługi QueryKey wartość: 
   
  `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Czy można utworzyć wstępnie rozwiązanie, jeśli mam już zainstalowany Microsoft Azure dla DreamSpark?
W tej chwili nie można utworzyć rozwiązanie wstępnie skonfigurowanych z [Platformy Microsoft Azure dla DreamSpark] [ lnk-dreamspark] konta. Jednak można utworzyć [bezpłatne konto wersji próbnej dla Azure] [ lnk-30daytrial] na kilka minut, które umożliwiają tworzenie wstępnie rozwiązanie.

### <a name="how-do-i-delete-an-aad-tenant"></a>Jak usunąć dzierżawę AAD?

Zobacz [Instruktaż usuwania dzierżawę Azure AD]blogu marek Golpe[lnk-delete-aad-tennant].

## <a name="next-steps"></a>Następne kroki

Można również zapoznać się niektóre z innych funkcji i możliwości rozwiązań pakietu IoT wstępnie:

- [Omówienie rozwiązania przewidywanych konserwacji wstępnie][lnk-predictive-overview]
- [Zabezpieczenia IoT od podstaw][lnk-security-groundup]

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-security-groundup]: securing-iot-ground-up.md

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
