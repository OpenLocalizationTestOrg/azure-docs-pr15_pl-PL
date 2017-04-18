<properties
    pageTitle="Tworzenie klastrów Hadoop, HBase lub Burza na Linux w przy użyciu zwinięcie i interfejsu API usługi REST Azure HDInsight | Microsoft Azure"
    description="Dowiedz się, jak utworzyć klastrów systemem Linux HDInsight przy użyciu zwinięcie, szablony Menedżera zasobów Azure i interfejsu API usługi REST Azure. Możesz określić typ klaster (Hadoop, HBase lub Burza) lub za pomocą skryptów można zainstalować składniki niestandardowe."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Tworzenie klastrów systemem Linux w przy użyciu zwinięcie i interfejsu API usługi REST Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Interfejsu API usługi REST Azure umożliwia wykonywanie operacji zarządzania na usługi obsługiwane platformy Azure, włączając w to tworzenie nowych zasobów, takich jak klastrów HDInsight systemem Linux. W tym dokumencie dowiesz się, jak tworzyć szablony Azure Menedżera zasobów, aby skonfigurować klaster HDInsight i skojarzone miejsca do magazynowania, a następnie użyj zwinięcie wdrożyć szablonu do interfejsu API usługi REST Azure, aby utworzyć nowy klaster HDInsight.

> [AZURE.IMPORTANT] Kroki opisane w tym dokumencie oznaczać klaster HDInsight domyślnej liczby węzłów pracownika (4). Jeśli planujesz na więcej niż 32 węzłach pracownika, podczas tworzenia klaster lub skalowania klaster po utworzeniu, musisz wybrać odpowiedni rozmiar węzła głównego z co najmniej 8 rdzeni i 14GB pamięci ram.
>
> Aby uzyskać więcej informacji na węzeł rozmiarów i koszty zobacz [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Wymagania wstępne

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Polecenie azure__. Polecenie Azure służy do tworzenia głównej usługi, która następnie jest używana do generowania tokenów uwierzytelniania dla żądania interfejsu API usługi REST Azure.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __zawinięcie__. To narzędzie jest dostępne za pośrednictwem systemu zarządzania pakiet lub można pobrać z [http://curl.haxx.se/](http://curl.haxx.se/).

    > [AZURE.NOTE] Jeśli korzystasz z programu PowerShell do uruchomienia polecenia w tym dokumencie, należy najpierw usunąć `curl` alias, który tworzy domyślnie. Ten alias używa WebRequest Wywołaj, polecenia cmdlet programu PowerShell, zamiast zwinięcie, korzystając z `curl` polecenie w wierszu polecenia programu PowerShell i zwróci błędy dla wielu poleceń używanych w tym dokumencie.
    > 
    > Aby usunąć ten alias, należy użyć następujących w wierszu polecenia programu PowerShell:
    >
    > `Remove-item alias:curl`
    >
    > Po usunięciu alias, należy korzystać z wersji zwinięcie zainstalowanym na komputerze.

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>Tworzenie szablonu

Azure szablony zarządzania zasobami są JSON dokumentów, które opisują __Grupa zasobów__ i wszystkie zasoby w nim (na przykład HDInsight). Tej metody podstawie umożliwia definiowanie wszystkie zasoby, które są potrzebne do HDInsight w jednym szablonie oraz zarządzanie zmianami do grupy całej __wdrożeń__ Zastosuj zmiany do grupy.

Szablony zazwyczaj znajdują się w dwóch części; samego szablonu, a plik parametrów wypełnianie wartościami określone w konfiguracji. Exmaple, klaster nazwy, nazwę administratora i hasło. Gdy bezpośrednio za pomocą interfejsu API usługi REST, musisz połączyć je w jeden plik. Format ten dokument JSON jest:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Poniżej przedstawiono na przykład połączenie plików szablonu i parametrów z [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), który tworzy klaster systemem Linux bezpiecznego SSH konto użytkownika za pomocą hasła.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

W tym przykładzie zostanie użyty w krokach w tym dokumencie. Symbol zastępczy _wartości_ w sekcji __Parametry__ na końcu dokumentu należy zamienić na wartości, które mają być używane dla klaster.

##<a name="login-to-your-azure-subscription"></a>Zaloguj się do subskrypcji usługi Azure

Wykonaj kroki opisane w [Nawiązywanie połączenia z subskrypcji usługi Azure z interfejsu wiersza polecenia Azure (polecenie Azure)](../xplat-cli-connect.md) i łączenie się z subskrypcji przy użyciu `azure login` polecenia.

##<a name="create-a-service-principal"></a>Tworzenie wystawcy usługi

> [AZURE.NOTE] Te kroki są skróconej wersji informacji zawartych w sekcji _głównej za pomocą hasła — polecenie Azure usługi uwierzytelniania_ [uwierzytelniania wystawcy usługi przy użyciu Menedżera zasobów Azure](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli) dokumentu. Poniższe czynności, Utwórz nową usługę kapitału, który może służyć do uwierzytelniania żądań interfejsu API usługi REST użyte do utworzenia Azure zasobów, takich jak klaster HDInsight.

1. Z poziomu wiersza polecenia, sesja lub powłoki Użyj następującego polecenia do wyświetlania listy subskrypcji Azure.

        azure account list
        
    Na liście Wybierz subskrypcję, do której chcesz użyć i zanotuj kolumny __identyfikator__ . To jest __identyfikator subskrypcji__ i będą używane w większości kroki opisane w tym dokumencie.

2. Tworzenie nowej aplikacji w usłudze Azure Active Directory.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Zamienianie wartości dla `--name`, `--home-page`, i `--identifier-uris` z własne wartości. Podanie hasła dla nowego wpisu usługi Active Directory.
    
    > [AZURE.NOTE] Ponieważ tworzysz tej aplikacji dla uwierzytelniania za pośrednictwem wystawcy usługi `--home-page` i `--identifier-uris` wartości nie muszą zostać utworzone odwołanie rzeczywistą stronę sieci web hostowana w Internecie. po prostu muszą być unikatowe identyfikatory URI.
    
    Przy użyciu danych zwróconych zapisać wartość __Identyfikator aplikacji__ .
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Tworzenie głównej przy użyciu wartości __Identyfikator aplikacji__ zwracane wcześniej usługi.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     Przy użyciu danych zwróconych zapisać wartość __Identyfikatora obiektu__ .
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. Przypisywanie roli __właściciela__ z usługą kapitału przy użyciu __Identyfikatora obiektu__ wartości zwracane wcześniej. __Identyfikator subskrypcji__ , który został pobrany wcześniej również należy użyć.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Po wykonaniu tego polecenia głównej usługi ma teraz właściciela dostęp do identyfikatora określonej subskrypcji.

##<a name="get-an-authentication-token"></a>Pobierz token uwierzytelniania

1. Znajdowanie __Identyfikatora dzierżawy__ dla subskrypcji należy wykonać następujące kroki.

        azure account show -s <subscription ID>
        
    Zwrócone dane znajdowanie __Identyfikatora dzierżawy__.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Generowanie nowego tokenu za pomocą interfejsu API usługi REST Azure.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    Zamień __TenantID__, __Identyfikator aplikacji__i __hasło__ wartości uzyskane w wyniku lub używane wcześniej.

    W przypadku pomyślnego tę prośbę, otrzymasz odpowiedź 200 serii i treść odpowiedzi będzie zawierać dokumentu JSON.

    Dokument JSON zwracane przez to żądanie będzie zawierać elementu o nazwie __access_token__; wartość tego elementu jest token dostępu, należy użyć do uwierzytelniania żądania używane w kolejnych sekcjach tego dokumentu.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Tworzenie nowej grupy zasobów, należy wykonać następujące kroki. Musisz najpierw utworzyć grupę przed utworzeniem zasobów, takich jak klaster HDInsight. 

* Zamień __SubscriptionID__ identyfikator subskrypcji podczas tworzenia wystawcy usługi.
* Zamień __AccessToken__ token dostępu otrzymanych w poprzednim kroku.
* Zamień __DataCenterLocation__ centrum danych, które chcesz utworzyć grupy zasobów i zasoby, w. Na przykład "Południe centralnej US". 
* Zastąp __ResourceGroupName__ nazwę, której chcesz użyć dla tej grupy:

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

W przypadku pomyślnego tę prośbę, otrzymasz odpowiedź 200 serii i treść odpowiedzi będzie zawierać JSON dokumentu zawierającego informacje o grupie. `"provisioningState"` Element będzie zawierać wartość `"Succeeded"`.

##<a name="create-a-deployment"></a>Tworzenie wdrożeniu

Za pomocą następujących do wdrażania konfiguracji klaster (szablon i wartości parametrów,) do grupy zasobów.

* Zamień wartości używane wcześniej __SubscriptionID__ i __AccessToken__ . 
* Zamień __ResourceGroupName__ Nazwa grupy zasobów utworzonego w poprzedniej sekcji.
* Zamień __DeploymentName__ nazwę, której chcesz użyć dla tego wdrożenia.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Jeśli został już zapisany dokument JSON zawierający szablon i parametrów do pliku, możesz użyć następujących zamiast `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

W przypadku pomyślnego tę prośbę, otrzymasz odpowiedź 200 serii i treść odpowiedzi będzie zawierać JSON dokumentu zawierającego informacje o operacji wdrożenia.

> [AZURE.IMPORTANT] Należy zauważyć, że wdrażanie zostały przesłane, ale nie została ukończona w tej chwili. Może potrwać kilka minut, zwykle około 15 wdrożenia do wykonania.

##<a name="check-the-status-of-a-deployment"></a>Sprawdzanie stanu instalacji

Aby sprawdzić stan wdrażania, użyj następujących opcji:

* Zamień wartości używane wcześniej __SubscriptionID__ i __AccessToken__ . 
* Zamień __ResourceGroupName__ Nazwa grupy zasobów utworzonego w poprzedniej sekcji.

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

To spowoduje zwrócenie informacji JSON dokumentu zawierającego informacje o operacji wdrożenia. `"provisioningState"` Element będzie zawierać stan wdrożenia; Jeśli ta strona zawiera wartość `"Succeeded"`, następnie wdrażanie zakończyło się pomyślnie. W tym momencie klaster powinny być dostępne do użycia.

##<a name="next-steps"></a>Następne kroki

Teraz, gdy został pomyślnie utworzony klaster HDInsight, Dowiedz się, jak pracować z klaster należy wykonać następujące kroki. 

###<a name="hadoop-clusters"></a>Klastrów Hadoop

* [Gałąź za pomocą usługi HDInsight](hdinsight-use-hive.md)
* [Świnka korzystanie z usługi HDInsight](hdinsight-use-pig.md)
* [Używanie MapReduce z usługi HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>Klastrów HBase

* [Rozpoczynanie pracy z HBase na HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Można opracowywać aplikacje Java dla HBase na HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Klastrów Burza

* [Można opracowywać topologii Java dla Burza na HDInsight](hdinsight-storm-develop-java-topology.md)
* [Używanie składników Python w Burza na HDInsight](hdinsight-storm-develop-python-topology.md)
* [Wdrażanie i monitorowanie topologii z Burza na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
