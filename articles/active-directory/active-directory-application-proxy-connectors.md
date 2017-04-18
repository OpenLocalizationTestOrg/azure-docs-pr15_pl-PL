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


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Publikowanie aplikacji na oddzielne sieci i lokalizacji przy użyciu grup łącznika

> [AZURE.SELECTOR]
- [Azure portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Portal Azure klasyczny](active-directory-application-proxy-connectors.md)


Łącznik grupy są przydatne dla różnych scenariuszy, w tym:

- Witryny z wieloma połączone centrach danych. W tym przypadku chcesz zachować tyle ruchu w centrum danych jak to możliwe, ponieważ łącza centrum danych krzyżowe są zwykle drogich i wolna. Można wdrażać łączniki w każdym centrum danych do obsługi tylko te aplikacje, które znajdują się w centrum danych. Ta metoda minimalizuje centrum danych między łączy i zapewnia całkowicie przezroczysty użytkowników.
- Zarządzanie aplikacjami zainstalowany na odizolowanych sieci, które nie są częścią głównego sieci firmowej. Za pomocą grup łącznik do zainstalowania dedykowane łączniki w sieciach odizolowanych również wyodrębnić aplikacji sieci.
- Dla aplikacji zainstalowanych na IaaS dostęp w chmurze łącznik grupy stanowią wspólne usługi bezpiecznego dostępu do wszystkich aplikacji. Łącznik grup nie tworzyć dodatkowe zależności w sieci firmowej lub fragment obsługi aplikacji. Łączniki można zainstalować na każdej centrum danych w chmurze i służy tylko te aplikacje, które znajdują się w tej sieci. Możesz zainstalować kilka łączników uzyskanie wysokiej dostępności.
- Obsługa środowiska wielu las, w których określone łączników można wdrożone na las i ustaw służyć określonych aplikacji.
- Łącznik grupy można w witrynach odzyskiwanie albo wykrywanie pracy awaryjnej lub jako kopii zapasowej dla głównej witryny.
- Łącznik grupy można również do obsługi wielu firm z jednej dzierżawy.

## <a name="prerequisite-create-your-connectors"></a>Wymagania wstępne: Tworzenie łączników
Aby pogrupować łączników, musisz upewnij się, że [zainstalowano wiele łączników](active-directory-application-proxy-enable.md)i tej nazwy możesz je, a następnie zgrupować oba. Na koniec należy je przypisać do określonej aplikacji.

## <a name="step-1-create-connector-groups"></a>Krok 1: Tworzenie grup łącznika
Możesz utworzyć dowolną liczbę grup łącznik. Tworzenie grupy łącznik jest wykonywana w portalu klasyczny Azure.

1. Zaznacz katalogu, a następnie kliknij przycisk **Konfiguruj**.  
    ![Serwer proxy aplikacji, konfigurowanie zrzut ekranu — kliknij pozycję Zarządzanie grupami łącznika](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)

2. W obszarze Serwer Proxy aplikacji kliknij pozycję **Zarządzanie grupami łącznik** i Utwórz nową grupę łącznik nadanie nazwy grupy.  
    ![Zrzut ekranu z grup łącznik serwera proxy aplikacji — nazwę nowej grupy](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Krok 2: Przypisywanie łączniki do grup
Po utworzeniu grupy łącznik przenoszenie łączników do odpowiedniej grupy.

1. W obszarze **Serwer Proxy aplikacji**kliknij polecenie **Zarządzaj łączniki**.
2. W obszarze **grupy**wybierz grupę, którą chcesz dla każdego łącznika. Łączniki może potrwać do 10 minut staje się aktywny w nowej grupie.  
    ![Zrzut łączników serwera proxy aplikacji — wybierz grupę z menu rozwijanego](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Krok 3: Przypisywanie aplikacji do grup łącznika
Ostatnim krokiem jest każdej aplikacji do grupy łącznik, który będzie służyć go.

1. W portalu klasyczny Azure, w katalogu wybierz aplikację chcesz przypisać do grupy, a następnie kliknij przycisk **Konfiguruj**.
2. W obszarze **grupy łącznika**zaznacz grupę, będzie aplikację do korzystania z. Natychmiast po zastosowaniu tej zmiany.  
    ![Zrzut ekranu z grupy łącznik proxy aplikacji — wybierz grupę z menu rozwijanego](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)


## <a name="see-also"></a>Zobacz też

- [Włączanie serwera Proxy aplikacji](active-directory-application-proxy-enable.md)
- [Włączyć logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)
- [Włączanie dostępu warunkowego](active-directory-application-proxy-conditional-access.md)
- [Rozwiązywanie problemów, jakie masz z serwera Proxy aplikacji](active-directory-application-proxy-troubleshoot.md)

Aby uzyskać najnowsze informacje i aktualizacje zapoznaj się z [serwera Proxy aplikacji blogu](http://blogs.technet.com/b/applicationproxyblog/)
