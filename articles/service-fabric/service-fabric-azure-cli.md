<properties
   pageTitle="Interakcja z klastrów tkaninie usługi przy użyciu interfejsu wiersza polecenia | Microsoft Azure"
   description="Jak za pomocą interfejsu wiersza polecenia Azure interakcji z klastrem tkaninie usługi"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Za pomocą interfejsu wiersza polecenia Azure współdziałanie z klastrem tkaninie usługi

Możliwa jest interakcja z klastrem tkaninie usługi z komputerów Linux za pomocą interfejsu wiersza polecenia Azure na Linux.

Pierwszym krokiem jest Pobierz najnowszą wersję programu polecenie z przedstawicielem cyfra i ustawić ją w ścieżce przy użyciu następujących poleceń:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Dla każdego polecenia obsługuje, można wpisać nazwę polecenia, aby uzyskać pomoc dotyczącą tego polecenia. Automatyczne uzupełnianie jest obsługiwane dla poleceń. Na przykład następujące polecenie zapewnia pomocy dla wszystkich poleceń aplikacji. 

```sh
 azure servicefabric application 
```

Dodatkowo można filtrować pomoc do określonego polecenia, jak w poniższym przykładzie:

```sh
 azure servicefabric application  create
```

Aby włączyć automatyczne uzupełnianie w polecenie, uruchom następujące polecenia:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Następujące polecenia Połącz się z klastrem i wyświetlić węzły w klastrze:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Aby używać parametrów nazwanych i Dowiedz się, jakie są, możesz wpisać — pomoc po użyciu polecenia. Na przykład:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Podczas łączenia się z wielu stanowiska klaster z komputera **czyli nie jest częścią klaster**, użyj następującego polecenia:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Zamień PublicIPorFQDN tag rzeczywistą IP lub nazwy FQDN zależnie od potrzeb. Podczas łączenia się z wielu stanowiska klaster z komputera **to jest częścią klaster**, użyj następującego polecenia:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Można użyć programu PowerShell lub interfejsu wiersza polecenia do współdziałania z klaster tkaninie usługi Linux utworzone przez Azure portal. 

**Uwaga:** Tych klastrów nie są bezpieczne, w związku z tym możesz może być otwierany w górę do jednego pola, dodając publiczny adres IP w manifeście klaster.



## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Nawiązywanie połączenia z klastrem tkaninie usługi za pomocą interfejsu wiersza polecenia Azure

Następujące polecenia polecenie Azure przedstawiają sposób łączenia z klastrem bezpiecznego. Informacje dotyczące certyfikatu musi odpowiadać certyfikatu na przez klaster.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```
 
Jeśli certyfikatu ma urzędów certyfikacji (UC), należy dodać parametr — urzędu certyfikacji certyfikatu ścieżki, jak w następującym przykładzie: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Jeśli masz wiele urzędów certyfikacji, należy używać przecinka jako ogranicznik.
 
Jeśli nazwy typowych certyfikat nie ma odpowiednika punkt końcowy połączenia, można użyć parametru `--strict-ssl` do pominięcia weryfikacji, jak pokazano na następujące polecenie: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl false 
```
 
Jeśli chcesz przejść weryfikacji urzędu certyfikacji, można dodać — Brak autoryzacji Odrzuć parametr jak to przedstawiono w następujące polecenie: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized false 
```
 
Po połączeniu, należy uruchomić inne polecenia polecenie, aby wykonać czynność związaną z klastrem. 

## <a name="deploying-your-service-fabric-application"></a>Wdrażanie aplikacji tkaninie usługi

Wykonanie poniższych poleceń, aby kopiowanie, zapisywanie i uruchomić aplikację tkaninie usługi:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Uaktualnianie aplikacji

Proces jest podobny do [procesu w systemie Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Tworzenie, kopiowanie, rejestrowanie i utworzyć aplikację z projektu głównego katalogu. Jeśli wystąpienia aplikacja ma nazwę tkaninie: / MySFApp i typ jest MySFApp, polecenia będą następujące:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Wprowadź zmiany w aplikacji i odbudować usługę zmienione.  Aktualizowanie pliku manifestu zmienione usługi (ServiceManifest.xml) od zaktualizowanej wersji usługi (i kod lub konfiguracji lub dane, zależnie od potrzeb). Numer zaktualizowanej wersji dla aplikacji i usługi zmienione także modyfikować manifest aplikacji (ApplicationManifest.xml).  Teraz skopiuj i zarejestruj zaktualizowaną aplikację za pomocą następujących poleceń:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Teraz możesz rozpocząć uaktualnianie aplikacji przy użyciu następującego polecenia:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

Teraz można monitorować uaktualnienia aplikacji za pomocą SFX. W ciągu kilku minut aplikacja będzie zostały zaktualizowane.  Można także wypróbować zaktualizowaną aplikację ze względu na błąd i sprawdzić funkcja automatycznego wycofywania w tkaninie usługi.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="copying-of-the-application-package-does-not-succeed"></a>Kopiowanie pakietu aplikacji nie powiodła się

Sprawdź, czy `openssh` jest zainstalowany. Domyślnie Ubuntu pulpitu nie jest zainstalowany. Zainstaluj go, wykonując następujące polecenie:

```
 sudo apt-get install openssh-server openssh-client**
```

Jeśli problem będzie nadal występował, spróbuj wyłączyć PAM dla ssh, zmieniając plik **sshd_config** za pomocą następujących poleceń:

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Jeśli problem nadal występuje, spróbuj zwiększyć liczbę ssh sesje, wykonując następujące polecenia:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Używanie klawiszy ssh uwierzytelniania (zamiast hasła) jeszcze nie jest obsługiwane, (ponieważ platformie używa ssh do kopiowania pakietów), aby użyć uwierzytelniania hasła.


## <a name="next-steps"></a>Następne kroki

Konfigurowanie środowiska projektowego i wdrożyć aplikację usługi tkaninie do klastrów Linux.
