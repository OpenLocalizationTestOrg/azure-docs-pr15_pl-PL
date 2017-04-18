<properties
   pageTitle="Publiczne i prywatne kontrolera domeny i OS agenta pul ACS | Microsoft Azure"
   description="Jak pul agenta publicznych i prywatnych działają z klastrem usługa Azure kontener."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontenery, Micro usług, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="timlt"/>

# <a name="dcos-agent-pools-for-azure-container-service"></a>Kontrolera domeny i OS agenta pul usługi Azure kontenera

Usługa kontenera Azure kontrolera domeny i OS dzieli agentów na pul publiczny czy prywatny. Wdrożenie mogą zostać wprowadzone do dowolnej puli, wpływu na dostępność między komputerami w usłudze kontener. Maszyny można udostępniana w Internecie (publiczna) lub przechowywanej w wewnętrznych (prywatnych). Ten artykuł zawiera krótkie omówienie, dlatego są publiczne i prywatne puli.

### <a name="private-agents"></a>Prywatne agentów

Uruchom agenta prywatne węzły za pośrednictwem sieci obsługi routingu. Tej sieci jest dostępny wyłącznie z strefy administratora lub za pośrednictwem routera krawędzi publicznej strefy. Domyślnie kontrolera domeny i OS uruchamianie aplikacji w węzłach prywatne agenta. Więcej informacji na temat zabezpieczeń sieci można znaleźć w [dokumentacji kontrolera domeny/systemu operacyjnego](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

### <a name="public-agents"></a>Publicznej czynników

Agent publicznej węzły uruchom kontrolera domeny i OS aplikacji i usług za pośrednictwem sieci publicznie. Więcej informacji na temat zabezpieczeń sieci można znaleźć w [dokumentacji kontrolera domeny/systemu operacyjnego](https://dcos.io/docs/1.7/administration/securing-your-cluster/) .

## <a name="using-agent-pools"></a>Za pomocą pul agenta

Domyślnie **Marathon** wdraża każdej nowej aplikacji *prywatne* węzły agenta. Musisz jawnie wdrożyć aplikację do węzła *publicznej* podczas tworzenia aplikacji. Wybierz kartę **opcjonalne** , a następnie wprowadź **slave_public** dla wartości **Zaakceptowane ról zasobów** . Ten proces został opisany [poniżej](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) i w dokumentacji [DC\OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) .

## <a name="next-steps"></a>Następne kroki

Przeczytaj więcej informacji o [zarządzaniu kontenerów kontrolera domeny/systemu operacyjnego](container-service-mesos-marathon-ui.md).

Dowiedz się, jak [otworzyć zapory](container-service-enable-public-access.md) dostarczony przez Azure umożliwia publiczny dostęp do swojego kontenera kontrolera domeny/systemu operacyjnego.