## <a name="scenario"></a>Scenariusz

Aby lepiej zilustrować sposobów tworzenia VNet i podsieci, ten dokument będzie używany scenariusz poniżej.

![Scenariusz VNet](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

W tym scenariuszu utworzysz VNet o nazwie **TestVNet** z zastrzeżone blok CIDR **192.168.0.0./16**. Usługi VNet będzie zawierać podsieci następujące czynności: 

- **Serwera sieci Web**, używając **192.168.1.0/24** jako jego blok CIDR.
- **Wewnętrznej bazy danych**, używając **192.168.2.0/24** jako jego blok CIDR.

 