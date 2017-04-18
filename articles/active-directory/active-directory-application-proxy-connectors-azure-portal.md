<properties
    pageTitle="Praca z łącznikami Proxy aplikacji usług Azure AD | Microsoft Azure"
    description="Opisano, jak tworzyć i zarządzanie grupami łączniki Azure AD serwera Proxy aplikacji."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups---public-preview"></a>Publikowanie aplikacji na oddzielne sieci i lokalizacji przy użyciu łącznika grup — Public Preview

> [AZURE.SELECTOR]
- [Azure portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Portal Azure klasyczny](active-directory-application-proxy-connectors.md)


Łącznik grupy są przydatne dla różnych scenariuszy, w tym:

- Witryny z wieloma połączone centrach danych. W tym przypadku chcesz zachować tyle ruchu w centrum danych jak to możliwe, ponieważ łącza centrum danych między są drogich i wolna. Można wdrażać łączniki w każdym centrum danych do obsługi tylko te aplikacje, które znajdują się w centrum danych. Ta metoda minimalizuje centrum danych między łączy i zapewnia całkowicie przezroczysty użytkowników.
- Zarządzanie aplikacjami zainstalowany na odizolowanych sieci, które nie są częścią głównego sieci firmowej. Za pomocą grup łącznik do zainstalowania dedykowane łączniki w sieciach odizolowanych również wyodrębnić aplikacji sieci.
- Dla aplikacji zainstalowanych na IaaS dostęp w chmurze łącznik grupy stanowią wspólne usługi bezpiecznego dostępu do wszystkich aplikacji. Łącznik grup nie tworzyć dodatkowe zależności w sieci firmowej lub fragment obsługi aplikacji. Łączniki można zainstalować na każdej centrum danych w chmurze i służy tylko te aplikacje, które znajdują się w tej sieci. Możesz zainstalować kilka łączników uzyskanie wysokiej dostępności.
- Obsługa środowiska wielu las, w których określone łączników można wdrożone na las i ustaw służyć określonych aplikacji.
- Łącznik grupy można w witrynach odzyskiwanie albo wykrywanie pracy awaryjnej lub jako kopii zapasowej dla głównej witryny.
- Łącznik grupy można również do obsługi wielu firm z jednej dzierżawy.

## <a name="prerequisite-create-your-connectors"></a>Wymagania wstępne: Tworzenie łączników
Aby pogrupować łączników, musisz mieć pewność, że [zainstalowano wiele łączników](active-directory-application-proxy-enable.md). Po zainstalowaniu nowego łącznika automatycznie łączy grupy **domyślne** łącznika.

## <a name="step-1-create-connector-groups"></a>Krok 1: Tworzenie grup łącznika
Możesz utworzyć dowolną liczbę grup łącznik. Tworzenie grupy łącznik jest wykonywana w [Azure portal](https://portal.azure.com).

1. Wybierz **Usługi Azure Active Directory** , aby przejść do pulpit nawigacyjny zarządzania do katalogu. W tym miejscu wybierz **aplikacje dla przedsiębiorstw** > **serwera proxy aplikacji**.

2. Wybierz przycisk **Łącznik grup** . Zostanie wyświetlona karta Nowa grupa łącznik.

3. Nadaj nazwę nowej grupy łącznik, a następnie za pomocą menu rozwijanego wybierz, które łączniki znajdować się w tej grupie.

4. Po wykonaniu tego łącznika grupy, wybierz przycisk **Zapisz** .

## <a name="step-2-assign-applications-to-your-connector-groups"></a>Krok 2: Przypisywanie aplikacji do grup łącznika
Ostatnim krokiem jest każdej aplikacji do grupy łącznik, który będzie służyć go.

1. Pulpit nawigacyjny zarządzania dla katalogu, zaznacz **aplikacji przedsiębiorstwa** > **wszystkie aplikacje** > aplikacji, której chcesz przypisać do grupy łącznika > **Serwer Proxy aplikacji**.
2. W obszarze **grupy łącznik**za pomocą menu rozwijanego wybierz grupę, której chcesz używać aplikacja.
3. Wybierz przycisk **Zapisz** , aby zastosować zmiany.


## <a name="see-also"></a>Zobacz też

- [Włączanie serwera Proxy aplikacji](active-directory-application-proxy-enable.md)
- [Włączyć logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)
- [Włączanie dostępu warunkowego](active-directory-application-proxy-conditional-access.md)
- [Rozwiązywanie problemów, jakie masz z serwera Proxy aplikacji](active-directory-application-proxy-troubleshoot.md)

Aby uzyskać najnowsze informacje i aktualizacje zapoznaj się z [serwera Proxy aplikacji blogu](http://blogs.technet.com/b/applicationproxyblog/)
