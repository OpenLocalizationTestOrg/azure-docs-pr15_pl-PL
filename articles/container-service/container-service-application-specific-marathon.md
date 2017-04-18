<properties
   pageTitle="Aplikacja lub usługa Marathon specyficzne dla użytkownika | Microsoft Azure"
   description="Tworzenie aplikacji lub usługi Marathon specyficzne dla użytkownika"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontenery, Marathon, Micro usług, kontrolera domeny i systemu operacyjnego, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# <a name="create-an-application-or-user-specific-marathon-service"></a>Tworzenie aplikacji lub usługi Marathon specyficzne dla użytkownika

Usługa Azure kontenera zapewnia zestaw serwerów głównych, na których wstępnie możemy skonfigurować Apache Mesos i Marathon. Te można dodać akompaniament aplikacji w klastrze, ale najlepiej nie chcesz używać serwerów głównych w tym celu. Na przykład ta konfiguracja Marathon wymaga logowania do serwerów głównych, się i wprowadzania zmian — to zaleca unikatowe serwerów głównych są nieco inne ze standardem i konieczności opieką i zarządzany niezależnie. Ponadto konfigurację wymaganą przez jeden zespół może nie być optymalną konfigurację innego zespołu.

W tym artykule firma Microsoft będzie wyjaśniono, jak dodać aplikacji lub usługi Marathon specyficzne dla użytkownika.

Ponieważ ta usługa będzie znajdować się pojedynczego użytkownika lub zespołu, są one bezpłatne ją skonfigurować w dowolny sposób, aby były najszybciej. Ponadto usługa Azure kontenera zapewni usługi będzie działać. Jeśli usługa nie powiedzie się, będzie Uruchom usługę kontenera Azure. W większości przypadków nie nawet widać, że znajdował się przestoje.

## <a name="prerequisites"></a>Wymagania wstępne

[Rozmieszczanie wystąpienie usługi kontenera Azure](container-service-deployment.md) za pomocą orchestrator typ kontrolera domeny-OS i [Upewnij się, że klient może połączyć się klaster](container-service-connect.md). Ponadto wykonaj następujące czynności.

[AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Tworzenie aplikacji lub usługi Marathon specyficzne dla użytkownika

Rozpocznij przez utworzenie pliku konfiguracji JSON, który definiuje nazwę usługi aplikacji, którą chcesz utworzyć. W tym miejscu firma Microsoft korzysta z `marathon-alice` jako nazwę framework. Zapisz plik jako podobną do `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Następnie użyć polecenie kontrolera domeny/systemu operacyjnego, aby zainstalować wystąpienie Marathon przy użyciu opcji, które są ustawione w pliku konfiguracji:

```bash
dcos package install --options=marathon-alice.json marathon
```

Powinien zostać wyświetlony z `marathon-alice` usługa działająca na karcie usługi kontrolera domeny i OS interfejsu użytkownika. Interfejs użytkownika zostanie `http://<hostname>/service/marathon-alice/` Jeśli chcesz uzyskać bezpośredni dostęp.

## <a name="set-the-dcos-cli-to-access-the-service"></a>Ustawianie polecenie kontrolera domeny/systemu operacyjnego, aby uzyskać dostęp do usługi

Opcjonalnie można skonfigurować usługi polecenie kontrolera domeny/systemu operacyjnego, aby uzyskać dostęp do tej nowej usługi, ustawiając `marathon.url` właściwości, aby wskazywały `marathon-alice` wystąpienia w następujący sposób:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Można sprawdzić, którego wystąpienie Marathon, czy usługi polecenie pracuje pod kątem zgodności z `dcos config show` polecenia. Można przywrócić przy użyciu usługi Marathon wzorcowej przy użyciu polecenia `dcos config unset marathon.url`.
