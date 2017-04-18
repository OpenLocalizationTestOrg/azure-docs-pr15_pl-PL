<properties
   pageTitle="Włączanie dostępu publicznego do aplikacji ACS | Microsoft Azure"
   description="Jak włączyć publicznego dostępu do usługi Azure kontener."
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
   ms.date="08/26/2016"
   ms.author="timlt"/>

# <a name="enable-public-access-to-an-azure-container-service-application"></a>Włączanie publicznego dostępu do aplikacji usługi kontenera Azure

Każdy kontener kontrolera domeny i OS ACS [puli publicznej agenta](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) automatycznie jest udostępniana w Internecie. Domyślne, porty **80**, **443** **8080** są otwarte, i dowolnego kontenera (publiczne) nasłuchują na te porty są dostępne. W tym artykule pokazano, jak otworzyć więcej portów dla aplikacji w usłudze Azure w kontenerze.

## <a name="open-a-port-portal"></a>Otwieranie portu (portal) 

Najpierw trzeba otworzyć portu, które będą.

1. Zaloguj się do portalu.
2. Znajdź grupę zasobów, do której wdrożyć usługę kontenera Azure.
3. Wybierz pozycję równoważenia obciążenia agenta (o nazwie podobne do **XXXX-agent kg XXXX**).

    ![Usługi równoważenia obciążenia usługi Azure kontenera](media/container-service-dcos-agents/agent-load-balancer.png)

4. Kliknij **sondy** , a następnie **Dodaj**.

    ![Sondy równoważenia obciążenia usługi Azure kontenera](media/container-service-dcos-agents/add-probe.png)

5. Wypełnij formularz sonda, a następnie kliknij **przycisk OK**.

  	| Pole | Opis |
  	| ----- | ----------- |
  	| Nazwa  | Opisowa nazwa sondy. |
  	| Port  | Port kontenera, aby przetestować. |
  	| Ścieżka  | (Gdy w trybie HTTP) Ścieżka względne witryny sieci Web do sondy. Protokół HTTPS nie są obsługiwane. |
  	| Interwał | Ilość czasu między sonda pozwala podjąć próbę w sekundach. |
  	| Nieprawidłowe progowa. | Liczbę następujących po sobie sondy prób przed uwzględnieniem kontenerze nieprawidłowe. | 
    

6. Ponownie we właściwościach Agenta równoważenia obciążenia kliknij pozycję **reguły równoważenia obciążenia** , a następnie **Dodaj**.

    ![Reguły równoważenia obciążenia usługi Azure kontenera](media/container-service-dcos-agents/add-balancer-rule.png)

7. Wypełnij formularz równoważenia obciążenia, a następnie kliknij **przycisk OK**.

  	| Pole | Opis |
  	| ----- | ----------- |
  	| Nazwa  | Opisowa nazwa usługi równoważenia obciążenia. |
  	| Port  | Publiczne port przychodzących. |
  	| Port wewnętrznej bazy danych | Port wewnętrzny publicznej kontenera, aby skierować ruch do. |
  	| Pula wewnętrznej bazy danych | Kontenery w tej puli będzie docelowej dla tej usługi równoważenia obciążenia. |
  	| Sonda | Sonda służące do ustalania, jeśli element docelowy w **puli wewnętrznej bazy danych** jest uszkodzony. |
  	| Utrzymywanie sesji | Określa sposób obsługi ruchu z klienta na czas trwania sesji.<br><br>**Brak**: kolejne żądania z tego samego klienta są obsługiwane przez dowolnego kontenera.<br>**IP klienta**: kolejne żądania z tym samym IP klienta są obsługiwane w tym samym kontenerze.<br>**IP klienta i protokół**: kolejne żądania z tym samym kombinacji IP i Protokół klienta są obsługiwane w tym samym kontenerze. |
  	| Limit czasu bezczynności | (Tylko TCP) W minutach czasu, aby zachować klienta TCP i HTTP Otwórz pominięciem *aktywności* wiadomości. |

## <a name="add-a-security-rule-portal"></a>Dodawanie reguły zabezpieczeń (portal)

Następnie należy dodać regułę zabezpieczeń, który kieruje ruch z naszych otwartego portu przez zaporę.

1. Zaloguj się do portalu.
2. Znajdź grupę zasobów, do której wdrożyć usługę kontenera Azure.
3. Zaznacz **publicznej** agenta sieci grupę zabezpieczeń (o nazwie podobne do **XXXX-agent publicznej nsg-XXXX**).

    ![Grupa zabezpieczeń sieci usługi Azure kontenera](media/container-service-dcos-agents/agent-nsg.png)

4. Wybierz pozycję **Zabezpieczenia reguły przychodzące** , a następnie **Dodaj**.

    ![Reguły grupy zabezpieczeń sieciowych usługi Azure kontenera](media/container-service-dcos-agents/add-firewall-rule.png)

5. Wypełnij reguły zapory, aby zezwolić na publicznej port i kliknij **przycisk OK**.

  	| Pole | Opis |
  	| ----- | ----------- |
  	| Nazwa  | Opisowa nazwa reguły zapory. |
  	| Priority (priorytet) | Pozycja priorytetu reguły. Im mniejsza liczba wyższa priorytet. |
  	| Źródła | Ograniczanie przychodzące zakres adresów IP, aby udzielić lub odmówić przez tę regułę. Za pomocą **dowolnego** określa ograniczenie. |
  	| Usługa | Wybierz zestaw wstępnie zdefiniowanych usług, których dotyczy ta reguła zabezpieczeń. W przeciwnym razie użyj **Niestandardowy** , aby utworzyć własne. |
  	| Protocol (protokół) | Ograniczyć ruch na podstawie **TCP** lub **UDP**. Za pomocą **dowolnego** określa ograniczenie. |
  	| Zakres portu | Jeśli **Usługa** jest **Niestandardowy**, określa zakres portów, których dotyczy ta reguła. Za pomocą pojedynczego portu, takich jak **80**lub zakres, takich jak **1024 1500**. |
  	| Akcja | Zezwalaj lub Odmów ruch, które spełniają kryteria. |

## <a name="next-steps"></a>Następne kroki

Informacje na temat różnicę między [agentami kontrolera domeny i OS publiczne i prywatne](container-service-dcos-agents.md).

Przeczytaj więcej informacji o [zarządzaniu kontenerów kontrolera domeny/systemu operacyjnego](container-service-mesos-marathon-ui.md).