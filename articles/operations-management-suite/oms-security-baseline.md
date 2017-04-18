<properties
   pageTitle="Operacje zarządzania pakietu zabezpieczeń i inspekcji rozwiązanie według planu bazowego | Microsoft Azure"
   description="Ten dokument wyjaśniono, jak używać usługi OMS zabezpieczeń i inspekcji rozwiązanie do wykonania ocena według planu bazowego, wszystkich komputerów monitorowane w celu zabezpieczenia i zgodność."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>

# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Ocena według planu bazowego w operacji zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji

Ten dokument ułatwia uzyskiwanie dostępu do bezpieczny stan monitorowania zasobów za pomocą [operacji zarządzania pakietu usługi (OMS) zabezpieczeń i rozwiązanie inspekcji](operations-management-suite-overview.md) możliwości oceny według planu bazowego.

## <a name="what-is-baseline-assessment"></a>Co to jest plan bazowy oceny?

Microsoft, razem z organizacji branżowe i dla instytucji rządowych na całym świecie, określa konfiguracji systemu Windows, która reprezentuje wdrożeń wysoce bezpieczny serwer. Ta konfiguracja jest zestaw kluczach rejestru, ustawienia zasad inspekcji i ustawienia zasad zabezpieczeń oraz zalecane wartości tych ustawień firmy Microsoft. Ten zestaw reguł nosi nazwę zabezpieczeń według planu bazowego. Możliwości oceny według planu bazowego zabezpieczeń usługi OMS i inspekcji można bezproblemowo przeglądania swoich komputerach w celu zapewnienia zgodności. 

Istnieją trzy typy reguł:

- **Reguły rejestru**: Sprawdź, czy klucze rejestru są prawidłowo.
- **Inspekcji reguł**: reguły dotyczące zasad inspekcji.
- **Zabezpieczenia reguł**: reguły dotyczące uprawnień użytkownika na komputerze.

> [AZURE.NOTE] Przeczytaj [Użyj zabezpieczeń usługi OMS do oceny konfiguracji zabezpieczeń według planu bazowego](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) dla krótkie omówienie tej funkcji.

## <a name="security-baseline-assessment"></a>Ocena według planu bazowego zabezpieczeń

Zapoznanie się z bieżącej oceny według planu bazowego zabezpieczeń na wszystkich komputerach, które są monitorowane przez zabezpieczenia usługi OMS i inspekcji za pomocą pulpitu nawigacyjnego.  Wykonaj poniższe czynności, aby uzyskać dostęp do pulpitu nawigacyjnego ocena według planu bazowego zabezpieczeń:

1. Ten **Pakiet zarządzania operacje Microsoft** głównym pulpit nawigacyjny kliknij Kafelek **zabezpieczeń i inspekcji** .
2. Pulpit nawigacyjny **zabezpieczeń i inspekcji** kliknij **Ocena według planu bazowego** w obszarze **Domen zabezpieczeń**. Pulpit nawigacyjny **Ocena według planu bazowego zabezpieczeń** pojawia się, jak pokazano na poniższej ilustracji:
    
    ![Zabezpieczenia usługi OMS i inspekcji ocena według planu bazowego](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Ten pulpit nawigacyjny dzieli się w trzech głównych obszarach:

- **Komputery w porównaniu z planu bazowego**: w tej sekcji przedstawiono podsumowanie liczba komputerów, które są dostępne i procent komputerów, które przekazywane oceny. Górny 10 komputerów i wyniku procent również oferuje oceny.
- **Wymagany stan reguł**: Ta sekcja ma przeznaczenie, aby wyświetlić Sygnalizacja dostępności reguł nie powiodło się ważności i nie powiodło się reguły według typu. Wyświetlając do pierwszego wykresu można szybko ustalić w przypadku większości reguł nie powiodło się krytyczne, czy nie. Zawiera listę górny 10 reguły nie powiodło się i ich ważności. Drugi wykres zawiera typ reguły, która nie powiodła się podczas oceny. 
- **Komputery brakuje ocena według planu bazowego**: w tej sekcji listy komputerów, które nie były dostępne z powodu niezgodności systemu operacyjnego lub awarii. 

### <a name="accessing-computers-compared-to-baseline"></a>Dostęp do komputerów w porównaniu z planu bazowego

Najlepiej komputery są wszystkie spełniać zabezpieczeń oceny według planu bazowego. Jednak przewiduje się, że w niektórych okolicznościach to nie się dzieje. W ramach procesu zarządzania zabezpieczeń należy uwzględnić przeglądania komputerów, na których nie można przekazać wszystkie testy oceny zabezpieczeń. Szybki sposób wizualizacji, wybierając opcję **komputerów dostęp do** znajdującego się w sekcji **komputerów w porównaniu z planu bazowego** . Powinien zostać wyświetlony wynik wyszukiwania dziennika z listą komputerów, jak pokazano na poniższym obrazie:

![Dostęp do komputera wyników](./media/oms-security-baseline/oms-security-baseline-fig2.png)

Wyniki wyszukiwania są wyświetlane w formacie tabeli i miejsce, w którym pierwsza kolumna zawiera nazwę komputera i drugi kolor zawiera liczbę reguł, które nie powiodło się. Aby pobrać informacje dotyczące typu reguły, która nie powiodło się, kliknij w polu Liczba reguł nie powiodło się obok nazwy komputera. Powinien zostać wyświetlony wynik podobny do tego pokazano na poniższej ilustracji:

![Szczegóły wyników komputera uzyskać do nich dostęp](./media/oms-security-baseline/oms-security-baseline-fig3.png)

W tym wynik wyszukiwania znajduje się sumę używanych reguł, liczba krytyczne reguły, które nie powiodło się, reguły ostrzeżeń i reguł dotyczących informacji nie powiodło się.

### <a name="accessing-required-rules-status"></a>Uzyskiwanie dostępu do stanu wymagane reguł

Po uzyskaniu informacji dotyczących wartości procentowej liczba komputerów, które przekazywane oceny, można uzyskać więcej informacji na temat reguł występują według poziomów ich istotności. W tej wizualizacji ułatwia Wyznaczanie priorytetów komputery, które powinny być najpierw kierowane do upewnij się, że będą one zgodne z następnego oceny. Umieść wskaźnik myszy na krytycznych części wykresu znajduje się na kafelku **nie powiodło się reguły ważności** w obszarze **wymagane stanu reguły** , a następnie kliknij go. Powinien zostać wyświetlony następujący ekran podobny do wyniku:

![Nie powiodła się reguły szczegóły ważności](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

W wyniku tego dziennika został wyświetlony odpowiedni typ reguły według planu bazowego, która nie powiodło się, opis tej reguły, a identyfikator typowych konfiguracji wyliczenia (CCE) tej reguły zabezpieczeń. Następujące atrybuty powinny być wystarczające dla wykonywanie działań naprawczych, aby rozwiązać ten problem na komputerze docelowym.

> [AZURE.NOTE] Aby uzyskać więcej informacji na temat CCE dostęp do [Krajowych luka bazy danych](https://nvd.nist.gov/cce/index.cfm).

### <a name="accessing-computers-missing-baseline-assessment"></a>Uzyskiwanie dostępu do komputerów brakuje ocena według planu bazowego

Usługi OMS obsługuje według planu bazowego opinii domeny w systemie Windows Server 2008 R2 do systemu Windows Server 2012 R2. Windows Server 2016 według planu bazowego nie jest jeszcze ostateczne i zostaną dodane zaraz po opublikowaniu. Wszystkie inne systemy operacyjne zeskanowany za pośrednictwem usługi OMS zabezpieczeń i inspekcji ocena według planu bazowego jest wyświetlana w sekcji **komputerów Brak ocena według planu bazowego** .

## <a name="see-also"></a>Zobacz też

W tym dokumencie wiesz dotyczące zabezpieczeń usługi OMS i inspekcji ocena według planu bazowego. Aby dowiedzieć się więcej na temat zabezpieczeń usługi OMS, zobacz następujące artykuły:

- [Przegląd operacji usługi zarządzania pakietu (OMS)](operations-management-suite-overview.md)
- [Monitorowanie i reagowanie na alerty zabezpieczeń w operacji zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji](oms-security-responding-alerts.md)
- [Monitorowanie zasobów operacje zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji](oms-security-monitoring-resources.md)

