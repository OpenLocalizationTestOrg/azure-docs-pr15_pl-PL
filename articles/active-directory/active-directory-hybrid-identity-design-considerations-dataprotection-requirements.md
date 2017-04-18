<properties
    pageTitle="Azure Active Directory hybrydowych tożsamości zagadnienia projektowe - określenie wymagań dotyczących ochrony danych | Microsoft Azure"
    description="Podczas planowania rozwiązania hybrydowego tożsamości i identyfikowanie wymagań ochrony danych dla swojej firmy i jakie opcje są dostępne dla najlepiej spełnia tych wymagań."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

#<a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Planowanie dotyczące zwiększania zabezpieczeń danych za pośrednictwem rozwiązanie silnych tożsamości

Pierwszy krok do ochrony danych jest określenie, kto może uzyskiwać dostęp do danych i w ramach tego procesu, trzeba mieć tożsamości rozwiązanie, które można zintegrować z systemu w celu zapewnienia możliwości uwierzytelniania i autoryzacji. Uwierzytelniania i autoryzacji często mylić ze sobą, a ich role błędnie interpretowane. W rzeczywistości są bardzo różne, jak pokazano na poniższej ilustracji:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)
 
**Etapów cyklu życia zarządzania urządzenia przenośnego**

Podczas planowania rozwiązania tożsamości hybrydowego należy zrozumieć wymagania dotyczące ochrony danych dla swojej firmy i jakie opcje są dostępne dla najlepiej spełnia tych wymagań.
 
>[AZURE.NOTE]
Po zakończeniu planowania zabezpieczania danych, przejrzyj [określenie wymagań uwierzytelnianie wieloskładnikowe](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) , aby upewnić się, że odpowiednie opcje dotyczące wymagań uwierzytelnianie wieloskładnikowe zostały nie dotyczy decyzji, wprowadzone w tej sekcji.

## <a name="determine-data-protection-requirements"></a>Określanie wymagania dotyczące ochrony danych
W wieku mobilności większość firm mają wspólne cel: umożliwić użytkownikom wydajną pracę na swoich urządzeniach przenośnych podczas lokalnego lub zdalnie z dowolnego miejsca w celu zwiększenia wydajności. Może to być wspólnego celu firmami, których takie wymaganie będzie również dotyczą dotyczące ilości zagrożenia, które musi być ograniczona w celu zabezpieczenia danych firmy i zachować prywatność użytkownika. Każdej firmy mogą mieć inne wymagania w tym zakresie; zgodność różnych reguł, które różnią się zgodnie z jakie branżowe działa firma będzie powodowało inny projekt decyzji. 

Istnieją jednak pewne zabezpieczeń zagadnienia powinny być zbadane i zweryfikowanym, bez względu na to branżowych, które zostały omówione w następnej sekcji.

## <a name="data-protection-paths"></a>Ścieżek ochrona danych

![](./media/hybrid-id-design-considerations/data-protection-paths.png)
 
**Ścieżek ochrona danych**

Na powyższym wykresie składnik tożsamości będzie pierwsza do zostać zweryfikowane, aby uzyskiwać dostęp do danych. Jednak tych danych może być w różne stany w czasie, który uzyskał dostęp. Każdy numer na tym diagramie oznacza ścieżkę, w którym dane mogą być umieszczane w pewnym momencie w czasie. Poniżej opisano te liczby:

1. Ochrona danych na poziomie urządzenia.
2. Ochrona danych znajduje się w drodze.
3. Ochrona danych przy jednoczesnym pozostałych lokalnego.
4. Ochrona danych w stanie spoczynku w chmurze.

Mimo że techniczne Określa, że będzie Włącz IT ochrony dane o każdym z tych etapów nie są bezpośrednio oferowane przez rozwiązanie tożsamości hybrydowe, jest to konieczne, rozwiązanie tożsamości hybrydowego jest w stanie używanie zarówno w lokalnej, jak i w chmurze tożsamości zarządzania zasobami do identyfikowania użytkownika przed udzielić dostępu do danych. Podczas planowania rozwiązania hybrydowego tożsamości upewnij się, że zgodnie z wymaganiami organizacji są odpowiedzieć na następujące pytania:

## <a name="data-protection-at-rest"></a>Ochrona danych spoczynku
Bez względu na miejsce, w którym dane są w stanie spoczynku (urządzenia, chmury lub lokalnego) należy przeprowadzić ocenę zrozumienie potrzeb organizacji w tym zakresie. Dla tego obszaru upewnij się, że monit następujące pytania:

- Czy firma należy chronić dane na pozostałych?
 - Jeśli tak, jest rozwiązanie tożsamości hybrydowego można zintegrować z bieżącej infrastruktury lokalnego?
 - Jeśli tak, jest rozwiązanie tożsamości hybrydowego można zintegrować z Twojej obciążenia znajduje się w chmurze?
- Zarządzanie tożsamościami chmurze jest możliwość ochrony poświadczenia użytkownika i inne dane przechowywane w chmurze?

## <a name="data-protection-in-transit"></a>Ochrona danych podczas przesyłania
Dane w drodze między urządzeniem i centrum danych lub urządzenie i chmury muszą być chronione. Jednak są w drodze musi oznaczać procesu komunikacji ze składnikiem poza usługi cloud; przejdzie ona wewnętrznie, również, na przykład między dwoma wirtualnych sieci. Dla tego obszaru upewnij się, że monit następujące pytania:

- Czy firma należy ochrony danych w drodze?
 - Jeśli tak, jest rozwiązanie tożsamości hybrydowego można zintegrować z bezpiecznego kontrolek, takich jak SSL/TLS?
- Zarządzanie tożsamościami chmury zachować ruchu do i w magazynie katalogu (wewnątrz i między centrami danych) podpisane?


## <a name="compliance"></a>Zgodność
Przepisów prawa i wymagania dotyczące zgodności z przepisami zależy od branży, należący do Twojej firmy. Firmy w branży wysoka regulowanych Musisz zająć się Zarządzanie tożsamościami kwestie związane z problemy ze zgodnością. Przepisów, takich jak ustawą Sarbanes-Oxley (ustawą Sarbanes-OXLEY), przenośność ubezpieczenia zdrowia i odpowiedzialności Act (HIPAA), Act Gramm-wypłukującej-Bliley (GLBA) i płatności karty branżowe danych zabezpieczeń standardowy (PCI DSS) są bardzo ściśle dotyczące tożsamości i access. Rozwiązanie tożsamości hybrydowe, które przyjmie firmy musi mieć możliwości podstawowych, które spełniają wymagania co najmniej jedną z tych przepisów. Dla tego obszaru upewnij się, że monit następujące pytania:

- Rozwiązanie tożsamości hybrydowego jest zgodne z przepisami dla swojej firmy?
- Czy rozwiązanie tożsamości hybrydowych wbudowanej w możliwości, które umożliwi firmie być zgodne z przepisami? 
 
>[AZURE.NOTE]
Pamiętaj sporządzać notatki każdej odpowiedzi i opis uzasadnienie odpowiedź. [Definiowanie strategię ochrony danych](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) będą wyświetlane na przycisku Opcje dostępne i zalety i wady poszczególnych opcji.  Przez o odpowiedzi na te pytania, która zostanie wybrana wybranej opcji najlepiej pasuje Twoja firma wymaga.

## <a name="next-steps"></a>Następne kroki
 [Określenie wymagań dotyczących zarządzania zawartością](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)


## <a name="see-also"></a>Zobacz też
[Omówienie zagadnienia dotyczące projektowania](active-directory-hybrid-identity-design-considerations-overview.md)
