Podczas tworzenia bramy wirtualnej sieci, musisz określić bramy SKU, którego chcesz użyć. Po wybraniu wyższą bramy SKU więcej procesorów i przepustowość sieci zostało przydzielonych do bramy, a w wyniku bramy może obsługiwać wyższą przepustowość sieci wirtualnej sieci.

Brama VPN można używać następujących wersji produktu:

- Podstawowe
- Standardowe
- O odwróconej

Po wybraniu SKU należy rozważyć następujące kwestie:

- Jeśli chcesz użyć typu PolicyBased VPN, należy użyć podstawowa jednostka SKU. PolicyBased sieci VPN (wcześniej nazywane Routing statyczny) nie są obsługiwane na innych wersji.
- BGP nie jest obsługiwana w wersji podstawowej.
- Brama ExpressRoute VPN występować konfiguracje nie są obsługiwane w wersji podstawowej.
- Połączenia bramy sieci VPN S2S aktywny aktywny można skonfigurować dla tylko za pomocą SKU o odwróconej.
