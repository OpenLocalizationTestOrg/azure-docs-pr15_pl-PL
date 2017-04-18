## <a name="service-chaining---transit-through-peered-vnet"></a>Usługa przypadku łączenia - przewozowe za pośrednictwem peered VNet

Użyj przekierowuje system ułatwia ruch automatycznie dla wdrożenia, istnieją przypadki, w których chcesz kontrolować routing pakietów za pośrednictwem wirtualnych urządzenia.
W tym scenariuszu istnieją dwa VNets w subskrypcji, HubVNet i VNet1 zgodnie z opisem na poniższej ilustracji. Wdrażanie Appliance(NVA) wirtualna sieć w VNet HubVNet. Po ustaleniu VNet zaglądanie między HubVNet i VNet1, możesz skonfigurować przekierowuje zdefiniowane przez użytkownika i określić następnego przeskoku do NVA w HubVNet.

![Czas przesyłania NVA](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] Dla uproszczenia założono, że wszystkie VNets w tym miejscu znajdują się w tej samej subskrypcji. Ale współpracuje ona także do scenariusza krzyżowe subskrypcji.

Właściwość klucza, aby włączyć routing przewozowe jest parametrem "Zezwalanie na ruch przekazywane". Dzięki temu akceptowanie i wysyłanie ruchu z NVA w peered VNet.  
