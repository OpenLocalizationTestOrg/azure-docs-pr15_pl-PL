<properties
   pageTitle="Nawiązywanie połączenia z klastrem usługa Azure kontenera | Microsoft Azure"
   description="Łączenie z klastrem usługa kontenera Azure za pomocą tunelem SSH."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontenery, Micro usług, kontrolera domeny i systemu operacyjnego, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>


# <a name="connect-to-an-azure-container-service-cluster"></a>Nawiązywanie połączenia z klastrem usługa Azure kontenera

Kontrolera domeny i system operacyjny i Docker Swarm klastrów wdrożonych przez usługę kontenera Azure uwidaczniają pozostałych punktów końcowych. Jednak te punkty końcowe nie są otwarte ze światem. Aby można było zarządzać te punkty końcowe, możesz utworzyć tunelem Secure Shell (SSH). Po SSH zostało ustanowione tunelem, można uruchomić polecenia przed punkty końcowe klaster i wyświetlić klaster interfejsu użytkownika za pośrednictwem przeglądarki sieci na własny system. Ten dokument przeprowadzi Cię przez proces tworzenia tunelem SSH z Linux, OS X i systemu Windows.

>[AZURE.NOTE] Możesz utworzyć sesję SSH w systemie zarządzania klaster. Jednak firma Microsoft nie jest to zalecane. Praca bezpośrednio w systemie zarządzania udostępnia ryzyka zmiany konfiguracji przypadkowe.   

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Tworzenie tunelem SSH na Linux lub OS X

Najpierw, jak podczas tworzenia tunelem SSH Linux lub OS X jest Znajdź nazwę DNS publicznej równoważenia obciążenia wzorców. Aby to zrobić, rozwiń grupy zasobów pojawia się każdego zasobu. Znajdź i zaznacz publiczny adres IP wzorca. Spowoduje to otwarcie w górę kartę, która zawiera informacje o publiczny adres IP, która zawiera nazwę DNS. Zapisywanie tej nazwy w celu późniejszego użycia. <br />


![Publiczną nazwę DNS](media/pubdns.png)

Teraz otwierania powłoki i uruchom następujące polecenie, gdzie:

**PORT** jest punktu końcowego, który chcesz udostępnić. W przypadku rój jest 2375. Dla kontrolera domeny i OS korzysta z portu 80.  
**Nazwa_użytkownika** to nazwa użytkownika, uzyskanych po wdrożeniu klaster.  
**DNSPREFIX** jest podana po wdrożeniu klaster prefiks DNS.  
**REGION** jest region, w którym znajduje się grupy zasobów.  
**PATH_TO_PRIVATE_KEY** [OPCJONALNY] jest ścieżką do klucz prywatny, który odpowiada klucz publiczny podana podczas tworzenia klaster usługi kontener. Użyj tej opcji z -i flagi.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> Port połączenia SSH jest 2200 — nie standardowego portu 22.

## <a name="dcos-tunnel"></a>Tunelem kontrolera domeny/systemu operacyjnego

Aby otworzyć tunelem do punktów końcowych związanych z kontrolera domeny/systemu operacyjnego, wydaj polecenie, który jest podobny do następującego:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Teraz masz dostęp kontrolera domeny/związane z systemem operacyjnym punkty końcowe w:

- KONTROLERA DOMENY/SYSTEMU OPERACYJNEGO:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Podobnie mogą skorzystać z pozostałych interfejsy API dla każdej aplikacji za pomocą tego tunelem.

## <a name="swarm-tunnel"></a>Tunelem rój

Aby otworzyć tunelem do punktu końcowego rój, wydaj polecenie, które wyglądają podobnie do następującej:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Teraz można ustawić do zmiennej środowiska DOCKER_HOST w następujący sposób. Możesz nadal korzystać z interfejsu wiersza polecenia Docker (polecenie) w zwykły sposób.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Tworzenie tunelem SSH w systemie Windows

Istnieje wiele opcji do tworzenia tuneli SSH w systemie Windows. Ten dokument będzie opisano, jak to zrobić przy użyciu Kit.

Pobierz Kit do systemu Windows i uruchomić aplikację.

Wprowadź nazwę hosta, która obejmuje klaster nazwy użytkownika i administratora publiczna nazwa DNS pierwszego wzorca w klastrze. **Nazwa hosta** będzie wyglądać następująco: `adminuser@PublicDNS`. Wprowadź 2200 **portu**.

![Konfiguracja Kit 1](media/putty1.png)

Wybierz pozycję **SSH** i **uwierzytelniania**. Dodawanie pliku kluczy prywatnych uwierzytelniania.

![Konfiguracja Kit 2](media/putty2.png)

Wybierz **tunele** i skonfigurować następujące elementy przesłane dalej porty:
- **Port źródłowy:** Preferencje — 80 do użycia w przypadku kontrolera domeny i OS lub 2375 dla rój.
- **Miejsce docelowe:** Używanie localhost:80 dla kontrolera domeny i OS lub localhost:2375 dla rój.

Poniższy przykład jest skonfigurowany do kontrolera domeny/systemu operacyjnego, ale będzie wyglądać podobnie do Docker Swarm.

>[AZURE.NOTE] Port 80 nie może być używany podczas tworzenia tej tunelem.

![Konfiguracja Kit 3](media/putty3.png)

Po zakończeniu zapisz konfiguracji połączenia i łączenie Kit sesji. Po nawiązaniu połączenia można zobaczyć konfigurację portu w dzienniku zdarzeń Kit.

![Kit dziennika zdarzeń](media/putty4.png)

Jeśli zostały skonfigurowane tunelem kontrolera domeny/systemu operacyjnego, uzyskiwania dostępu do powiązanych punkt końcowy w:

- KONTROLERA DOMENY/SYSTEMU OPERACYJNEGO:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Po skonfigurowaniu tunelem dla Docker Swarm, masz dostęp do klaster rój za pośrednictwem interfejsu wiersza polecenia Docker. Musisz najpierw skonfigurować zmiennej środowiska Windows o nazwie `DOCKER_HOST` o wartości ` :2375`.

## <a name="next-steps"></a>Następne kroki

Wdrażanie i zarządzanie nią kontenerów z kontrolera domeny i OS lub rój:

- [Praca z usługi Azure kontenera i kontrolera domeny/systemu operacyjnego](container-service-mesos-marathon-rest.md)
- [Praca z usługi Azure kontenera i rój Docker](container-service-docker-swarm.md)
