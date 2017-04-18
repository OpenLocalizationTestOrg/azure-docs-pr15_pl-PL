<properties
   pageTitle="Brama danych lokalnych | Microsoft Azure"
   description="Brama w środowisku lokalnym jest to konieczne, jeśli serwer usług Analysis Services w Azure połączy się lokalnych źródeł danych."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="on-premises-data-gateway"></a>Brama dane lokalne

Brama dane lokalne pełni rolę mostka, dostarczając bezpiecznego transferu danych między lokalnych źródeł danych i serwer usług Analysis Services Azure w chmurze.

Brama jest zainstalowany na komputerze w sieci. Dla każdego serwera Azure usług Analysis Services, który jest w ramach subskrypcji Azure musi być zainstalowany jednej bramy. Na przykład jeśli masz dwa serwery w ramach subskrypcji Azure, które połączenia ze źródłami danych w lokalnym bramę musi być zainstalowany na dwóch różnych komputerach w sieci.

## <a name="requirements"></a>Wymagania dotyczące

**Minimalne wymagania:**

- 4,5 .NET framework
- 64-bitowej wersji systemu Windows 7 i Windows Server 2008 R2 (lub nowszy)

**Zalecane:**

- Procesor 8 core
- 8 GB pamięci RAM
- 64-bitowej wersji systemu Windows 2012 R2 (lub nowszy)

**Ważne zagadnienia:**

- Brama nie można zainstalować na kontrolerze domeny.

- Na jednym komputerze można zainstalować tylko jednej bramy.

- Zainstalować bramy na komputerze, na którym pozostają na i nie przejdź do wstrzymania. Jeśli komputer nie jest włączony, serwer usług Analysis Services Azure nie można połączyć źródła danych lokalnych, aby odświeżyć dane.

- Nie można zainstalować bramy na komputerze, bezprzewodowo połączeni z siecią. Wydajność może być mniejsza.

- Aby zmienić nazwę serwera dla bramy, która została już skonfigurowana, musisz ponownie zainstalować i skonfigurować Nowa brama.

- W niektórych przypadkach modeli tabelarycznych połączenia ze źródłami danych za pomocą natywnego dostawców, takich jak program SQL Server Native Client (SQLNCLI11) może zostać zwrócony błąd. Aby uzyskać więcej informacji, zobacz [połączenia źródła danych](analysis-services-datasource.md).

## <a name="supported-on-premises-data-sources"></a>Obsługiwane lokalnego źródła danych
W polu Podgląd bramy obsługuje połączenia między serwer usług Analysis Services Azure i następujące lokalnych źródeł danych:

- Program SQL Server
- Magazyn danych SQL
- DOSTĘPU
- Oracle
- Teradata


## <a name="download"></a>Plik do pobrania
 [Pobierz bramy](https://aka.ms/azureasgateway)


## <a name="install-and-configure"></a>Instalowanie i konfigurowanie

1. Uruchom Instalatora.

2. Wybieranie lokalizacji instalacji i zaakceptuj postanowienia licencyjne.

3. Zaloguj się do Azure.

4. Określ nazwę serwera analizy Azure. Można określić tylko jeden serwer na bramy. Kliknij przycisk **Konfiguruj** i kontynuować.

    ![Zaloguj się do azure](./media\analysis-services-gateway\aas-gateway-configure-server.png)


## <a name="how-it-works"></a>Jak to działa
Brama działa jako usługa systemu Windows, **bramy danych lokalnych**, na komputerze w sieci Twojej organizacji. Brama instalowanych przez Azure usług Analysis Services jest oparty na tej samej bramie na potrzeby innych usług, takich jak usługi Power BI, ale z pewne różnice w tym kroku jest skonfigurowany.

![Jak to działa](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Kwerendy danych przepływu pracy i tak jak poniżej:

1.  Kwerenda jest tworzona przez usług w chmurze z szyfrowanymi poświadczeniami dla lokalnego źródła danych. Następnie jest wysyłana do kolejki bramy przetwarzania.

2.  Usługa w chmurze brama analizuje kwerendy i umieszcza żądania [Bus usługi Azure](https://azure.microsoft.com/documentation/services/service-bus/).

3.  Brama danych lokalnych sprawdza Bus usługi Azure dla oczekujących żądań.

4.  Brama otrzymuje kwerendę, odszyfrowuje poświadczenie za i łączy do źródeł danych za pomocą tych poświadczeń.

5.  Brama wysyła kwerendę w źródle danych do wykonania.

6.  Powrót do bramy, a następnie na usługi w chmurze, wyniki są wysyłane ze źródła danych.

## <a name="windows-service-account"></a>Konto usługi systemu Windows

Brama danych lokalnych jest skonfigurowany do używania *NT SERVICE\PBIEgwService* dla poświadczenia logowania usługi systemu Windows. Domyślnie ma po prawej stronie logowania usługi; w kontekście instalowanego bramy na komputerze. Poświadczenia nie jest to samo konto używane do łączenia się ze źródłami danych lokalnych lub konto Azure.  

Jeśli występują problemy z serwerem proxy z powodu uwierzytelniania, warto zmienić konto usługi Windows użytkownikowi domeny lub zarządzany konta usługi.

## <a name="ports"></a>Porty

Brama tworzy połączenia wychodzące do Bus usługi Azure. Komunikuje na porty wyjściowe: port TCP 443 (ustawienie domyślne), 5671, 5672, 9350 za pośrednictwem 9354.  Bramy nie wymaga porty przychodzące.

Zalecamy możesz listy sprawdzonej adresów IP dla danego regionu danych w zaporze. Możesz pobrać [listę adresów IP centrum danych Azure firmy Microsoft](https://www.microsoft.com/download/details.aspx?id=41653). Ta lista jest aktualizowana co tydzień.

> [AZURE.NOTE]  Adresy IP wymienione na liście IP centrum danych Azure są w notacji CIDR. Na przykład 10.0.0.0/24 nie oznacza 10.0.0.0 za pośrednictwem 10.0.0.24. Dowiedz się więcej na temat [notacji CIDR](http://whatismyipaddress.com/cidr).

Poniżej przedstawiono nazwy FQDN używane przez bramę.

|Nazwy domen|Porty wyjściowe|Opis|
|---|---|---|
|*. powerbi.com|80|HTTP używany do pobierania Instalatora.|
|*. powerbi.com|443|PROTOKÓŁ HTTPS|
|*. analysis.windows.net|443|PROTOKÓŁ HTTPS|
|*. login.windows.net|443|PROTOKÓŁ HTTPS|
|*. servicebus.windows.net|5671 5672|Zaawansowane kolejkowanie Protocol (AMQP)|
|*. servicebus.windows.net|443, 9350 9354|Detektory na usługę Bus przekazywania przez TCP (wymaga 443 uzyskania tokenu kontrola dostępu)|
|*. frontend.clouddatahub.net|443|PROTOKÓŁ HTTPS|
|*. core.windows.net|443|PROTOKÓŁ HTTPS|
|Login.microsoftonline.com|443|PROTOKÓŁ HTTPS|
|*. msftncsi.com|443|Służy do sprawdzenia łączność z Internetem, czy brama jest nieosiągalny przez usługę Power BI.|
|*.microsoftonline p.com|443|Używany do uwierzytelniania w zależności od konfiguracji.|


### <a name="forcing-https-communication-with-azure-service-bus"></a>Wymuszanie HTTPS komunikacji z Bus usługi Azure

Można wymusić bramy na komunikowanie się przy użyciu protokołu HTTPS zamiast bezpośredniego TCP; Bus usługi Azure Jednak to znacznie zmniejszyć wydajność. Należy zmodyfikować plik *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* . Zmień wartość z `AutoDetect` do `Https`. Ten plik znajduje się domyślnie w *lokalnej Files\On C:\Program danych bramy*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>Rozwiązywanie problemów
W obszarze Ustawienia zaawansowane brama danych lokalnych, używane do łączenia do lokalnych źródeł danych usług Analysis Services Azure jest tej samej bramie używane dzięki usłudze Power BI.

Jeśli masz problemy podczas instalowania i konfigurowania bramy, pamiętaj, że zobacz [Rozwiązywanie problemów z bramy Power BI](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/). Jeśli uważasz, że masz problem z zapory, zobacz sekcje zapory lub serwera proxy.

Jeśli uważasz, że masz występują problemy z serwera proxy, bramy, zobacz [Konfigurowanie ustawień serwera proxy dla bramy Power BI](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md).

## <a name="next-steps"></a>Następne kroki
- [Zarządzanie usług Analysis Services](analysis-services-manage.md)
- [Pobieranie danych z usług Analysis Services Azure](analysis-services-connect.md)
