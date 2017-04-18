<properties
    pageTitle="Nawiązywanie połączenia z Azure stos polecenie | Microsoft Azure"
    description="Dowiedz się, jak wdrożyć zasobów w stos Azure i zarządzanie nimi za pomocą interfejsu wiersza polecenia i platform (polecenie)"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>Instalowanie i konfigurowanie polecenie stos Azure

W tym dokumencie możemy poprowadzą Cię proces zarządzania zasobami Azure stos na platformach klienta Linux i komputerów Mac za pomocą interfejsu wiersza polecenia Azure (polecenie).  

## <a name="install-azure-stack-cli"></a>Instalowanie Azure stosu interfejsu wiersza polecenia

Jeśli jesteś na Mac lub Linux, polecenie można uzyskać przy użyciu następującego polecenia:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>Nawiązywanie połączenia z stos Azure
W następującej procedurze można skonfigurować polecenie Azure, aby nawiązać połączenie stos Azure. Następnie zaloguj się i pobrać informacje o subskrypcji.

1.  Pobierz wartość w polu zasobu identyfikator, active directory w-przez wykonanie tego programu PowerShell:
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  Użyj następującego polecenia polecenie dodać środowiska stos Azure, pamiętając zaktualizować *— zasobów identyfikator, active directory w-* z danymi adresu URL pobrane w poprzednim kroku:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  Zaloguj się przy użyciu następującego polecenia (Zamień *nazwę użytkownika* z nazwą użytkownika):

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]W razie wystąpienia problemów sprawdzania poprawności certyfikatów wyłączyć sprawdzanie poprawności certyfikatu, uruchamiając polecenie `set        NODE_TLS_REJECT_UNAUTHORIZED=0`.

4.  Ustawianie trybu Azure konfiguracji do Menedżera zasobów Azure przy użyciu następującego polecenia:

        azure config mode arm

5.  Pobieranie listy subskrypcji.

        azure account list     

## <a name="next-steps"></a>Następne kroki

[Wdrażanie szablonów z polecenie Azure](azure-stack-deploy-template-command-line.md)

[Nawiązywanie połączenia z programem PowerShell](azure-stack-connect-powershell.md)

[Zarządzanie uprawnieniami użytkowników](azure-stack-manage-permissions.md)
