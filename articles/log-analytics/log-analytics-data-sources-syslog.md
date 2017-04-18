<properties 
   pageTitle="SYSLOG wiadomości w dzienniku analizy | Microsoft Azure"
   description="SYSLOG jest protokołem rejestrowanie zdarzeń, należący do Linux.   Ten artykuł zawiera opis sposobu konfigurowania kolekcji Syslog wiadomości w analizy dziennika i szczegółowe informacje o rekordy, które tworzą w repozytorium usługi OMS."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />


# <a name="syslog-data-sources-in-log-analytics"></a>Źródła danych SYSLOG w analizy dziennika

SYSLOG jest protokołem rejestrowanie zdarzeń, należący do Linux.  Aplikacje będą wysyłać wiadomości, które mogą być przechowywane na komputerze lokalnym lub dostarczane do zbierających Syslog.  Po zainstalowaniu agenta usługi OMS Linux konfiguruje demona Syslog lokalne przesyłanie dalej wiadomości do agenta.  Agent wyśle wiadomość do analizy dziennika, której odpowiedni rekord jest tworzona w repozytorium usługi OMS.  

> [AZURE.NOTE]Dziennik analizy obsługuje kolekcji wiadomości wysłane przez rsyslog lub syslog ng. Demona syslog domyślne w wersji 5 wersję Red funkcję Enterprise Linux, CentOS i Oracle Linux (sysklog) nie jest obsługiwane dla zbioru zdarzeń syslog. Zbieranie danych syslog z tej wersji tych dystrybucji, należy zainstalować i skonfigurować Aby zamienić sysklog [demon rsyslog](http://rsyslog.com) .

![Kolekcja SYSLOG](media/log-analytics-data-sources-syslog/overview.png)


## <a name="configuring-syslog"></a>Konfigurowanie Syslog
Agent usługi OMS Linux są zbierane zdarzenia za pomocą urządzenia i severities, które są określane w konfiguracji.  Możesz skonfigurować Syslog za pośrednictwem portalu usługi OMS lub zarządzanie plikami konfiguracji usługi agentów Linux.


### <a name="configure-syslog-in-the-oms-portal"></a>Konfigurowanie Syslog w portalu usługi OMS

Konfigurowanie Syslog z [menu dane w ustawieniach analizy dziennika](log-analytics-data-sources.md#configuring-data-sources).  Ta konfiguracja jest dostarczana do pliku konfiguracji w każdego agenta Linux.

Możesz dodać nowych funkcji, wpisując jej nazwę i klikając polecenie **+**.  Dla każdego obiektu tylko wiadomości z wybranego severities zostanie pobrana.  Sprawdzanie severities dla określonego obiektu, który chcesz zebrać.  Nie zawiera wszelkie dodatkowe kryteria filtrowania wiadomości.

![Konfigurowanie Syslog](media/log-analytics-data-sources-syslog/configure.png)


Domyślnie wszystkie zmiany konfiguracji są automatycznie przenoszone do wszystkich agentów.  Jeśli chcesz ręcznie skonfigurować Syslog każdego agenta Linux, następnie wyczyść pole wyboru *Zastosuj poniżej konfiguracji na komputerach moje Linux*.


### <a name="configure-syslog-on-linux-agent"></a>Konfigurowanie Syslog Linux agenta

Gdy [agent usługi OMS jest zainstalowany na komputerze klienckim Linux](log-analytics-linux-agents.md), instaluje pliku konfiguracji syslog domyślny, który definiuje pomieszczenia i ważności wiadomości, które były zbierane.  Możesz zmienić ten plik, aby zmienić konfigurację.  Plik konfiguracyjny różni się w zależności od demona Syslog zainstalowanym przez klienta.

> [AZURE.NOTE] Jeśli edytujesz konfiguracji syslog, należy ponownie uruchomić demona syslog, aby zmiany zostały wprowadzone.

#### <a name="rsyslog"></a>rsyslog

Plik konfiguracji rsyslog znajduje się w **/etc/rsyslog.d/95-omsagent.conf**.  Poniżej przedstawiono domyślną zawartość.  To zbiera syslog wiadomości wysłanych z lokalnego agenta dla wszystkich urządzeń z poziomu ostrzeżenia lub nowszym.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Obiekt można usunąć przez usunięcie sekcji pliku konfiguracji.  Można ograniczyć severities, które są zbierane dla określonego obiektu, modyfikując wpis tej funkcji.  Na przykład aby ograniczyć możliwości użytkownika wiadomości z istotność błędów lub należy zmodyfikować go plik konfiguracyjny następujące czynności:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>SYSLOG ng

Plik konfiguracji rsyslog znajduje się w **/etc/syslog-ng/syslog-ng.conf**.  Poniżej przedstawiono domyślną zawartość.  To zbiera syslog wiadomości wysłanych z lokalnego agenta dla wszystkich urządzeń i wszystkie severities.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };
    
    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };
    
    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };
    
    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };
    
    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };
    
    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };
    
    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Obiekt można usunąć przez usunięcie sekcji pliku konfiguracji.  Można ograniczyć severities, które są zbierane dla określonego obiektu, usuwając ich ze swojej listy.  Na przykład aby ograniczyć możliwość użytkownika po prostu alertów i krytyczne wiadomości, należy zmodyfikować tej sekcji plik konfiguracyjny następujące czynności:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>Zmienianie portu Syslog

Agent usługi OMS odbiera wiadomości Syslog na lokalnym kliencie na porcie 25224.  Możesz zmienić tego portu, dodając następną sekcję do pliku konfiguracji agenta usługi OMS znajdującym się w **/etc/opt/microsoft/omsagent/conf/omsagent.conf**.  Zamień 25224 we wpisie **port** numer portu, który ma być.  Należy zauważyć, że będzie również należy zmodyfikować plik konfiguracji demona Syslog do wysyłania wiadomości do tego portu.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Zbieranie danych

Agent usługi OMS odbiera wiadomości Syslog na lokalnym kliencie na porcie 25224. Plik konfiguracji demona Syslog przekazuje Syslog wiadomości wysyłanych z aplikacji do tego portu, gdzie są zbierane przez analizy dziennika.


## <a name="syslog-record-properties"></a>SYSLOG właściwościach rekordu

Rekordy SYSLOG rodzaju **Syslog** i mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| Komputer | Komputer, który zdarzenie zostało pobrane od. |
| Pomieszczenia | Określa część systemu, który wygenerował wiadomości. |
| HostIP | Adres IP systemu wysłania wiadomości.  |
| Nazwa hosta | Nazwa systemu wysłania wiadomości. |
| SeverityLevel | Poziom ważności zdarzenia. |
| SyslogMessage | Tekst wiadomości. |
| Identyfikator procesu | Identyfikator procesu, który wygenerował wiadomości. |
| EventTime | Data i godzina wygenerowania zdarzenia.



## <a name="log-queries-with-syslog-records"></a>Dziennik kwerend z rekordami Syslog

Poniższa tabela zawiera przykłady różnych dziennika kwerend, które pobierają Syslog rekordów.

| Kwerendy | Opis |
|:--|:--|
| Typ = Syslog | Wszystkie Syslogs. |
| Typ = Syslog SeverityLevel = błąd | Wszystkie rekordy Syslog z istotność błędów. |
| Typ = Syslog & #124; Zmierz count() przez komputer | Liczba Syslog rekordów przez komputer. |
| Typ = Syslog & #124; Zmierz count() przez pomieszczenia | Liczba Syslog rekordów według funkcji. |

## <a name="next-steps"></a>Następne kroki

- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do analizowania danych zebranych ze źródła danych i rozwiązań. 
- Za pomocą [Pól niestandardowych](log-analytics-custom-fields.md) do analizowania danych pochodzących z rekordów syslog do poszczególnych pól.
- [Konfigurowanie Linux agentów](log-analytics-linux-agents.md) zbieranie innych typów danych. 
