<properties
   pageTitle="Rozwiązywanie problemów z błędy rozszerzenia maszyn wirtualnych Linux | Microsoft Azure"
   description="Więcej informacji na temat rozwiązywania problemów błędy rozszerzenia maszyn wirtualnych Linux Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Rozwiązywanie problemów z błędy rozszerzenia maszyn wirtualnych Linux Azure

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Wyświetlanie stanu rozszerzenia
Azure szablony Menedżera zasobów mogą być wykonywane z polecenie Azure. Po wykonaniu tego szablonu można wyświetlić stan rozszerzenia z Eksploratora zasobów Azure lub narzędzi wiersza polecenia.

Oto przykład:

Azure interfejs wiersza polecenia:

      azure vm get-instance-view


Oto przykładowe dane wyjściowe:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extenson-failures"></a>Rozwiązywanie problemów z Extenson błędy:

### <a name="re-running-the-extension-on-the-vm"></a>Uruchom ponownie rozszerzenie na maszyn wirtualnych

Jeśli używasz skryptów maszyn wirtualnych przy użyciu rozszerzenia skrypt niestandardowy, czasami można napotkać błędu którym maszyn wirtualnych został utworzony pomyślnie, ale skrypt nie powiodło się. W obszarze tych warunków zalecany sposób, aby naprawić ten błąd jest usuwanie rozszerzenia i ponownie uruchom ponownie szablon.
Uwaga: W przyszłości tej funkcji można rozszerzyć usunąć potrzebę odinstalowanie rozszerzenia.

#### <a name="remove-the-extension-from-azure-cli"></a>Usuwanie rozszerzenia polecenie Azure

      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

Miejsce, w którym "publsher nazwa" odpowiada typowi rozszerzenia z wyników "azure maszyn wirtualnych get wystąpienie — widok", a nazwa to nazwę zasobu rozszerzenia z szablonu

Po usunięciu rozszerzenia szablonu może być ponownie wykonane uruchamiania skryptów na maszyn wirtualnych.
