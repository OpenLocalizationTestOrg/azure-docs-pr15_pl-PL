Zapłacić za dwie czynności: co godzina obliczania kosztów dla bramy wirtualnej sieci i dane wyjściowe przenoszenie Brama wirtualnej sieci. Informacje o cenach można znaleźć na stronie [ceny](https://azure.microsoft.com/pricing/details/vpn-gateway) .

**Wirtualna sieć bramy obliczeń kosztów**<br>Każdej bramy wirtualną sieć ma co godzina obliczania kosztów. Cena jest oparty na bramy SKU określona podczas tworzenia bramy wirtualnej sieci. Koszt jest samej bramy i oprócz transferu danych przepływał przez bramę.

**Koszty przeniesienia danych**<br>Koszty transfer danych są obliczane na podstawie wyjściowym ruchu z bramy wirtualną sieć źródło.

- Jeśli wysyłasz ruch do lokalnego urządzenia VPN, zostanie obciążony szybkość przesyłania dane wyjściowe Internet.
- Jeśli wysyłasz ruch między wirtualnych sieci w różnych regionach ceny podstawie regionu.
- Jeśli wysyłasz ruch tylko między wirtualnych sieci, które znajdują się w tym samym regionie, istnieje bez kosztów danych. Ruch między VNets w tym samym regionie jest bezpłatna.