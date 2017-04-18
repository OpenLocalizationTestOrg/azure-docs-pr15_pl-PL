<properties
   pageTitle="Rejestrowanie w dzienniku analizy | Microsoft Azure"
   description="Internetowe usługi informacyjne (IIS) są przechowywane aktywności użytkownika w plikach dziennika, które mogą być zbierane przez analizy dziennika.  Ten artykuł zawiera opis sposobu konfigurowania zbieranie dzienników programu IIS i szczegóły rekordy, które tworzą w repozytorium usługi OMS."
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
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="iis-logs-in-log-analytics"></a>Rejestrowanie w dzienniku analizy
Internetowe usługi informacyjne (IIS) są przechowywane aktywności użytkownika w plikach dziennika, które mogą być zbierane przez analizy dziennika.  

![Dzienniki programu IIS](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Konfigurowanie usług IIS dzienniki
Dziennik analizy tworzy spis z pliki dziennika utworzone przez IIS, więc należy [skonfigurować do logowania](https://technet.microsoft.com/library/hh831775.aspx).

Analizy dziennika tylko obsługuje zapisane w formacie W3C plików dziennika programu IIS, a nie obsługuje pól niestandardowych lub Zaawansowane rejestrowanie programu IIS.  
Analizy dziennika nie zbieranie dzienników w formacie natywnych NCSA lub usług IIS.

Konfigurowanie usług IIS dzienniki w analizy dziennika z [menu dane w ustawieniach analizy dziennika](log-analytics-data-sources.md#configuring-data-sources).  Nie istnieje żadna konfiguracja wymagane inne niż wybieranie **format W3C zbieranie usług IIS pliki dziennika**.

Zaleca się, że po włączeniu zbieranie dzienników programu IIS, należy skonfigurować ustawienie najazd dziennika programu IIS na wszystkich serwerach.


## <a name="data-collection"></a>Zbieranie danych

Dziennik analizy zbiera wpisy dziennika usług IIS z każdego połączonego źródła, co 15 minut.  Agent rejestruje jego położenie w każdym dzienniku zdarzeń, zbierane z.  Jeśli agent przechodzi do trybu offline, następnie analizy dziennika gromadzi zdarzenia z miejsce, w którym go ostatnio przerwana, nawet jeśli te zdarzenia zostały utworzone podczas agent był w trybie offline.


## <a name="iis-log-record-properties"></a>Właściwości rekordu dziennika usług IIS

Rekordy dziennika programu IIS typu **W3CIISLog** i mają właściwości w poniższej tabeli:

| Właściwość | Opis |
|:--|:--|
| Komputer | Nazwa komputera, na którym zdarzenie zostało pobrane od. |
| cIP | Adres IP klienta. |
| csMethod | Metoda żądania, takich jak GET lub POST. |
| csReferer | Witryna, że użytkownik użyto łącza z bieżącej witryny. |
| csUserAgent | Typ przeglądarki klienta. |
| csUserName | Nazwa użytkownika uwierzytelnionego, który uzyskał dostęp do serwera. Użytkownikom anonimowym są oznaczone łącznik. |
| csUriStem | Cel żądania, takich jak strony sieci web. |
| csUriQuery | Kwerendy, jeśli istnieją, że klient próbował wykonać. |
| ManagementGroupName | Nazwa grupy zarządzania dla programu Operations Manager agentów.  W przypadku innych czynników jest AOI —\<identyfikator obszaru roboczego\> |
| RemoteIPCountry | Kraj, adres IP klienta. |
| RemoteIPLatitude | Szerokość adres IP klienta. |
| RemoteIPLongitude | Długość geograficzna adresu IP klienta. |
| scStatus | Kod stanu HTTP. |
| scSubStatus | Kod błędu podstanu. |
| scWin32Status | Kod stanu systemu Windows. |
| sIP | Adres IP serwera sieci web. |
| SourceSystem  | OpsMgr |
| sPort | Port na serwerze klienta połączony. |
| sSiteName | Nazwa witryny usług IIS. |
| TimeGenerated | Data i godzina zarejestrowania wpisu. |
| Właściwość TimeTaken | Okres żądania (w milisekundach). |

## <a name="log-searches-with-iis-logs"></a>Dziennik wyszukiwania za pomocą dzienników usług IIS

Poniższa tabela zawiera przykłady różnych dziennika kwerend, które pobierają rekordów dziennika programu IIS.

| Kwerendy | Opis |
|:--|:--|
| Typ = IISLog | Wszystkie rekordy dziennika programu IIS. |
| Typ = IISLog EventLevelName = błąd | Wszystkie zdarzenia systemu Windows z istotność błędów. |
| Typ = W3CIISLog & #124; Zastosowane count() cIP | Liczba programu IIS pozycje dziennika, adres IP klienta. |
| Typ = W3CIISLog csHost = "www.contoso.com" & #124; Zmierz count() przez csUriStem | Liczba programu IIS rejestrowanie pozycje przez adres URL www.contoso.com hosta. |
| Typ = W3CIISLog & #124; Zmierz Sum(csBytes) przez komputer i #124; góry 500000| Całkowitą liczbę bajtów odebranych przez każdy komputer usług IIS. |

## <a name="next-steps"></a>Następne kroki

- Konfigurowanie analizy dziennika do zbierania innych [źródeł danych](log-analytics-data-sources.md) na potrzeby analizy.
- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do analizowania danych zebranych ze źródła danych i rozwiązań.
- Konfigurowanie alertów w analizy dziennika wyprzedzeniem informujący o istotnych warunków znaleziony w dziennikach IIS.
