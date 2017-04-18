## <a name="scenario"></a>Scenariusz

Aby lepiej zilustrować sposobów tworzenia UDRs, ten dokument będzie używany scenariusz poniżej.

![OPIS OBRAZU](./media/virtual-network-create-udr-scenario-include/figure1.png)

W tym scenariuszu utworzysz jeden UDR *wierzch zakończenia podsieci* i innym UDR *Wstecz zakończenia podsieci* , zgodnie z poniższym opisem: 

- **UDR FrontEnd**. Fronton UDR zostanie zastosowany do danej podsieci *serwera sieci Web* i zawierają jednej trasy:  
    - **RouteToBackend**. Tą drogą wyśle cały ruch do podsieci wewnętrznej maszyn wirtualnych **FW1** .
- **Wewnętrznej bazy danych UDR**. Wewnętrzna UDR zostanie zastosowany do danej podsieci *wewnętrznej bazy danych* i zawierają jedna droga: 
    - **RouteToFrontend**. Tą drogą wyśle cały ruch do podsieci fronton maszyn wirtualnych **FW1** .

Kombinacja te trasy będziesz mieć pewność, że cały ruch przeznaczony z jednej podsieci będą kierowane do maszyny wirtualnej **FW1** , który jest używany jako wirtualna urządzenia. Należy włączyć funkcję przesyłania adresów IP dla tego Głosowa, aby upewnić się, że może odbierać ruchu przeznaczonego dla innych maszyny wirtualne.
