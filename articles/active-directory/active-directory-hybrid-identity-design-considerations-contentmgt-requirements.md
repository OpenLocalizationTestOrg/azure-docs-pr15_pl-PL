<properties
    pageTitle="Azure Active Directory hybrydowych tożsamości zagadnienia projektowe - określenie wymagań dotyczących zarządzania zawartością | Microsoft Azure"
    description="Zapewnia wgląd w sposób ustalania wymagania dotyczące zarządzania zawartością firmy. Zazwyczaj gdy użytkownik ma własny urządzenia mogą ma on również wiele poświadczeń, które będą zmienny zgodnie z aplikacji, której używa. Ważne odróżnić, jaka zawartość została utworzona przy użyciu poświadczeń osobistych w stosunku do nich utworzony przy użyciu poświadczeń firmy jest. Rozwiązanie tożsamości powinno być możliwe do współdziałania z chmury services, aby umożliwić użytkownikowi podczas płynną obsługę zapewnienia prywatności i zwiększenia ochrony przed wycieku danych."
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

# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Określenie zarządzania zawartością wymagań dotyczących rozwiązania hybrydowego tożsamości

Opis wymagań dotyczących zarządzania zawartością dla swojej firmy bezpośrednio mogą mieć wpływ na decyzję dotyczącą rozwiązania tożsamości hybrydowych za pomocą. Z mnożenia wielu urządzeń i możliwości użytkowników, aby wyświetlić własne urządzenia ([BYOD](http://aka.ms/byodcg)) firma musi chronić swoje własne dane, ale go również należy zachować prywatność użytkownika niezmieniony. Zazwyczaj gdy użytkownik ma własny urządzenia mogą ma on również wiele poświadczeń, które będą zmienny zgodnie z aplikacji, której używa. Ważne odróżnić, jaka zawartość została utworzona przy użyciu poświadczeń osobistych w stosunku do nich utworzony przy użyciu poświadczeń firmy jest. Rozwiązanie tożsamości powinno być możliwe do współdziałania z chmury services, aby umożliwić użytkownikowi podczas płynną obsługę zapewnienia prywatności i zwiększenia ochrony przed wycieku danych. 

Rozwiązanie tożsamości będzie osobie różnych techniczne kontrolek w celu zapewnienia zarządzania zawartością, jak pokazano na poniższej ilustracji:
 
![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Kontroli zabezpieczeń, które będą używanie systemie zarządzania tożsamościami**

Na ogół wymagania dotyczące zarządzania zawartością będzie korzystać z systemu zarządzania tożsamości w następujących obszarach:

- Prywatność: Identyfikowanie użytkownika, który jest właścicielem zasobu i stosowania odpowiednich kontrolek w celu zachowywanie integralności.
- Klasyfikacja danych: Identyfikowanie użytkownika lub grupę i poziom dostępu do obiektu zgodnie z ich klasyfikację. 
- Ochrona wycieku danych: kontroli bezpieczeństwa odpowiedzialnych za ochronę danych, aby uniknąć wyciek muszą interakcji z systemem tożsamości, aby sprawdzić poprawność tożsamość użytkownika. Jest to także ważne przeznaczenie dziennik inspekcji.

>[AZURE.NOTE]
Przeczytaj [klasyfikacji danych do chmury gotowości](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) , aby uzyskać więcej informacji o najważniejszych wskazówkach dotyczących i wskazówki dotyczące klasyfikacji danych.

Podczas planowania rozwiązania hybrydowego tożsamości upewnij się, że zgodnie z wymaganiami organizacji są odpowiedzieć na następujące pytania:

- Firma ma kontroli bezpieczeństwa w miejscu, aby wymusić prywatności danych?
 - Jeśli tak, te kontroli bezpieczeństwa będą mogli Integracja z rozwiązanie tożsamości hybrydowe, które ma być przyjęcie?
- Firma używa klasyfikacji danych?
 - Jeśli tak, jest możliwość integracji z rozwiązanie tożsamości hybrydowe, które mają zostać przyjmuje bieżącego rozwiązania?
- Firma ma obecnie każdego rozwiązania dla wycieku danych? 
 - Jeśli tak, jest możliwość integracji z rozwiązanie tożsamości hybrydowe, które mają zostać przyjmuje bieżącego rozwiązania?
- Czy firma należy inspekcji dostępu do zasobów?
 - Jeśli tak, jakiego typu zasobów?
 - Jeśli tak, konieczne jest poziom informacji?
 - Jeśli tak, gdy dziennik inspekcji musi znajdować się? Lokalne lub w chmurze?
- Czy firma należy szyfrować wszystkie wiadomości e-mail, które zawierają poufne dane (SSNs, numerów kart kredytowych itp.)?
- Czy firma należy szyfrować wszystkie dokumenty i zawartość udostępnione dla partnerów biznesowych zewnętrznych?
- Czy firmy muszą wymuszanie stosowania zasad firmowych na niektórych rodzajów wiadomości e-mail (nie ma wszystkie odpowiedzi, nie przesyłaj dalej)?
 
>[AZURE.NOTE]
Pamiętaj sporządzać notatki każdej odpowiedzi i opis uzasadnienie odpowiedź. [Definiowanie strategię ochrony danych](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) będą wyświetlane na przycisku Opcje dostępne i zalety i wady poszczególnych opcji.  Przez o odpowiedzi na te pytania, która zostanie wybrana wybranej opcji najlepiej pasuje Twoja firma wymaga.


## <a name="next-steps"></a>Następne kroki
[Określanie wymagania kontroli dostępu](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Zobacz też
[Omówienie zagadnienia dotyczące projektowania](active-directory-hybrid-identity-design-considerations-overview.md)
