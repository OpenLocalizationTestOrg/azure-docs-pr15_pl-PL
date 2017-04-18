<properties
   pageTitle="Azure Zarządzanie kontenera kontenera usługi za pośrednictwem interfejsu użytkownika w sieci web | Microsoft Azure"
   description="Wdrażanie kontenerów Azure kontenera klaster usługi przy użyciu sieci web Marathon interfejsu użytkownika."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontenery, Micro usług, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/19/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-web-ui"></a>Zarządzanie kontenera za pośrednictwem interfejsu użytkownika w sieci web

Kontrolera domeny i OS zapewnia środowisko umożliwiające wdrażanie i skalowanie grupowany obciążenia podczas dostępnego używanego sprzętu. U góry kontrolera domeny/systemu operacyjnego ma struktury, która zarządza planowania i wykonywania obliczeń obciążenia.

Ramy są dostępne dla wielu popularnych obciążenie pracą, ten dokument, opisując, jak tworzyć i skalowanie wdrożeń kontenera z Marathon. Przed przystąpieniem do pracy z tych przykładów, należy klaster kontrolera domeny/systemu operacyjnego, który jest skonfigurowany w usłudze Azure w kontenerze. Można również muszą być połączenia zdalnego z tym klastrem. Aby uzyskać więcej informacji o tych elementów zobacz następujące artykuły:

- [Wdrażanie klastrze Usługa Azure kontenera](container-service-deployment.md)
- [Nawiązywanie połączenia z klastrem usługa Azure kontenera](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>Eksplorowanie kontrolera domeny i OS interfejsu użytkownika

Z ustanowioną tunelem Secure Shell (SSH) przejdź do http://localhost/. To ładowania web kontrolera domeny i OS interfejsu użytkownika i zawiera informacje o grupie, takich jak zasobów używanych, active czynników i programem services.

![INTERFEJS UŻYTKOWNIKA KONTROLERA DOMENY/SYSTEMU OPERACYJNEGO](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Eksplorowanie Marathon interfejsu użytkownika

Aby wyświetlić interfejs użytkownika Marathon, przejdź do http://localhost/Marathon. W tym oknie można rozpocząć nowego kontenera lub innej aplikacji w klastrze Azure kontenera usługi kontrolera domeny/systemu operacyjnego. Informacje o uruchamianiu kontenerów i aplikacjach również są widoczne.  

![Marathon interfejsu użytkownika](media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Wdrażanie kontenera sformatowanych przy użyciu Docker

Aby wdrożyć nowego kontenera przy użyciu Marathon, kliknij przycisk **Utwórz aplikację** i wprowadź następujące informacje w formularzu:

Pole           | Wartość
----------------|-----------
IDENTYFIKATOR              | nginx
Obraz           | nginx
Sieci         | Połączone
Port hosta       | 80
Protocol (protokół)        | PORT TCP

![Nowa aplikacja interfejsu użytkownika — ogólne](media/dcos/dcos4.png)

![Nowa aplikacja interfejsu użytkownika — Docker kontenera](media/dcos/dcos5.png)

![Nowa aplikacja interfejsu użytkownika — porty i protokoły odnajdowanie usługi](media/dcos/dcos6.png)

Statycznie mapować port kontenera do portu agent, należy użyć trybu JSON. Aby zrobić, przełącz się do **Trybu JSON** za pomocą przełącznika Kreatora nowej aplikacji. Następnie wprowadź następujące dane w obszarze `portMappings` sekcji definicji aplikacji. W tym przykładzie jest powiązana portu 80 kontenera portu 80 agenta kontrolera domeny/systemu operacyjnego. Po wprowadzeniu tej zmiany, możesz przełączać tego kreatora z trybu JSON.

```none
"hostPort": 80,
```

![Nowa aplikacja interfejsu użytkownika — przykład portu 80](media/dcos/dcos13.png)

Klaster kontrolera domeny/systemu operacyjnego jest wdrożona z zestawem czynników prywatne i publiczne. Klaster mogą uzyskiwać dostęp do aplikacji z Internetu należy zainstalować aplikacje agenta publicznej. Aby to zrobić, wybierz kartę **Opcjonalnie** Kreatora nowej aplikacji i wprowadź **slave_public** **Zaakceptowane ról zasobów**.

![Nowa aplikacja interfejsu użytkownika — publicznej agenta ustawienie](media/dcos/dcos14.png)

Ponownie na stronie głównej Marathon znajdują się stanu rozmieszczania kontenerze.

![Strona główna Marathon interfejsu użytkownika — stan wdrażania kontenera](media/dcos/dcos7.png)

Po przełączeniu się do sieci web kontrolera domeny i OS interfejsu użytkownika (http://localhost/), zostanie wyświetlony, czy zadania (w tym przypadku kontenera sformatowane Docker) jest uruchomiony w klastrze kontrolera domeny/systemu operacyjnego.

![Interfejs użytkownika — zadania uruchamiania w klastrze sieci web kontrolera domeny/systemu operacyjnego](media/dcos/dcos8.png)

Można również Zobacz węzła, uruchomionym zadania.

![Kontrolera domeny i OS interfejs użytkownika sieci web — węzła zadania](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Skalowanie kontenerów

Za pomocą interfejsu użytkownika Marathon przeskalować wystąpienia liczba kontenera. Aby to zrobić, przejdź do strony **Marathon** , wybierz pozycję kontener, w którym chcesz skalować i kliknij przycisk **Skala** . W oknie dialogowym **Skala aplikacji** wprowadź liczbę wystąpień kontenera, które mają, a następnie wybierz **Aplikację Skala**.

![Marathon interfejsu użytkownika — okno dialogowe Skala aplikacji](media/dcos/dcos10.png)

Po zakończeniu działania skali zostanie wyświetlona wielu wystąpień tego samego zadania rozciągnąć kontrolera domeny i OS agentów.

![Pulpit nawigacyjny interfejsu użytkownika web kontrolera domeny i OS — zadań rozpowszechniania przez agentów](media/dcos/dcos11.png)

![Interfejs użytkownika — węzły sieci web kontrolera domeny/systemu operacyjnego](media/dcos/dcos12.png)

## <a name="next-steps"></a>Następne kroki

- [Praca z kontrolera domeny/systemu operacyjnego i interfejsu API Marathon](container-service-mesos-marathon-rest.md)

Głębokości dive w usłudze Azure kontenera z Mesos

> [AZURE. Azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos wideo]]
