<properties
    pageTitle="Azure AD Connect synchronizacją: opis użytkowników i kontaktów | Microsoft Azure"
    description="W tym miejscu wyjaśniono użytkowników i kontaktów w narzędzie Azure AD Connect synchronizacji."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Azure AD Connect synchronizacją: opis użytkowników i kontaktów

Istnieje kilka różnych powodów, dlaczego trzeba wielu lasów usługi Active Directory i istnieje kilka różnych wdrożenia topologii. Typowe modele zawierają wdrożenia usługi konta zasobów i GAL sync'ed lasy po połączeniu & nabycia. Lecz nawet w przypadku modeli wyłącznie, modeli hybrydowych są także wspólne. Domyślna konfiguracja w narzędzie Azure AD Connect synchronizacją nie przyjmuje poszczególnych modeli, ale w zależności od sposobu pasujących użytkownika zostało zaznaczone w podręczniku instalacji można zaobserwować różne zachowania.

W tym temacie zostały opisane na za pośrednictwem zachowanie domyślnej konfiguracji w niektórych topologii. Firma Microsoft przejdzie przez konfigurację i Edytor reguł synchronizacji umożliwia przeglądanie konfiguracji.

Istnieje kilka ogólnych reguł, które w konfiguracji przyjęto:

- Bez względu na kolejność możemy Importowanie ze źródła aktywnego katalogów w wyniku powinny mieć takie same.
- Aktywne konto będzie zawsze Współtworzenie informacji logowania, takich jak **userPrincipalName** i **sourceAnchor**.
- Wyłączone konto będzie Współtworzenie userPrincipalName i sourceAnchor, chyba że jest skrzynki pocztowej połączonego przypadku nie aktywnego konta do znalezienia.
- Konto z połączonych skrzynki pocztowej nigdy nie zostanie użyty userPrincipalName i sourceAnchor. Przyjmuje się, później odnaleźć aktywne konto.
- Obiekt kontaktu może być przygotowana do Azure AD jako kontakt lub jako użytkownik. Naprawdę nie można ustalić, aż wszystkie źródła lasy usługi Active Directory zostały przetworzone.

## <a name="contacts"></a>Kontakty

Kontakty, które reprezentującą użytkownika w różnych las jest typowych po połączeniu & nabycia miejsce, w którym rozwiązanie GALSync jest łączące dwa lub więcej lasy programu Exchange. Obiektu kontaktu jest zawsze łączącą z obszaru łącznik metaverse przy użyciu atrybutu poczty. Jeśli już istnieje obiekt kontaktu lub użytkownika z tym samym adresem poczty, obiekty są połączone ze sobą. To jest skonfigurowany w regule **w z usługi Active Directory — dołączanie do kontaktu**. Jest również reguły o nazwie **w z usługi Active Directory — kontakt typowych** przepływu atrybut do metaverse atrybutów **sourceObjectType** z stałą **kontaktu**. Ta reguła ma bardzo niskie pierwszeństwo, jeśli dowolnego obiektu użytkownika jest dołączony do tego samego obiektu metaverse, a następnie reguły **w z usługi Active Directory — użytkownika typowych** przyczynić się wartość użytkownika do tego atrybutu. Przy użyciu tej reguły ten atrybut będzie miał wartość kontaktu, jeśli użytkownik nie dołączył i wartość użytkownika jeśli wykryło co najmniej jednego użytkownika.

Dla inicjowania obsługi administracyjnej obiektu Azure AD, reguły wychodzącej **zewnątrz do AAD — kontakt dołączanie** utworzy obiektu kontaktu, jeśli ustawiono atrybut metaverse **sourceObjectType** **skontaktuj**się. Jeśli ten atrybut jest ustawiona na **użytkownika**, następnie reguły **zewnątrz do AAD — dołączanie do użytkownika** będzie utworzyć użytkownika obiekt.
Jest to możliwe, że obiekt jest podwyższany od kontaktu do użytkownika po katalogów aktywnego źródła są importowane i synchronizowane.

Na przykład w topologii GALSync możemy spowoduje znalezienie kontaktom dla wszystkich osób w drugi las możemy importowania pierwszego las. Spowoduje to etapie nowych kontaktów obiektów w łącznik AAD. Gdy firma Microsoft później importowanie i synchronizowanie drugi las, możemy znajdzie rzeczywistą użytkowników i połączyć je do istniejących obiektów metaverse. Firma Microsoft zostanie następnie usuń kontaktów obiekt AAD i utworzyć nowy obiekt użytkownika.

Jeśli masz topologii miejsce, w którym użytkowników i przedstawione w postaci kontaktów, upewnij się, zaznacz odpowiadające użytkowników na atrybut poczty w podręczniku instalacji. Jeśli wybierzesz opcję inny, będą mieć konfiguracji zależne kolejności. Kontaktom będzie zawsze dołączyć na atrybut poczty, ale na atrybut poczty tylko połączą obiektów użytkowników, jeśli ta opcja została wybrana w podręczniku instalacji. Może następnie na końcu dwóch różnych obiektów w metaverse atrybutem poczty Jeśli obiektu kontaktu został zaimportowany przed obiekt użytkownika. Podczas eksportowania do Azure AD zostanie zgłoszony błąd. To zachowanie jest zgodne z projektem i wskazują nieprawidłowych danych lub że topologii nie został poprawnie zidentyfikował podczas instalacji.

## <a name="disabled-accounts"></a>Wyłączone konta

Wyłączone konta są synchronizowane także Azure AD. Wyłączone konta są wspólne dla reprezentują zasobów w programie Exchange, na przykład sal konferencyjnych. Wyjątkiem jest użytkowników ze skrzynki pocztowej połączonego; jak wcześniej wspomniano te będą nigdy nie obsługi administracyjnej konta do Azure AD.

Przy założeniu jest który, w przypadku wyłączone konto użytkownika zostanie znaleziony, a następnie firma Microsoft nie znajdzie innego aktywne konto później i obiekt ponieważ jest ono inicjowane Azure AD przy użyciu userPrincipalName i sourceAnchor znaleźć. W przypadku innego aktywne konto będzie dołączyć do tego samego obiektu metaverse, następnie jej userPrincipalName i sourceAnchor zostanie użyty.

## <a name="changing-sourceanchor"></a>Zmienianie sourceAnchor

Gdy nie można już zmienić sourceAnchor, a następnie wyeksportowaniu Azure AD obiektu. Po wyeksportowaniu obiektu metaverse atrybut **cloudSourceAnchor** jest ustawiona wartość **sourceAnchor** akceptowane przez Azure AD. **SourceAnchor** zostanie zmieniony, które nie pasują **cloudSourceAnchor**reguły **zewnątrz do AAD — dołączanie do użytkownika** będzie Zgłoś błąd **atrybutów sourceAnchor uległa zmianie**. W tym przypadku konfiguracji lub dane muszą zostać poprawione, tym samym sourceAnchor znajduje się w metaverse ponownie przed obiekt może być ponownie zsynchronizowane.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Azure AD Sync połączenia: Opcje dostosowywania synchronizacji](active-directory-aadconnectsync-whatis.md)
* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
