<properties
    pageTitle="Logowanie analizy — często zadawane pytania | Microsoft Azure"
    description="Odpowiedzi na często zadawane pytania dotyczące usług analiz dziennika."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-analytics-faq"></a>Dziennik analizy — często zadawane pytania

Ten FAQ Microsoft przedstawiono listę często zadawane pytania dotyczące analizy dziennika w usługi Microsoft operacje zarządzania pakietu (OMS). Jeśli masz dodatkowe pytania dotyczące analizy dziennika, przejdź do [forum dyskusyjne](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) i Publikuj pytania. Osoby z naszej społeczności pomoże Ci swojej odpowiedzi. Jeśli często zadawane jest pytanie, firma Microsoft będzie ją dodać do tego artykułu, aby można znaleźć szybko i łatwo.

## <a name="general"></a>Ogólne

**Pytanie: jakie są sprawdzane przez AD i ocena SQL rozwiązania?**

ODPOWIEDZI. Poniższe zapytanie zawiera opis wszystkie testy obecnie uruchomione:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Wyniki następnie można eksportować do programu Excel do dalszej recenzji.

* *P: dlaczego widzę inną niż *usługi OMS* Administration? ** w SCOM

O: w zależności od jakie aktualizacji zestawienie z SCOM jest włączony można zobaczyć węzeł *Advisor Centrum systemu*, *Operacyjne wniosków*lub *Analizy dziennika*.

Aktualizowanie ciągu tekstowego, aby *usługi OMS* znajduje się w pakiecie zarządzania, które są potrzebne do zaimportowania ręcznie. Postępuj zgodnie z instrukcjami na najnowszych artykuł z bazy wiedzy zestawienie aktualizacji SCOM i Odśwież konsoli usługi OMS, aby wyświetlić najnowsze aktualizacje w węźle *usługi OMS* .

* *P: czy istnieje *lokalnego* wersji OMS? **

O: nie. Dziennik analizy przetwarza i zapisuje bardzo dużych ilości danych. Jako usługi w chmurze analizy dziennika będzie mógł Skala up w razie potrzeby i uniknięcia wpływu wydajności w środowisku.

Ponadto jest sposób usługi cloud nie należy przechowywać infrastrukturę analizy dziennika i uruchomiony, a może otrzymywać aktualizacje funkcji częste i poprawki.

## <a name="configuration"></a>Konfiguracja
**Pytanie: można zmienić nazwę kontenera tabeli i obiektów blob odczytane z diagnostyki Azure (WAD)?**  

ODPOWIEDZI.  Nie, to nie jest obecnie możliwe, ale jest planowane w przyszłości.

**Adresy IP co pytanie: czy używanie usług usługi OMS? Jak zapewnić, że zaporę tylko zezwala na ruch z usługami usługi OMS?**  

ODPOWIEDZI. Usługa Analytics dziennika jest tworzona na bieżąco Azure i punkty końcowe odbierać adresy IP, które znajdują się w [Zakresów adresów IP centrum danych Microsoft Azure](http://www.microsoft.com/download/details.aspx?id=41653).

Wprowadzono wdrożenia usługi, zmienić rzeczywiste adresy IP usługi usługi OMS. Nazw DNS, aby umożliwić przez zaporę opisano na [Konfigurowanie ustawień serwera proxy i zapory w dzienniku analizy](log-analytics-proxy-firewall.md).

**Pytanie: ExpressRoute można używać do łączenia się z Azure. Moje ruch analizy dziennika użyje połączenie ExpressRoute?**  

ODPOWIEDZI. Różne rodzaje ruchu ExpressRoute opisane w [dokumentacji ExpressRoute](./expressroute/expressroute-faqs.md#supported-services).

Ruch do analizy dziennika używa elektrycznego ExpressRoute zaglądanie publicznej.

**Pytanie: jest proste i łatwe umożliwia przenoszenie istniejącego obszaru roboczego analizy dziennika do innej subskrypcji obszaru roboczego i Azure analizy dziennika?**  Mamy obszarów roboczych usługi OMS kilku klientów, które zostały możemy badania i trialing w naszym Azure subskrypcji, a są przenoszenia produkcji, chcemy przenieść je do własnych subskrypcji usługi Azure-OMS.  

ODPOWIEDZI. `Move-AzureRmResource` Polecenia cmdlet pozwala zmienić z obszarem roboczym analizy dziennika, a także konta automatyzacji jedną subskrypcję Azure. Aby uzyskać więcej informacji zobacz [Przenoszenie AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Ta zmiana może również w portalu Azure.

Nie można przenieść dane z jednego obszaru roboczego analizy dziennika ani zmieniać obszaru analizy dziennika dane są przechowywane w.

**P: jak dodać usługi OMS do SCOM?**

O: zaktualizować do pakietu najnowszych aktualizacji i importowania pakietów zarządzania pozwoli nawiązać SCOM analizy dziennika.

Należy zauważyć, że połączenie SCOM analizy dziennika tylko dostępne dla SCOM 2012 z dodatkiem SP1 lub nowszym.

**P: jak można potwierdzić, że agenta się komunikować się z dziennika analizy?**

O: aby upewnić się, że agent można komunikować się z usługi OMS, przejdź do: Panel sterowania, zabezpieczenia i ustawienia **Monitorowania agenta firmy Microsoft**.

Na karcie **Usługi Azure dziennika analizy (OMS)** Znajdź zielony znacznik wyboru. Ikona zielonego znacznika wyboru potwierdza, że agent jest komunikować się z usługę.

Żółta ikona ostrzeżenia oznacza, że agent występują problemy komunikacji z usługi OMS. Jeden typowe przyczyny jest usługę Microsoft monitorowania agenta został zatrzymany i konieczne jest ponowne uruchomienie.

**P: jak zatrzymać agenta komunikację z analiz dziennika?**

O: w SCOM, usunąć komputer z listy zarządzane usługi OMS. Powoduje zatrzymanie wszystkich komunikacji za pośrednictwem SCOM dla tego agenta. Dla czynników podłączone bezpośrednio do usługi OMS, możesz uniemożliwić ich komunikowania się z usługi OMS za pośrednictwem: Panel sterowania, zabezpieczenia i ustawienia **Monitorowania agenta firmy Microsoft**.
W obszarze **Usługi Azure dziennika analizy (OMS)**Usuń wszystkie obszary robocze na liście.

## <a name="agent-data"></a>Agent danych

**Pytanie: ilości danych można wysyłać agenta w celu analizy dziennika? Czy istnieje maksymalnej ilości danych dla klientów?**  
ODPOWIEDZI. Bezpłatne plan Ustawia dzienny zakończenie 500 MB na obszar roboczy. Plany standardowych i premium mieć bez limitu na ilości danych, które zostało przesłane. Jako usługi w chmurze, analizy dziennika w usługi OMS przeznaczona do automatycznie skali maksymalnie uchwyt wielkość pochodzące z klienta — nawet w przypadku terabajtów dziennie.

Agent analizy dziennika została zapewnienie, ma niewielkie rozmiary i czy pewien stopień kompresji podstawowych danych. Jedną z klientami faktycznie zapisano blogu z testów, które ich przeprowadzana naszych agenta i jak wrażeniem były. Wielkość danych zależy od rozwiązań udostępnia klientom. Można znaleźć szczegółowe informacje na temat wielkość danych i wyświetlić grupy przez rozwiązanie **zastosowania** kafelka na stronie Omówienie usługi OMS.

Aby uzyskać więcej informacji można znaleźć w [blogu klienta](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) o niskiej za agenta usługi OMS.

**Pytanie: jak przepustowości sieci jest używany przez agenta zarządzania firmy Microsoft (MMA) podczas wysyłania danych do analizy dziennika?**

ODPOWIEDZI. Dostępna przepustowość jest funkcją od ilości danych wysyłanych. Dane są kompresowane podczas przesyłania przez sieć.

**Pytanie: ilości danych są wysyłane na agenta?**

ODPOWIEDZI. To polega na:

- rozwiązania, które są włączone
- Liczba liczniki wydajności są zbierane i dzienników
- ilości danych w Dzienniku

Bezpłatne cennik warstwa jest dobrym sposobem na płycie kilka serwerów i Miernik głośności typowych danych. Ogólne zastosowania jest wyświetlana na stronie **Użycie** .
Dla komputerów, które mogą być uruchamiane agenta WireData możesz zobaczyć, ile danych jest wysyłana przy użyciu następującej kwerendy:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy dziennika](log-analytics-get-started.md) , aby dowiedzieć się więcej o analizy dziennika i uzyskiwanie pracę w minutach.
