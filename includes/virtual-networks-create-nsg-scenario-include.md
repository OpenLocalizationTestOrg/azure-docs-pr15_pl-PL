## <a name="scenario"></a>Scenariusz

Aby lepiej zilustrować sposobów tworzenia NSGs, ten dokument będzie używany scenariusz poniżej.

![Scenariusz VNet](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

W tym scenariuszu utworzysz NSG dla każdej podsieci w wirtualnej sieci **TestVNet** , zgodnie z poniższym opisem: 

- **NSG FrontEnd**. Fronton NSG zostanie zastosowany do danej podsieci *serwera sieci Web* i zawierają dwie reguły:  
    - **Reguła rdp**. Ta reguła będzie zezwalanie na ruch RDP do podsieci *serwera sieci Web* .
    - **Reguła w sieci web**. Ta reguła będzie zezwalanie na ruch HTTP do podsieci *serwera sieci Web* .
- **NSG-wewnętrznej bazy danych**. Wewnętrzna NSG zostanie zastosowany do podsieci *wewnętrznej bazy danych* i zawierają dwie reguły: 
    - **Reguła sql**. Ta reguła zezwala na ruch SQL tylko z podsieci *serwera sieci Web* .
    - **Reguła w sieci web**. Ta reguła powoduje zablokowanie wszystkich internet powiązać ruchu z podsieci *wewnętrznej bazy danych* .

Kombinacja tych reguł utworzyć scenariusz przypominających strefy Zdemilitaryzowanej, gdzie podsieci wewnętrznej może odbierać ruch przychodzący SQL z podsieci fronton i podczas podsieci fronton można komunikować się z Internetem i odbierać przychodzące żądania HTTP nie ma dostępu do sieci Internet.
 
