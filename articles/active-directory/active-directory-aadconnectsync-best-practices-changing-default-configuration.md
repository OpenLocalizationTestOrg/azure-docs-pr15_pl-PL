<properties
    pageTitle="Azure AD Connect synchronizacją: Najważniejsze wskazówki dotyczące zmieniania konfiguracji domyślnej | Microsoft Azure"
    description="Zawiera najważniejsze wskazówki dotyczące zmieniania domyślnej konfiguracji synchronizacji Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect synchronizacją: Najważniejsze wskazówki dotyczące zmieniania konfiguracji domyślnej
Celem w tym temacie jest do opisu obsługiwanych i nieobsługiwanych zmiany Azure AD Connect synchronizacji.

Konfigurację utworzone przez narzędzie Azure AD Connect działa "jakim jest" w przypadku większości środowisk synchronizowanych z lokalnej usługi Active Directory z usługą Azure Active Directory. Jednak w niektórych przypadkach należy zastosować zmiany konfigurację w celu spełnienia określonych potrzeb lub wymagania.

## <a name="changes-to-the-service-account"></a>Zmiany na koncie usługi
Azure AD Connect synchronizacją działa na koncie usługi utworzone przez Kreatora instalacji. Tego konta usługi zawiera klucze szyfrowania do bazy danych używane przez synchronizacji. Zostanie utworzona za pomocą hasła długiej 127 znaków i hasło jest ustawiona na nie wygaśnie.

- Jest **nieobsługiwane** zmienianie lub resetowanie hasła do konta usługi. Wykonanie tej czynności powoduje utratę wszystkich zapisanych kluczy szyfrowania i nie będzie mógł uzyskać dostęp do bazy danych i nie będzie mógł rozpocząć.

## <a name="changes-to-the-scheduler"></a>Zmiany do harmonogramu
Począwszy od wersji z kompilacji 1.1 (luty 2016) można skonfigurować [Harmonogram](active-directory-aadconnectsync-feature-scheduler.md) mają cyklu synchronizacji inny niż domyślny 30 minut.

## <a name="changes-to-synchronization-rules"></a>Zmiany wprowadzone w regułach synchronizacji
W Kreatorze instalacji jest dostępny konfiguracji, która ma pracować najbardziej typowych scenariuszy. W przypadku, gdy chcesz wprowadzić zmiany w konfiguracji, należy wykonać te reguły do jeszcze obsługiwana konfiguracja.

- Możesz [zmienić przepływów atrybut](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) , jeśli przepływów bezpośrednich atrybut domyślne nie są odpowiednie dla Twojej organizacji.
- Jeśli chcesz [przepływu atrybut](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) i usuń wszelkie istniejące wartości atrybutu Azure AD, należy utworzyć regułę dla tego scenariusza.
- [Wyłączyć niepotrzebne regułę synchronizacji](#disable-an-unwanted-sync-rule) , a nie zostanie usunięta. Usunięte reguły zostanie ponownie utworzony podczas uaktualniania.
- [Zmienianie reguły w nowym polu](#change-an-out-of-box-rule)należy utworzyć kopię istniejącą regułę i wyłączyć regułę w nowym polu. Edytor reguł synchronizacji wyświetla monity i ułatwia.
- Eksportowanie reguł niestandardowych przy użyciu edytora reguły synchronizacji. Edytor pozwala uzyskać skrypt programu PowerShell, które umożliwiają łatwe utwórz je ponownie w scenariuszu odzyskiwania po awarii.

>[AZURE.WARNING] Reguły synchronizacji w nowym polu mają odcisku palca. Jeśli wprowadzisz zmiany w tych reguł pasujących jest już odcisku palca. Mogą wystąpić problemy w przyszłości, podczas próby stosowanie nowa wersja Azure AD Connect. Tylko zmienić sposób opisano w tym artykule.

### <a name="disable-an-unwanted-sync-rule"></a>Wyłączanie reguły niepożądane synchronizacji
Nie usuwaj reguły synchronizacji w nowym polu. Zostanie ponownie utworzony podczas uaktualniania dalej.

W niektórych przypadkach Kreatora instalacji produkcji konfiguracji, która nie działa dla danej topologii. Na przykład jeśli masz konto zasobu las topologii, ale mają rozszerzenia schematu w las konta ze schematem Exchange, zasady programu Exchange są tworzone las kontem i las zasobów. W tym przypadku należy wyłączyć regułę synchronizacji dla programu Exchange.

![Reguła synchronizacji wyłączone](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Na powyższym obrazie Kreatora instalacji znalazł stare schematu programu Exchange 2003 w las konta. To rozszerzenie schemat został dodany przed wprowadzeniem las zasobów w środowisku firmy. Aby upewnić się, że nie atrybuty ze starej wdrażania programu Exchange są synchronizowane, reguły synchronizacja powinna być wyłączona, jak pokazano.

### <a name="change-an-out-of-box-rule"></a>Zmienianie reguły w nowym polu
Jeśli potrzebujesz wprowadzić zmiany do reguły w nowym polu, należy utworzyć kopię reguły w nowym polu i wyłączyć istniejącą regułę. Następnie wprowadź odpowiednie zmiany sklonowanym reguły. Edytor reguł synchronizacji pomaga z tych kroków. Po otwarciu reguły w nowym polu są prezentowane przy użyciu tego okna dialogowego:  
![Ostrzeżenie dotyczące poza pole reguły](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Wybierz pozycję **Tak,** Aby utworzyć kopię reguły. Następnie otwarciu sklonowanym reguły.  
![Sklonowany reguły](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Na tej regule sklonowanym wprowadź niezbędne zmiany zakresu, Dołącz i przekształceń.

## <a name="next-steps"></a>Następne kroki

**Przegląd tematów**

- [Azure AD Connect synchronizacją: opis i dostosowywanie synchronizacji](active-directory-aadconnectsync-whatis.md)
- [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
