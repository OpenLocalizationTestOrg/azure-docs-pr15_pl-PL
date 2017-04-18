
<properties
    pageTitle="Za pomocą Azure AD nawiązywanie połączenia z usługami AD DS kondycji | Microsoft Azure"
    description="To jest strona Azure AD łączenie kondycja, która przedstawimy sposobów monitorowania usług AD DS."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="arluca"/>

# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Za pomocą Azure AD nawiązywanie połączenia z usługami AD DS kondycji
Poniższą dokumentację dotyczy monitorowania usług domenowych Active Directory z Azure AD łączenie kondycją. Obsługiwane wersje usług AD DS to: Windows Server 2008 R2, Windows Server 2012 i Windows Server 2012 R2.

Aby uzyskać więcej informacji dotyczących monitorowania usług AD FS z Azure AD łączenie kondycji zobacz [Przy użyciu Azure AD łączenie kondycji z usług AD FS](active-directory-aadconnect-health-adfs.md). Ponadto informacje dotyczące monitorowania Azure AD Connect (synchronizacja) z Azure AD łączenie kondycją można znaleźć [Przy użyciu Azure AD łączenie kondycji do synchronizacji](active-directory-aadconnect-health-sync.md).

![Azure AD Connect kondycji w usługach AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Alerty dotyczące Azure AD łączenie kondycji w usługach AD DS
W sekcji alerty w Azure AD łączenie kondycji w usługach AD DS zawiera listę alerty aktywne i rozwiązane, związane z kontrolerów domeny. Wybranie alertu aktywny ani rozwiązany zostanie wyświetlona karta nowy z dodatkowymi informacjami, wraz z kroki rozwiązywania i łącza do pomocniczych dokumentacji. Każdego typu alertu mogą mieć co najmniej jeden wystąpień, które odpowiadają każdemu kontroler domeny wpływu tego określonego alertu. W dolnej części karta alertów możesz kliknąć dwukrotnie kontrolerze domeny, aby otworzyć dodatkowe karta zawierające więcej informacji na temat tego wystąpienia alertów.

W tym karta można włączyć powiadomienia e-mail dla alertów i zmienić zakres czasu w widoku. Rozszerzanie zakresu czasu umożliwia wyświetlenie poprzednich rozwiązany alertów.

![Błąd synchronizacji usługi Azure AD Connect](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Pulpit nawigacyjny kontrolerów domeny
Ten pulpit nawigacyjny Wyświetla topologii środowiska, razem z podstawowych wskaźników operacyjnych i stanie poszczególnych kontrolerach domeny monitorowane. Prezentowana metryki ułatwia szybkie identyfikowanie, dowolnego kontrolera domeny, które mogą wymagać dalszych dochodzenia. Domyślnie jest wyświetlany tylko niektóre kolumny. Jednak możesz znaleźć całego zestawu dostępne kolumny, klikając polecenie kolumny. Wybieranie kolumn, które najbardziej istotnych informacji, umieść tego pulpitu nawigacyjnego w ramach jednej i łatwe wyświetlanie kondycji środowiska usługi Domenowe zmieni się.

![Kontrolery domeny](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Kontrolery domeny mogą być grupowane według ich odpowiedniej domeny lub witryny, co jest przydatne dla zrozumienia topologii środowiska. Ponadto po dwukrotnym kliknięciu nagłówka karta Pulpit nawigacyjny maksymalizuje korzystanie z nieruchomości obrazu. W tym widoku większe jest przydatne, gdy wiele kolumn są wyświetlane.

## <a name="replication-status-dashboard"></a>Pulpit nawigacyjny stan replikacji
Ten pulpit nawigacyjny zawiera widoku stan replikacji i topologii replikacji kontrolerach domeny monitorowane. Stan ostatnio próba replikacji znajduje się wraz z dokumentacji przydatne dla dowolnego problemu, który znajduje się. Dwukrotne kliknięcie kontrolera domeny ze względu na błąd, aby otworzyć nowy karta z informacjami o takich jak: szczegółowe informacje o błędzie, zalecane kroki rozwiązywania i łącza do dokumentacja dotycząca rozwiązywania problemów.

![Stan replikacji](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Monitorowanie
Ta funkcja zapewnia graficzne trendów liczniki różnych wydajności, które wciąż są pobierane z każdego kontroler monitorowane domeny. Łatwo można porównać wydajność kontrolera domeny na wszystkich innych kontrolerach domeny monitorowane w sieci. Ponadto widać różnych liczniki wydajności obok siebie, która jest pomocne podczas rozwiązywania problemów w środowisku.

![Monitorowanie](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Domyślnie możemy wstępnie zostały wybrane cztery liczniki wydajności; Możesz jednak umieścić inne osoby przez kliknięcie polecenia Filtruj i zaznaczenie lub usunięcie zaznaczenia wszystkie liczniki wydajności odpowiedniej. Ponadto możesz kliknąć dwukrotnie wykres licznik wydajności, aby otworzyć kartę nowy, zawierający punktów danych dla każdej z nich kontroler domeny monitorowane.

## <a name="related-links"></a>Łącza pokrewne

* [Azure AD Connect kondycji](active-directory-aadconnect-health.md)
* [Azure AD Connect instalacji agenta kondycji](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect operacje kondycji](active-directory-aadconnect-health-operations.md)
* [Podłącz za pomocą Azure AD kondycji z usług AD FS](active-directory-aadconnect-health-adfs.md)
* [Przy użyciu Azure AD łączenie kondycji dla synchronizacji](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect kondycji — często zadawane pytania](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect kondycji Historia wersji](active-directory-aadconnect-health-version-history.md)
