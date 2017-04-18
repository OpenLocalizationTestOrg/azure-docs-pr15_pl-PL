<properties
    pageTitle="Włącz stan Enterprise mobilnego dostępu do ustawień w usłudze Azure Active Directory | Microsoft Azure"
    description="Często zadawane pytania dotyczące mobilnego stan Enterprise ustawień na urządzenia z systemem Windows. Mobilny stan przedsiębiorstwa umożliwia użytkownikom korzystanie z obsługi ujednolicony przez urządzenia z systemem Windows i skraca czas potrzebny do konfigurowania nowego urządzenia."
    services="active-directory"
    keywords="Stan Enterprise mobilnych, chmurze systemu windows, jak włączyć mobilnych stan przedsiębiorstwa"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>



# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Włącz stan Enterprise mobilnego dostępu do ustawień w usłudze Azure Active Directory

Mobilnego stan przedsiębiorstwa są dostępne dla Twojej organizacji z subskrypcją Premium usługi Azure Active Directory (Azure AD). Aby uzyskać więcej informacji na temat uzyskiwania subskrypcji usługi Azure AD wyświetlona [Strona produktu Azure AD](https://azure.microsoft.com/services/active-directory).

Po włączeniu mobilnego stan przedsiębiorstwa, organizacji automatycznie otrzyma licencji dla subskrypcji bezpłatne, ograniczonej do usługi Azure Rights Management. To bezpłatna subskrypcja jest ograniczone do szyfrowania i odszyfrowywania ustawienia przedsiębiorstwa i danych aplikacji synchronizowane przez usługę mobilnego stan przedsiębiorstwa; Musisz mieć subskrypcję płatną korzystać w pełni możliwości usługi Azure Rights Management.

Po uzyskaniu subskrypcji Premium Azure AD, wykonaj następujące kroki umożliwiające mobilnego stan Enterprise:

1. Zaloguj się do portalu klasyczny Azure.
2. Po lewej stronie wybierz pozycję **Usługi ACTIVE DIRECTORY**, a następnie wybierz katalogu, dla której chcesz włączyć mobilnych stan przedsiębiorstwa.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Przejdź do karty **Konfiguruj** u góry.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4.  Przewinięcie strony w dół i wybierz, **Użytkownicy mogą SYNCHRONIZOWAĆ ustawienia i dane aplikacji na poziomie przedsiębiorstwa**, a następnie kliknij przycisk **ZAPISZ**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Dla systemu Windows 10 na urządzeniu mobilny dostęp do ustawień za pomocą usług mobilnych stan przedsiębiorstwa urządzenie musi uwierzytelnianie za pomocą tożsamości Azure AD. W przypadku urządzeń dołączonych do Azure AD użytkownik podstawowy jest tożsamości Azure AD dodatkowa konfiguracja nie jest więc wymagana. Dla urządzeń korzystających z usługi Active Directory w lokalnej tradycyjnych administrator IT należy [połączyć urządzenia domeny, aby Azure AD dla środowiska systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Magazynowanie danych synchronizacji
Przedsiębiorstwo stan mobilnego dostępu do ustawień danych jest obsługiwana w jeden lub więcej [regionów Azure](https://azure.microsoft.com/regions/ ) , najlepiej wyrównany o wartości kraj/region w przypadku usługi Azure Active Directory. Przedsiębiorstwo stan mobilnego dostępu do ustawień danych jest podzielona oparte na trzech głównych regionach geograficznych: Ameryka Północna, EMEA i APAC. Mobilnego stan Enterprise danych dla dzierżawy znajduje się lokalnie z regionu geograficznego, a nie są replikowane między różnymi regionami.  Na przykład klienci, którzy mają wartości kraju/regionu, ustaw jedną z krajów regionu EMEA, takich jak "Francja" lub "Zambii" będą używane dane o jego hostowana w jednym lub Azure regionów w Europie.  Klienci, którzy ustawić wartości kraj/region w Azure AD do jednego z krajów Ameryki Północnej, takich jak "USA" lub "Kanada" będą mieli swoje dane przechowywane w jednej lub większej liczby Azure regionów w Stanach Zjednoczonych.  Klienci, którzy ustawić wartości kraj/region w Azure AD do jednego z krajów APAC, takie jak "Australia" lub "Nowa Zelandia" będą mieli swoje dane przechowywane w jednej lub większej liczby Azure regionów w Azji.  Krajów Ameryki Południowej i Antarktyka danych będzie obsługiwana w jednym lub kilku regionach Azure w Stanach Zjednoczonych.  Wartość kraju/regionu jest ustawiony w ramach procesu tworzenia katalogu Azure AD i nie można zmodyfikować później. 

Aby uzyskać więcej informacji na temat lokalizacji przechowywania danych, zgłoś biletów z [obsługą Azure](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Zarządzanie mobilnego stan przedsiębiorstwa
Azure AD administratorów globalnych można włączać i wyłączać usługi mobilnego stan Enterprise wiadomości w portalu klasyczny Azure.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Globalne Administratorzy mogą ograniczyć ustawienia synchronizacji w określonych grupach zabezpieczeń.

Administratorzy globalni także wyświetlić raport o stanie synchronizacji urządzenia użytkownika przez wybranie określonego użytkownika z listy **użytkowników** wystąpienia usługi Active Directory, klikając kartę **urządzenia** i wybieranie widoku **synchronizacji ustawienia i dane aplikacji przedsiębiorstwa urządzeń**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

##<a name="data-retention"></a>Przechowywanie danych
Dane zsynchronizowanym z usługą Azure za pośrednictwem przedsiębiorstwa stan mobilnego dostępu do ustawień zostaną zachowane czas nieokreślony, chyba że przeprowadzana jest operacja usuwania ręcznego lub omawiane dane jest uznane za przestarzałe. 

**Jawne usunięcie:** Dane są usuwane po Azure administrator usuwa użytkownika lub katalogu lub administratorem jawnie żąda danych ma zostać usunięty.

- **Usunięcie użytkownika**: po usunięciu użytkownika w Azure AD mobilnego dane konto użytkownika będzie oznaczona do usunięcia i zostaną usunięte między 180 do 90 dni. 
- **Usuwanie katalogu**: jest usunięcie cały katalog w Azure AD natychmiastowego działania. Wszystkie dane ustawień skojarzonych z których katalogu będzie oznaczona do usunięcia i zostaną usunięte między 180 do 90 dni. 
- **Żądanie usuwania**: Jeśli administrator Azure AD chce ręcznie usunąć określonego użytkownika lub ustawienia danych, administrator może pliku biletów z [Azure pomocy technicznej](https://azure.microsoft.com/support/). 

**Usuwanie starych danych**: dane, które nie była używana rok ("okres przechowywania") jest traktowana jako stałe i może zostać usunięty z platformy Azure. Okres przechowywania może ulec zmianie, ale nie będzie mniej niż 90 dni. Dane stare może być określonego zestawu ustawienia systemu Windows i aplikacji lub wszystkie ustawienia użytkownika. Na przykład:
 
- Jeśli żadne urządzenia dostępu do kolekcji określonych ustawień (np. aplikacja zostanie usunięta z urządzenia lub grupa ustawienia, takie jak "Motywu" jest wyłączona dla wszystkich urządzeń użytkownika), a następnie tej kolekcji staną się przestarzałe po okres przechowywania i mogą zostać usunięte. 
- Jeśli użytkownik ma wyłączone ustawienia synchronizacji na wszystkich swoich urządzeniach, następnie żadne dane ustawienia są dostępne, a wszystkie dane ustawienia dla tego użytkownika staną się przestarzałe i mogą zostać usunięte po okres przechowywania. 
- Jeśli administrator katalogu Azure AD wyłącza mobilnego stan przedsiębiorstwa na całym katalogu, następnie wszystkich użytkowników w tym katalogu będą już synchronizowane ustawienia, a wszystkie dane ustawienia dla wszystkich użytkowników staną się przestarzałe i mogą zostać usunięte po okres przechowywania. 

**Odzyskiwanie usuniętych danych**: zasad przechowywania danych jest niemożliwe. Po danych został trwale usunięty, nie będzie możliwe do odzyskania. Jest jednak pamiętać, że dane ustawienia tylko zostaną usunięte z platformy Azure, a nie na urządzeniu użytkownika końcowego. Jeśli dowolnego urządzenia później ponownie połączy się usługę mobilnego stan przedsiębiorstwa, ustawienia ponownie będą synchronizowane i przechowywane w Azure.


## <a name="related-topics"></a>Tematy pokrewne
- [Omówienie mobilnego stan przedsiębiorstwa](active-directory-windows-enterprise-state-roaming-overview.md)
- [Ustawienia i dane mobilnych — często zadawane pytania](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Ustawienia zasad i MDM dla synchronizacja ustawień grupy](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Odwołanie ustawienia mobilnego systemu Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
