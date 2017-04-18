<properties
   pageTitle="Skrypty na maszyny wirtualne Linux | Microsoft Azure"
   description="Automatyzowanie zadań konfiguracji Linux maszyn wirtualnych przy użyciu rozszerzenia skrypt niestandardowy"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="nepeters"/>

# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Rozszerzenie Azure skryptu niestandardowego za pomocą maszyn wirtualnych Linux

Rozszerzenie skrypt niestandardowy do pobrania i wykonuje skrypty w przypadku Azure maszyn wirtualnych. To rozszerzenie jest przydatne w przypadku konfiguracji wdrażania wpis, instalacja oprogramowania lub innych konfiguracji i zarządzania zadania. Skryptów można pobrane z Azure miejsca do magazynowania lub inne dostępne miejsce w Internecie lub dostarczane do uruchomienia rozszerzenia. Rozszerzenie niestandardowego skryptu można zintegrować z Menedżera zasobów Azure szablony, a także mogą być uruchamiane przy użyciu Azure interfejsu wiersza polecenia programu PowerShell, Azure portal i interfejsu API usługi REST maszyn wirtualnych Azure.

Ten dokument, szczegółowe informacje dotyczące rozszerzenie skrypt niestandardowy z polecenie Azure i szablonu wiadomości Menedżera zasobów Azure za pomocą, a także szczegóły kroków w systemach Linux.

## <a name="extension-configuration"></a>Rozszerzenie konfiguracji

Konfiguracja niestandardowy skrypt rozszerzenia określa elementy takie jak lokalizacja skryptu i polecenia do uruchomienia. Tej konfiguracji mogą być przechowywane w plikach konfiguracyjnych określonego w wierszu polecenia lub w szablonie Azure Menedżera zasobów. Poufne dane mogą być przechowywane w chronionym konfigurację, która jest szyfrowane i odszyfrowywane tylko wewnątrz maszyny wirtualnej. Chroniony konfiguracji jest przydatny, gdy polecenie wykonanie zawiera hasła, takich jak hasło.

### <a name="public-configuration"></a>Konfiguracja publicznej

Schemat:

- **commandToExecute**: (wymagane, ciąg) wykonywania skryptu punktu wejścia
- **fileUris**: (opcjonalne, ciąg tablica) adresy URL dla plików do pobrania.
- **Sygnatura czasowa** (opcjonalnie, liczba całkowita) za pomocą tego pola tylko wyzwalać Uruchom ponownie skryptu, zmieniając wartość tego pola.

```none
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Chroniony konfiguracji

Schemat:

- **commandToExecute**: (ciąg opcjonalny) wykonywania skryptu punktu wejścia. Zamiast tego użyj tego pola, jeśli polecenie zawiera hasła, takich jak haseł.
- **storageAccountName**: (ciąg opcjonalny) nazwę konta magazynu. Jeśli użytkownik określi magazynu poświadczeń, wszystkie fileUris musi być adresy URL dla obiektów blob platformy Azure.
- **storageAccountKey**: (ciąg opcjonalny) klawisz dostępu konta miejsca do magazynowania.


```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Polecenie Azure

Za pomocą interfejsu wiersza polecenia Azure uruchamianie rozszerzenia skrypt niestandardowy, tworzenie pliku konfiguracji lub plików zawierających co najmniej uri pliku i polecenie wykonywanie skryptów.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /scirpt-config.json
```

Opcjonalnie polecenia mogą być uruchamiane przy użyciu `--public-config` i `--private-config` opcję, która umożliwia konfigurowanie określonych podczas wykonywania oraz bez pliku konfiguracji oddzielnych.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

### <a name="azure-cli-examples"></a>Przykłady polecenie Azure

**Przykład 1** - publicznej konfiguracji z plik skryptu.

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
  "commandToExecute": "./hello.sh"
}
```

Polecenie Azure polecenie:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Przykład 2** - publicznej konfiguracji z nie plik skryptu.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Polecenie Azure polecenie:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Przykład 3** — pliku konfiguracji publicznej służy do określania plik skryptu identyfikatora URI, a plik konfiguracji chronionego służy do określania polecenie do wykonania.

Plik konfiguracyjny publicznej:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
}
```

Plik konfiguracyjny chronione:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Polecenie Azure polecenie:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path ./public.json --private-config-path ./protected.json
```

## <a name="resource-manager-template"></a>Szablon Menedżera zasobów

Rozszerzenie Azure niestandardowy skrypt można uruchamiać w czasie rozmieszczania maszyn wirtualnych przy użyciu szablonu Menedżera zasobów. W tym celu należy dodać JSON poprawnie sformatowany do szablonu wdrożenia.

### <a name="resource-manager-examples"></a>Przykłady Menedżera zasobów

**Przykład 1** - publicznej konfiguracji.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Przykład 2** — wykonanie polecenia w chronionym konfiguracji.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Zobacz .net Core muzyki sklepu pokaz pełny przykład - [Pokaz sklepu muzyki](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Po uruchomieniu z rozszerzeniem skrypt niestandardowy skrypt tworzenia lub pobrane do katalogu podobny do następującego przykładu. Dane wyjściowe polecenia również są zapisywane w tym katalogu `stdout` i `stderr` pliku.

```none
/var/lib/azure/custom-script/download/0/
```

Rozszerzenie skrypt Azure tworzy dziennika, który można znaleźć tutaj.

```none
/var/log/azure/customscript/handler.log
```

Stan wykonywania rozszerzenia skrypt niestandardowy także mogą być pobierane z polecenie Azure.

```none
azure vm extension get <resource-group> <vm-name>
```

Wynik wygląda następujący tekst:

```none
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Następne kroki

Aby informacji na temat innych rozszerzenia skryptów maszyn wirtualnych zobacz [Omówienie rozszerzenia skrypt Azure Linux](./virtual-machines-linux-extensions-features.md).