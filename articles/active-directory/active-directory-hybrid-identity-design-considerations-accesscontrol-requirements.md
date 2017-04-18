
<properties
    pageTitle="Azure Active Directory hybrydowych tożsamości zagadnienia projektowe - określanie wymagań dotyczących kontroli dostępu | Microsoft Azure"
    description="Obejmuje tożsamości i identyfikujący typ przepływu pracy programu access wymagań dotyczących zasobów dla użytkowników w środowisku hybrydowym programu."
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

# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Określanie wymagania dotyczące rozwiązania hybrydowego tożsamości kontroli dostępu
Projektując organizacji jest ich rozwiązanie tożsamości hybrydowego można też szansy sprzedaży do programu access wymagania dotyczące zasobów, które jest planowane był dostępny dla użytkowników. Dostęp do danych granic wszystkie cztery grupy tożsamości, które są:

- Administracja
- Uwierzytelnianie
- Autoryzacja
- Inspekcja

W poniższych sekcjach poniżej będzie obejmować uwierzytelniania i autoryzacji bardziej szczegółowe, Administracja i inspekcja należą do zarządzania cyklem tożsamości hybrydowego. Aby uzyskać więcej informacji na temat tych funkcji, przeczytaj [zadania związane z zarządzaniem Określanie hybrydowych tożsamości](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) .

>[AZURE.NOTE]
W tym artykule [Cztery grupy tożsamości — Zarządzanie tożsamościami w wieku hybrydowych się](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) uzyskać więcej informacji o każdej z nich tych słupków.

## <a name="authentication-and-authorization"></a>Uwierzytelnianie i autoryzacja
Istnieją różne scenariusze uwierzytelniania i autoryzacji, sytuacje będą mieć szczegółowe wymagania, które muszą być spełnione przy rozwiązanie tożsamości hybrydowe, które ma przyjąć firmy. Scenariusze dotyczące komunikacji (B2B) firma i firma można dodać dodatkowe wezwanie dla administratorów IT, ponieważ musisz upewnij się, że używane przez organizację metodę uwierzytelniania i autoryzacji można komunikować się z jej partnerów biznesowych. W trakcie procesu projektowania wymagania dotyczące uwierzytelniania i autoryzacji upewnij się, że należy odpowiedzieć na następujące pytania:

- Będzie organizacji uwierzytelniania i autoryzacji tylko użytkownicy znajdujący się w ich systemu zarządzania tożsamości?
 - Czy jest planowane scenariuszach B2B?
 - Jeśli tak, znasz już protokołów (SAML, OAuth, Kerberos, tokeny lub certyfikaty) są używane do łączenia obu firmy?
- Czy rozwiązanie tożsamości hybrydowe, które mają zostać przyjęcie pomocy technicznej tych protokołów?

Innym ważnym brać pod uwagę to miejsce, w którym będą umieszczane repozytorium uwierzytelniania, używany przez użytkowników i partnerów i modelu administracyjnego ma być używany. Należy rozważyć następujące opcje dwa podstawowe:
- Scentralizowane: w tym modelu poświadczeń użytkownika, zasady i administracji może być scentralizowane lokalnego lub w chmurze.
- Hybrydowe: w tym modelu poświadczeń użytkownika, zasady i administracji będzie scentralizowane lokalnego i zreplikowanej w chmurze.

Model, który przyjmie organizacji różnią się zgodnie z ich wymagań, który chcesz odpowiedzieć na następujące pytania określ miejsce, w którym znajdują się systemu zarządzania tożsamości i tryb administracyjny umożliwia:

- Twoja organizacja ma obecnie Zarządzanie tożsamościami lokalnego?
 - Jeśli tak, czy jest planowana zachowaniu go?
 - Czy istnieją wymagań wykonawczych i zgodności, że organizacji należy wykonać tego ile wymagają tego warunki miejsce, w którym powinien znajdować się systemu zarządzania tożsamości?
- Twoja organizacja używa rejestracji jednokrotnej dla aplikacji znajdującego się w lokalnym lub w chmurze?
 - Jeśli tak, przyjęcie wzoru tożsamości hybrydowych wpływa na ten proces?

## <a name="access-control"></a>Kontrola dostępu
Podczas uwierzytelniania i autoryzacji są podstawowe elementy, które umożliwiają dostęp do danych firmowych do sprawdzania poprawności użytkownika, należy również na kontrolowanie poziomu dostępu, który Ci użytkownicy będą mieć i poziomu dostępu administratorów będą mieć na zarządzania zasobami. Rozwiązanie tożsamości hybrydowych muszą mieć możliwość szczegółowego dostęp do zasobów, delegowanie i kontroli dostępu podstawowego roli. Upewnij się, że odpowiedzi są następujące pytanie dotyczące kontroli dostępu:

- Czy firma ma więcej niż jeden użytkownik z podwyższonym poziomem uprawnień do zarządzania systemem tożsamości?
 - Jeśli tak, usługa każdego użytkownika musi ten sam poziom dostępu?
- Firma musi pełnomocnictwa do użytkowników do zarządzania zasobami określonych?
 - Jeśli tak, jak często tak się dzieje?
- Firma musi być zintegrować funkcje kontroli dostępu między wdrożeniem lokalnym a chmurą zasoby?
- Firma należy ograniczyć dostęp do zasobów według niektóre warunki?
- Firma musi dowolnej aplikacji, która wymaga niestandardowego kontrolowanie dostępu do niektórych zasobów?
 - Jeśli tak, gdzie te aplikacje znajdują się (lokalnego lub w chmurze)?
 - Jeśli tak, gdzie są te docelowe zasobów (lokalnego lub w chmurze)?

>[AZURE.NOTE]
Pamiętaj sporządzać notatki każdej odpowiedzi i opis uzasadnienie odpowiedź. [Definiowanie strategię ochrony danych](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) będą wyświetlane na przycisku Opcje dostępne i zalety i wady poszczególnych opcji.  Odpowiedzi na te pytania, możesz wybrać opcji najlepiej pasuje do potrzeb biznesowych.

## <a name="next-steps"></a>Następne kroki

[Określanie wymagania odpowiedzi wystąpieniu zdarzenia](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Zobacz też
[Omówienie zagadnienia dotyczące projektowania](active-directory-hybrid-identity-design-considerations-overview.md)
