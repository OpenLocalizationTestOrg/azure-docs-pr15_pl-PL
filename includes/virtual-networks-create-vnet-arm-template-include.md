## <a name="download-and-understand-the-arm-template"></a>Pobieranie i opis szablonu ARM

Możesz pobrać istniejący szablon ARM tworzenia z github VNet i dwóch podsieci, wprowadź odpowiednie zmiany, może być i użyć jej ponownie. Aby to zrobić, wykonaj poniższe czynności.

1. Przejdź do [strony szablonu próbki](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Kliknij **azuredeploy.json**, a następnie kliknij opcję **RAW**.
3. Zapisywanie pliku do lokalnego folderu na komputerze.
4. Użytkownicy zaznajomieni z szablonami ARM, przejdź do kroku 7.
5. Otwórz plik, który został zapisany, a następnie spójrz na zawartość w obszarze **parametrów** w wierszu 5. Parametry szablonu ARM zapewniają symbolu zastępczego wartości, które można wypełniać podczas wdrażania.

    | Parametr | Opis |
    |---|---|
    | **Lokalizacja** | Azure region, w której zostanie utworzony VNet |
    | **vnetName** | Nazwę nowego VNet |
    | **addressPrefix** | Wpisanie adresu VNet, w formacie CIDR |
    | **subnet1Name** | Nazwę pierwszej VNet |
    | **subnet1Prefix** | Blok CIDR pierwszą podsieć |
    | **subnet2Name** | Nazwę drugiej VNet |
    | **subnet2Prefix** | Blok CIDR drugą podsieć |

    >[AZURE.IMPORTANT] Szablony ARM przechowywane w github można zmienić w czasie. Upewnij się, że możesz sprawdzić przed użyciem tego szablonu.
    
6. Zaznacz zawartość w obszarze **zasoby** i zwróć uwagę na następujące czynności:

    - **Typ**. Typ zasobu zostanie utworzony w szablonie. W tym przypadku **Microsoft.Network/virtualNetworks**, które odpowiadają VNet.
    - **Nazwa**. Nazwa zasobu. Zwróć uwagę, użyj **[parameters('vnetName')]**, co oznacza nazwę będą dostępne jako danych wejściowych przez użytkownika lub pliku parametrów podczas wdrażania.
    - **Właściwości**. Lista właściwości dla zasobu. Ten szablon korzysta z właściwości miejsca i podsieci adresu podczas tworzenia VNet.

7. Przejść z powrotem do [strony szablonu próbki](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Kliknij **azuredeploy paremeters.json**, a następnie kliknij opcję **RAW**.
9. Zapisywanie pliku do lokalnego folderu na komputerze.
10. Otwórz plik, który został zapisany i zmodyfikuj wartości parametrów. Umożliwia wdrażanie VNet opisaną w naszym scenariuszu poniższe wartości.

        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }

11. Zapisz plik.
  