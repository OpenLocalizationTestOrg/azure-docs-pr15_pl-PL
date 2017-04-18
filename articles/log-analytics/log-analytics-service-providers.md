<properties
    pageTitle="Logowanie funkcje analizy dla usługodawców | Microsoft Azure"
    description="Analizy dziennika może pomóc w zarządzaniu usługodawców (MSPs), dużych przedsiębiorstwach niezależnych dostawców oprogramowania (ISV) i dostawców usług hostingu serwerach klienta lokalnego lub infrastruktury chmury monitorowania i zarządzania nimi."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="richrund"/>

# <a name="log-analytics-features-for-service-providers"></a>Funkcje analizy dziennika dla usługodawców

Analizy dziennika może pomóc w zarządzanych usługodawców (MSPs), dużych przedsiębiorstwach, niezależnych dostawców oprogramowania (ISV) i dostawców usług hostingu serwerach klienta lokalnego lub infrastruktury chmury monitorowania i zarządzania nimi. 

Dużych przedsiębiorstwach udostępnić podobieństwa wielu usługodawców, szczególnie wówczas, gdy scentralizowane zespołu IT, który jest odpowiedzialny za zarządzanie IT dla wielu różnych jednostek biznesowych. Dla uproszczenia ten dokument używa terminów *usługodawcy* , ale te same funkcje jest również dostępne dla przedsiębiorstw i innych odbiorców.

## <a name="cloud-solution-provider"></a>Dostawcy rozwiązań chmury

Dla partnerów i dostawców usług, które są częścią programu [Chmury rozwiązanie dostawcy (dostawcy)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) analizy dziennika jest jednym z dostępnych dla subskrypcji dostawcy usług Azure. 

Analiz dziennika następujące funkcje są włączone w subskrypcjach *Dostawcy rozwiązań w chmurze* .

*Dostawcy rozwiązań chmury* można wykonywać następujące czynności:

+ Tworzenie obszarów roboczych analizy dziennika subskrypcji dzierżawy (klienta).
+ Obszary robocze programu Access, utworzone przez dzierżaw. 
+ Dodawanie i usuwanie dostępu użytkownika do obszaru roboczego przy użyciu zarządzania użytkownikami Azure. Gdy w obszarze roboczym dzierżawy w portalu usługi OMS Zarządzanie użytkownikami strony w obszarze Ustawienia nie jest dostępna
  - Analizy dziennika nie obsługuje dostępu oparta na rolach jeszcze — w określeniu użytkownika `reader` uprawnień w portalu Azure pozwala na wprowadzanie zmian w konfiguracji w portalu usługi OMS

Aby zalogować się do subskrypcji dzierżawy, musisz określić wartość Identyfikator dzierżawy. Identyfikator dzierżawy jest zazwyczaj ostatnim część adres e-mail służący do logowania się.

+ W portalu usługi OMS Dodaj `?tenant=contoso.com` w adresie URL portalu. Na przykład`mms.microsoft.com/?tenant=contoso.com`
+ W programie PowerShell, za pomocą `-Tenant contoso.com` parametru podczas korzystania z `Add-AzureRmAccount` polecenia cmdlet
+ Identyfikator dzierżawy są automatycznie dodawane, gdy używasz `OMS portal` łącze z portalu Azure, aby otworzyć, a następnie zaloguj się do portalu usługi OMS w wybranym obszarze roboczym

Jako *klienta* dostawcę rozwiązań chmurze, możesz wykonać następujące czynności:

+ Tworzenie dziennika analizy obszarów roboczych w subskrypcji dostawcy
+ Obszary robocze programu Access utworzone przez dostawcy
  -  Używanie `OMS portal` łącze z portalu Azure, aby otworzyć, a następnie zaloguj się do portalu usługi OMS w wybranym obszarze roboczym
+ Wyświetlanie i używanie stronie zarządzania użytkownikami w obszarze Ustawienia w portalu usługi OMS

>[AZURE.NOTE] Wykonywanie kopii zapasowych i odzyskiwanie witryny rozwiązań dla analizy dziennika nie są w stanie nawiązać połączenia z magazynu usługi odzyskiwania i nie można skonfigurować w subskrypcji dostawcy.

## <a name="managing-multiple-customers-using-log-analytics"></a>Zarządzanie wieloma klientami przy użyciu analizy dziennika 

Zaleca się tworzenie obszaru roboczego analizy dziennika dla każdego z klientów, którymi można zarządzać. Obszar roboczy analizy dziennika zawiera:

+ Lokalizację geograficzną przechowywania danych. 
+ Szczegółowy dla rozliczeń 
+ Izolacji danych 
+ Unikatowe konfiguracji

Tworzenie nowego obszaru roboczego na klienta, jesteś mógł zachować osobno dane poszczególnych klientów, a także śledzić zastosowania każdego z klientów.

Więcej informacji na temat kiedy i dlaczego utworzyć wiele obszarów roboczych opisanej w [zarządzanie dostępem do logowania analizy] (log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Tworzenie i konfigurowanie obszarów roboczych klienta można zautomatyzować przy użyciu [programu PowerShell](log-analytics-powershell-workspace-configuration.md), [Szablony Menedżera zasobów](log-analytics-template-workspace-configuration.md), lub za pomocą [Interfejsu API usługi REST](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/).

Korzystania z szablonów Menedżer konfiguracji, obszar roboczy umożliwia konfiguracji wzorca, który może być używany do tworzenia i konfigurowania obszarów roboczych. Można mieć pewność, jak obszary robocze są tworzone dla klientów, automatycznie skonfigurowane do własnych potrzeb. Gdy zaktualizujesz wymagań, szablon zostanie zaktualizowany i ponownie zastosować istniejących obszarów roboczych. Ten proces zapewnia, że nawet istniejących obszarów roboczych do nowych standardów.    

Podczas zarządzania wielu analizy dziennika obszary robocze, firma Microsoft zaleca integrowanie każdego obszaru roboczego z istniejącego systemu śledzenia biletów / konsoli operacji przy użyciu funkcji [alertów](log-analytics-alerts.md) . Integrowanie z istniejących systemów, pracowników pomocy technicznej nadal wykonaj ich znanych procesów. Analizy dziennika regularnie sprawdza każdego obszaru roboczego przed określonych kryteriów alertów i generuje alert, gdy jest wymagana akcji.

Dla raportów dla kierownictwa poziomu którego podsumowywanie danych wielu obszarów roboczych można użyć integracja między analizy dziennika i [PowerBI](log-analytics-powerbi.md). Jeśli potrzebujesz Integracja z innego systemu raportowania, w interfejsie API wyszukiwania (przy użyciu programu PowerShell lub [pozostałych](log-analytics-log-search-api.md)) umożliwia uruchamianie kwerend i eksportowanie wyników wyszukiwania.

## <a name="next-steps"></a>Następne kroki

+ Zautomatyzować tworzenie i konfigurowanie obszarów roboczych przy użyciu [szablonów Menedżera zasobów](log-analytics-template-workspace-configuration.md)
+ Zautomatyzować tworzenie obszarów roboczych przy użyciu [programu PowerShell](log-analytics-powershell-workspace-configuration.md) 
+ Korzystanie z [alertów](log-analytics-alerts.md) do zintegrować z istniejącymi systemami
+ Generowanie podsumowania raportu przy użyciu [PowerBI](log-analytics-powerbi.md)
