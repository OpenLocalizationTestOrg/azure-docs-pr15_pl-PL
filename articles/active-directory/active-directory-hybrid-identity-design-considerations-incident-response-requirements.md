
<properties
    pageTitle="Azure Active Directory hybrydowych tożsamości zagadnienia projektowe - określenie wymagań zdarzenia rResponse | Wymagania dotyczące platformy Microsoft Azure "
    description="Określanie monitorowania i raportowania funkcje hybrydowe rozwiązanie tożsamości, które mogą zostać wykorzystane przez IT podjęcie działania w celu identyfikowania i ograniczanie potencjalnych zagrożeń"
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

# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Określenie zdarzenia odpowiedzi wymagań dotyczących rozwiązania hybrydowego tożsamości

Duże lub średnich firmach najprawdopodobniej będą mieć [odpowiedź zdarzenia zabezpieczeń](https://technet.microsoft.com/library/cc700825.aspx) mające na celu pomoc IT wykonać akcje odpowiednio do poziomu zdarzenia. Systemu zarządzania tożsamości jest ważny składnik w procesie zdarzenia odpowiedzi, ponieważ może służyć do pomocy identyfikujący typ przepływu pracy, który wykonywane określone działania względem celu. Rozwiązanie tożsamości hybrydowych muszą mieć możliwość zapewniają możliwości monitorowania i raportowania, które mogą zostać wykorzystane przez IT podjęcie działania w celu identyfikowania i ograniczanie zagrożenie. W planie typowe zdarzenia odpowiedzi będą mieć następujące fazy w ramach planu:

1.  Ocena początkowej.
2.  Komunikat zdarzenia.
3.  Formant uszkodzenia i zmniejszenia ryzyka.
4.  Identyfikacja co było złamania i ważności.
5.  Zachowywanie dowodów.
6.  Powiadomienie do odpowiednich osób.
7.  Odzyskiwania systemu.
8.  Dokumentacja.
9.  Ocena uszkodzenia i koszt.
10. Proces i planowanie poprawek.

Podczas identyfikacji jakie it złamania i ważności fazy, konieczne będzie do identyfikowania systemów, które zostały złamane, pliki, które zostały dostępne i określić charakter tych plików. System tożsamości hybrydowych powinno być możliwe w celu spełnienia tych wymagań, aby ułatwić identyfikowanie użytkownika, który utworzył te zmiany. 

## <a name="monitoring-and-reporting"></a>Monitorowania i raportowania
Wiele razy w systemie tożsamości pomaga w fazie wstępnej oceny głównie, jeśli system ma wbudowane inspekcji i funkcje raportowania. Podczas początkowej oceny administrator musi mieć możliwość identyfikowania podejrzane działania lub systemu powinno być możliwe wyzwalacza, które go automatycznie na podstawie wstępnie skonfigurowane zadania. Wiele działań może oznaczać atakiem możliwe, jednak w innych przypadkach źle skonfigurowany system może prowadzić do określonej liczby wyniki fałszywie dodatnie w systemie wykrywania intruzów. 

System zarządzania tożsamości pomocy Administratorzy do identyfikowania i raportowania tych podejrzanych działań. Zazwyczaj te wymagania techniczne mogą być spełnione przez wszystkich systemów monitorowania i o możliwości raportowania, które można wyróżnić potencjalnych zagrożeń. Użyj poniższe pytania ułatwiające projektowanie rozwiązania hybrydowego tożsamości uwzględniając wymagania zdarzenia odpowiedzi:

- Czy firma ma odpowiedź zdarzenia zabezpieczeń w miejscu
 - Jeśli tak, bieżącego systemu zarządzania tożsamości, służy jako część procesu?
- Czy firmy należy zidentyfikować prób logowania podejrzanych użytkownikom wielu różnych urządzeń?
- Czy firmy należy wykryć potencjalne złamany poświadczenia użytkownika?
- Czy firma należy inspekcji dostępu i akcji użytkownika?
- Czy firma należy wiedzieć, kiedy użytkownika Resetowanie hasła?

## <a name="policy-enforcement"></a>Wymuszania zasad

Podczas opanować i fazy zmniejszenie ryzyka ważne jest szybko zredukować efekty rzeczywistych i potencjalnych atakiem. W tym momencie tę akcję, która ma być może mieć różnicę między pomocnicze i jedną głównych. Porównaj odpowiedzi zależy od Twojej organizacji i rodzaj ataki, które możesz buźkę. Ocena wstępna zawarte, że konto zostało zostało naruszone, będzie konieczne wymuszania zasad do blokowania tego konta. To jest tylko jeden przykład miejsce, w którym można użyć systemu zarządzania tożsamości. Użyj poniższe pytania ułatwiające projektowanie rozwiązania hybrydowego tożsamości przy uwzględnieniu jak zasady będą wymuszane reakcji na bieżąco zdarzenia:

- Firma ma zasady w miejscu użytkownikom blok z programu access sieci w razie potrzeby?
 - Jeśli tak, czy bieżącego rozwiązania Integracja z systemu zarządzania tożsamości hybrydowe, który ma być przyjęcie?
- Czy firmy należy wymusić dostępu warunkowego dla użytkowników, które znajdują się w kwarantanny? 
 
>[AZURE.NOTE]
Pamiętaj sporządzać notatki każdej odpowiedzi i opis uzasadnienie odpowiedź. [Definiowanie strategię ochrony danych](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) będą wyświetlane dostępne opcje i zalety i wady poszczególnych opcji.  Przez o odpowiedzi na te pytania, która zostanie wybrana wybranej opcji najlepiej pasuje Twoja firma wymaga.

## <a name="next-steps"></a>Następne kroki
[Definiowanie strategię ochrony danych](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Zobacz też
[Omówienie zagadnienia projektu](active-directory-hybrid-identity-design-considerations-overview.md)
