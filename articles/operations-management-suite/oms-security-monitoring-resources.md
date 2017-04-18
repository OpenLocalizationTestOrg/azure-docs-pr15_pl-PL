<properties
   pageTitle="Monitorowanie zasobów operacje zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji | Microsoft Azure"
   description="Ten dokument pomaga za pomocą usługi OMS zabezpieczeń i inspekcji możliwości monitorowania zasobów i identyfikowanie problemów z zabezpieczeniami."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Monitorowanie zasobów w operacje zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji

Ten dokument pomaga monitorować zasobów i identyfikowanie problemów z zabezpieczeniami za pomocą usługi OMS zabezpieczeń i możliwości inspekcji.

## <a name="what-is-oms"></a>Co to są usługi OMS?

Pakiet Microsoft operacje zarządzania usługi (OMS) jest podstawą rozwiązanie do zarządzania IT, który ułatwia zarządzanie i chronić swoje lokalnie i w chmurze infrastruktury chmury firmy Microsoft. Aby uzyskać więcej informacji na temat usługi OMS przeczytaj artykuł [Pakiet administracyjny operacji](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Monitorowanie zasobów

Zawsze, gdy jest to możliwe, można zapobiec bezpieczeństwem problemu w pierwszej kolejności. Istnieje możliwość zapobiec wszystkie zdarzenia zabezpieczeń. Gdy zdarzeniem bezpieczeństwa przeprowadzona, będzie konieczne upewnij się, że ich wpływu jest zminimalizowane.  Istnieją trzy zalecenia krytycznych, które mogą być używane w celu zminimalizowania numer i wpływu zabezpieczeń:

- Oceń regularnie luk w środowisku.
- Okresowe sprawdzanie wszystkich komputerów i urządzeń sieciowych do udostępnienia im wszystkie najnowsze poprawki zainstalowany.
- Okresowe sprawdzanie wszystkich dzienników i mechanizmy rejestrowania, takich jak dzienniki zdarzeń systemu operacyjnego, określonego Dzienniki aplikacji i dzienniki systemu wykrywania intruzów.

Zabezpieczenia usługi OMS i umożliwiają rozwiązanie inspekcji IT, aktywne monitorowanie wszystkie zasoby, które mogą ułatwić minimalizowanie wpływu zabezpieczeń. Zabezpieczenia usługi OMS i inspekcji ma domen zabezpieczeń, które mogą być używane do monitorowania zasobów. Domen zabezpieczeń zapewnia szybki dostęp do opcji zabezpieczeń monitorowania, które następujące domeny zostaną objęte więcej szczegółów:

- Oceny przed złośliwym oprogramowaniem
- Ocena aktualizacji
- Tożsamości i dostęp

> [AZURE.NOTE] Zawiera omówienie dotyczące tych opcji przeczytaj [Wprowadzenie do zabezpieczeń pakietu zarządzania operacje i rozwiązanie inspekcji](oms-security-getting-started.md).

### <a name="monitoring-system-protection"></a>Ochrona systemu monitorowania

W obrony programu głębokości każdej warstwie ochrona jest ważna w przypadku ogólnego stanu zabezpieczeń do środka. Na komputerach z wykryto zagrożeń i zabezpieczonych za mało są wyświetlane na danym kafelku oceny przed złośliwym oprogramowaniem, w obszarze zabezpieczenia domeny. Na podstawie informacji na temat oceny przed złośliwym oprogramowaniem, można określić plan dotyczą ochrony serwerów, które muszą ją. Aby uzyskać dostęp do tej opcji, wykonaj następujące czynności:

1. Na pulpicie nawigacyjnym głównym **Pakietu zarządzania operacje Microsoft** kliknij Kafelek **zabezpieczeń i inspekcji** .

    ![Zabezpieczenia i inspekcji](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. Pulpit nawigacyjny **zabezpieczeń i inspekcji** kliknij **Ocena ochrony przed złośliwym oprogramowaniem** w obszarze **Domen zabezpieczeń**. Pulpit nawigacyjny **Ocena ochrony przed złośliwym oprogramowaniem** pojawia się, jak pokazano poniżej:

![Oceny przed złośliwym oprogramowaniem](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

Pulpit nawigacyjny **Oceny przed złośliwym oprogramowaniem** służy do identyfikowania następujące zagadnienia dotyczące zabezpieczeń:

- **Aktywne zagrożeń**: komputerach naruszenia i że masz aktywne zagrożeń w systemie.
- **Zagrożenia Remediated**: komputerach naruszenia, ale zagrożeń zostały wyeliminować.
- **Podpis nieaktualny**: komputerach włączona ochrona przed złośliwym oprogramowaniem, ale podpis jest nieaktualny.
- **Nie ochrony w czasie rzeczywistym**: komputerach, które nie mają zainstalowanej ochrony przed złośliwym oprogramowaniem.

### <a name="monitoring-updates"></a>Monitorowanie aktualizacji 

Stosowanie najnowszych aktualizacji zabezpieczeń jest ze względów bezpieczeństwa i powinny być ujęte w strategii zarządzania aktualizacji. Usługa monitorowania agenta firmy Microsoft (HealthService.exe) odczytuje informacje o aktualizacji z komputerów monitorowane i przesyła zaktualizowane informacje do usługę w chmurze do przetwarzania. Usługa Microsoft Agent monitorowania jest skonfigurowana jako Usługa automatyczna i powinny być zawsze uruchomione na komputerze docelowym.

![monitorowanie aktualizacji](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Logika jest stosowana do aktualizowania danych i usług w chmurze rekordów danych. Jeśli znajdują się w brakujących aktualizacji, są one wyświetlane na pulpicie nawigacyjnym **aktualizacji** . Pulpit nawigacyjny **aktualizacje** służy do pracy z brakujących aktualizacji i opracowanie planu dotyczą serwerów, które są potrzebne. Wykonaj poniższe czynności, aby uzyskać dostęp do pulpitu nawigacyjnego **aktualizacji** :

1. Na pulpicie nawigacyjnym głównym **Pakietu zarządzania operacje Microsoft** kliknij Kafelek **zabezpieczeń i inspekcji** .
2. Pulpit nawigacyjny **zabezpieczeń i inspekcji** kliknij **Ocena aktualizacji** w obszarze **Domen zabezpieczeń**. Pulpit nawigacyjny aktualizacji pojawia się, jak pokazano poniżej:

![Ocena aktualizacji](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

W tym pulpitu nawigacyjnego można wykonywać ocenę aktualizacji do zrozumienia bieżący stan swoich komputerach i zajmowania najważniejszych zagrożeń. Przy użyciu kafelka **krytyczne lub aktualizacje zabezpieczeń** , pozwala administratorom systemów informatycznych będą mieli dostęp do szczegółowych informacji na temat aktualizacji, które nie są widoczne, jak pokazano poniżej:

![wynik wyszukiwania](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Ten raport zawierają informacje krytyczne, który może być używany do identyfikowania typu zagrożenia, które system występuje, zawierające w artykułach bazy wiedzy Microsoft KB skojarzone z aktualizacji zabezpieczeń i Bulletin MS, który zawiera szczegółowe informacje o luka.

### <a name="monitoring-identity-and-access"></a>Monitorowanie tożsamości i dostęp

Użytkownikom pracę z dowolnego miejsca, za pomocą różnych urządzeń i uzyskiwanie dostępu do dużych ilości aplikacje chmury i lokalnych konieczne jest ochronę swoje poświadczenia. Ataki Kradzieże poświadczenia są te, w których atakującej początkowo uzyskuje dostęp do zwykłego użytkownika poświadczenia, aby uzyskać dostęp do systemu w sieci. Wiele razy początkowe ataki jest tylko sposobem uzyskania dostępu do sieci, ultimate celem jest poznawanie uprawnień konta. 

Ataki pozostanie w sieci przy użyciu bezpłatnie narzędzie, aby wyodrębnić poświadczenia z sesji innych kont logowania. W zależności od konfiguracji systemu można wyjąć tych poświadczeń w postaci wartości skrótów, biletów lub nawet zwykłego tekstu hasła.  

> [AZURE.NOTE] komputery, które są dostępne bezpośrednio z Internetem zostanie wyświetlony wielu prób nie powiodło się, które spróbuj zalogować się przy użyciu wszystkich rodzaju dobrze znane nazwy użytkownika (na przykład Administrator). W większości przypadków jest OK Jeśli dobrze znane nazwy użytkownika nie są używane i będzie na tyle silne hasła.

Istnieje możliwość do identyfikowania tych intruzów przed wykraczają konto uprawnień. Można wykorzystać **zabezpieczeń usługi OMS i rozwiązanie inspekcji** monitorowanie tożsamości i access. Wykonaj poniższe czynności, aby uzyskać dostęp do pulpitu nawigacyjnego **tożsamości i Access** :

1. Na pulpicie nawigacyjnym głównym **Pakietu zarządzania Microsoft operacje** kliknij pozycję Zabezpieczenia i inspekcji kafelka.
2. Na pulpicie nawigacyjnym **zabezpieczeń i inspekcji** kliknij **tożsamości i dostęp** w obszarze **Domen zabezpieczeń**. Pulpit nawigacyjny **tożsamości i dostęp** pojawia się, jak pokazano poniżej:

![tożsamości i dostęp](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

W ramach strategii monitorowania zwykła musi zawierać monitorowanie tożsamości. Administrator powinien wyglądać po określonej prawidłowej nazwy użytkownika, który ma wiele prób. To może wskazać albo atakująca nabyte rzeczywistą nazwę użytkownika i próby atakami lub automatyczne narzędzie używa stałe hasło o wygaśnięciu.

Włączanie tego pulpitu nawigacyjnego IT szybkie identyfikowanie potencjalnych zagrożeń związanych z tożsamości i dostęp do zasobów firmy. Jest określony należy również zidentyfikować potencjalne tendencji, na przykład na kafelku logowania w czasie widać w okresie czasu, ile razy nieudanej próby logowania została wykonana. W tym przypadku komputera **SerwerPlików** odebrana 35 prób logowania. Zapoznaj się szczegółowe informacje o tym komputerze, klikając go. 

![więcej informacji](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Raport wygenerowany dla tego komputera powoduje przydatne szczegóły dotyczące tego wzorca. Należy zauważyć, że kolumny **kont** umożliwia konto użytkownika, którego użyto do próby uzyskania dostępu do systemu, kolumnie **TIMEGENERATED** przedstawiono przedział czasu, w którym zostało wykonane próba i kolumny **LOGONTYPENAME** udostępnia lokalizację, w której zostało wykonane próba. Jeśli takie próby były wykonywane lokalnie w systemie przez program, kolumnie **proces** czy z nazwą procesu. W sytuacjach, gdy próbę logowania pochodzi z programu masz już dostępnych nazwa procesu i teraz można wykonywać dalszego dochodzenia w systemie docelowym.

## <a name="see-also"></a>Zobacz też

W tym dokumencie, wiesz, jak używać usługi OMS zabezpieczeń i inspekcji rozwiązanie monitorowanie zasobów. Aby dowiedzieć się więcej na temat zabezpieczeń usługi OMS, zobacz następujące artykuły:

- [Przegląd operacji usługi zarządzania pakietu (OMS)](operations-management-suite-overview.md)
- [Wprowadzenie do zabezpieczeń pakietu zarządzania operacje i rozwiązanie inspekcji](oms-security-getting-started.md)
- [Monitorowanie i reagowanie na alerty zabezpieczeń w operacji zarządzania pakietu zabezpieczeń i rozwiązanie inspekcji](oms-security-responding-alerts.md)