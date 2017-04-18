<properties
    pageTitle="Wysoką dostępność geograficzne krzyżowe AD FS wdrożenia w Azure z Menedżer ruchu Azure | Microsoft Azure"
    description="W tym dokumencie dowiesz się, jak wdrożyć serwer usług AD FS platformy Azure dla wysokiej dostępności."
    keywords="Usług AD fs Menedżer ruchu Azure, adfs Menedżer ruchu Azure, geograficzne, wielokrotne centrum danych, centrach danych geograficznych, centrach danych geograficznych wielokrotne, wdrażanie usług AD FS platformy azure, wdrażanie azure adfs, azure adfs, azure ad fs, wdrażanie usług adfs, wdrażanie usług ad fs adfs w azure, wdrażanie usług adfs platformy azure, wdrażanie usług AD FS azure, adfs azure i wprowadzenie do usług AD FS, Azure AD FS w Azure, iaas , ADFS, Przenieś adfs Azure"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="anandy;billmath"/>
    
#<a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Wysoce geograficzne krzyżowe AD FS wdrożenia w Azure przy użyciu Menedżera ruch Azure

[Wdrażanie usług AD FS w Azure](active-directory-aadconnect-azure-adfs.md) zawiera wskazówki krok po kroku, jak można wdrażać prosty infrastruktury usług AD FS dla organizacji w Azure. Ten artykuł zawiera następnego procedurę tworzenia geograficzne krzyżowe wdrażanie usług AD FS platformy Azure za pomocą [Menedżera ruch Azure](../traffic-manager/traffic-manager-overview.md). Azure Menedżer ruchu ułatwia tworzenie geograficznie rozpowszechniania wysokiej dostępności i infrastruktury usług AD FS wysokiej wydajności organizacji dokonując stosowania zakres metod routingu dostępne do potrzeb różnych z infrastruktury.

Wysoce dostępnej geograficzne krzyżowe infrastruktury usług AD FS umożliwia:

* **Wyeliminowania pojedynczego punktu awarii:** Z możliwości przeniesienia awaryjnego Menedżer ruchu Azure można uzyskać wysoce dostępnej infrastruktury usług AD FS, nawet wtedy, gdy jeden z centrami danych w części globusa awarii
* **Zwiększona wydajność:** Sugerowane wdrożenia w tym artykule umożliwia podanie infrastruktury usług AD FS wysokiej wydajności, które mogą ułatwić użytkownikom uwierzytelnienia szybciej. 

##<a name="design-principles"></a>Zasady projektowania

![Ogólnego projektu](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Zasady podstawowe projektowania będzie samego wymienione w zasady projektowania w artykule wdrożenia usług AD FS w Azure. Na powyższym diagramie przedstawiono prosty rozszerzenia podstawowe wdrażania do innego regionu geograficznego. Poniżej przedstawiono niektóre kwestie do rozważenia podczas rozszerzania wdrożenia do nowego regionu geograficznego

* **Wirtualnej sieci:** Należy utworzyć nową wirtualna sieć w regionu geograficznego chcesz wdrożyć dodatkowa Infrastruktura usług AD FS. Na powyższym diagramie jest wyświetlany Geo1 VNET i Geo2 VNET jako dwa wirtualnych sieci w poszczególnych regionach geograficznych.

* **Kontrolery domeny i serwery usług AD FS w nowej VNET geograficznych:** Zaleca się wdrażanie domeny, którą kontrolerów w nowej regionu geograficznego, aby serwer usług AD FS w nowym regionie nie być konieczne skontaktowanie się kontrolera domeny w innej daleko sieci do wykonania uwierzytelniania i w związku z tym przyspieszania.

* **Kont miejsca do magazynowania:** Konta przestrzeni dyskowej są skojarzone z danym regionie. Ponieważ wdrażania komputery w nowego regionu geograficznego, masz do tworzenia nowych kont miejsca do magazynowania może być używany w regionie.  

* **Sieci grupy zabezpieczeń:** Jak kont miejsca do magazynowania, grupy zabezpieczeń sieci utworzone w regionie nie można używać w innego regionu geograficznego. W związku z tym należy utworzyć nową sieć grupy zabezpieczeń podobne do pierwszej regionu geograficznego podsieci INT i strefy Zdemilitaryzowanej w nowych regionu geograficznego.

* **Etykiety DNS dla publicznych adresów IP:** Azure Menedżer ruchu może odwoływać się do punktów końcowych tylko przy użyciu etykiet DNS. W związku z tym są wymagane do tworzenia etykiet DNS dla równoważenia obciążenia zewnętrznych publicznych adresów IP.

* **Azure Menedżer ruchu:** Menedżer ruch programu Microsoft Azure umożliwia kontrolowanie rozkład ruchu użytkownika do punktów końcowych usługi uruchomione w różnych centrach danych na całym świecie. Menedżer ruchu Azure działa na poziomie DNS. Aby skierować ruch użytkowników końcowych do punktów końcowych globalnie distributed używa odpowiedzi DNS. Następnie łączą się klienci do tych punktów końcowych bezpośrednio. Przy użyciu różnych opcji routing wydajności, ważone i priorytet możesz łatwo opcję routing, najlepiej dopasowane do potrzeb danej organizacji. 

* **Netto V V-sieć łączność między dwóch regionów:** Nie musisz być łączność między wirtualnych sieci. Ponieważ każda wirtualna sieć ma dostęp do kontrolerów domeny oraz serwer usług AD FS i WAP w sobie, one działać, bez dowolnego łączność między wirtualnych sieci w różnych regionach. 

##<a name="steps-to-integrate-azure-traffic-manager"></a>Czynności, aby zintegrować Menedżer ruchu Azure

###<a name="deploy-ad-fs-in-the-new-geographical-region"></a>Wdrażanie usług AD FS w nowej regionu geograficznego
Wykonaj kroki i wskazówki dotyczące [wdrożenia usług AD FS w Azure](active-directory-aadconnect-azure-adfs.md) wdrożenia samej topologii w nowej regionu geograficznego.

###<a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>DNS etykiety publiczne adresy IP równoważenia obciążenia Internet przeciwległych (publiczna)
Jak wcześniej wspomniano, Menedżer ruchu Azure tylko skrót w odniesieniu do etykiet DNS jako punkty końcowe i w związku z tym ważne jest, aby utworzyć etykiety DNS dla równoważenia obciążenia zewnętrznych publicznych adresów IP. Poniżej zrzut ekranu przedstawia, jak można skonfigurować oznaczenie DNS publiczny adres IP. 

![Etykieta DNS](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

###<a name="deploying-azure-traffic-manager"></a>Wdrażanie Azure Menedżer ruchu

Wykonaj poniższe czynności, aby utworzyć profil Menedżera ruch. Aby uzyskać więcej informacji może dotyczyć [Zarządzanie profilu Menedżer ruchu Azure](../traffic-manager/traffic-manager-manage-profiles.md).

1. **Tworzenie profilu Menedżer ruchu:** Określ unikatową nazwę dla profilu Menedżera ruch. Ta nazwa profilu jest część nazwy DNS i działa jako prefiksu Menedżer ruchu etykiety nazwę domeny. Nazwa / prefiks jest dodawana do. trafficmanager.net, aby utworzyć etykietę DNS dla ruchu menedżera. Zrzut ekranu poniżej pokazano Menedżer ruchu prefiks DNS są ustawione jako mysts i wyniku Etykieta DNS będzie mysts.trafficmanager.net. 

    ![Tworzenie profilu Menedżer ruchu](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
 
2. **Ruchu metody routingu:** Dostępne są trzy opcje routingu w Menedżerze ruchu:

    * Priority (priorytet) 
    * Wydajność
    * Ważona
    
    **Wydajność** jest zalecana opcja uzyskanie wysoce odpowiada infrastruktury usług AD FS. Jednak możesz wybrać dowolne metody routingu, najlepiej dopasowane do potrzeb wdrożenia. Funkcje usług AD FS nie ma wpływu na przy użyciu opcji routing zaznaczone. Aby uzyskać więcej informacji, zobacz [metody routingu ruchu Menedżer ruchu](../traffic-manager/traffic-manager-routing-methods.md) . W próbce zrzut ekranu powyżej, które można wyświetlić metodę **wydajności** zaznaczone.
   
3.  **Skonfigurować punkty końcowe:** Na stronie Menedżera ruchu kliknij punkty końcowe, a następnie wybierz pozycję Dodaj. Spowoduje to otwarcie strony punktu końcowego Dodaj podobne do poniższych zrzut ekranu
 
    ![Konfigurowanie punktów końcowych](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
 
    W przypadku różnych danych wejściowych wykonaj poniższe wytyczne:

    **Typ:** Wybierz punkt końcowy Azure, jak firma Microsoft wskazywać Azure publiczny adres IP.

    **Nazwa:** Tworzenie nazwy, która ma zostać skojarzony z punktem końcowym. To nie jest nazwą DNS i nie ma żadnego wpływu na rekordy DNS.

    **Docelowy typ zasobu:** Wybierz pozycję publiczny adres IP jako wartości tej właściwości. 

    **Docelowe zasobu:** Zapewni to opcję, aby wybrać z różnych etykiet DNS, że masz dostępne w obszarze subskrypcji. Wybierz pozycję Etykieta DNS do.

    Dodawanie punktu końcowego dla każdego regionu geograficznego odpowiednią Azure Menedżera ruchu w celu rozsyłania ruchu.
    Aby uzyskać więcej informacji i uzyskać szczegółowe instrukcje dotyczące dodawania / configure punkty końcowe w Menedżerze ruch odwołują się do [dodać, wyłączanie, włączanie i usuwanie punktów końcowych](../traffic-manager/traffic-manager-endpoints.md)
    
4. **Sonda Konfiguruj:** Na stronie Menedżera ruchu kliknij konfiguracji. Na stronie Konfiguracja musisz zmienić ustawienia monitora, aby sonda u HTTP porty 80 i ścieżkę względną /adfs/probe

    ![Konfigurowanie sondy](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 

    >[AZURE.NOTE] **Upewnij się, że stan punkty końcowe jest ONLINE po zakończeniu konfiguracji**. Jeśli wszystkie punkty końcowe są w stanie "ograniczone", Menedżer ruchu Azure wykona najlepsze próba, aby skierować ruch przy założeniu, że diagnostyki jest nieprawidłowa i wszystkie punkty końcowe są dostępne.

5. **Rekordy DNS modyfikowanie Menedżer ruchu Azure:** Usługi federacyjne powinny być CNAME na nazwę Azure Menedżer ruchu DNS. Utwórz CNAME publicznej rekordów DNS, tak aby kto próbuje uzyskać dostęp do usługi federacyjne osiągnie faktyczną Menedżer ruchu Azure.

    Na przykład aby wskazać fs.fabidentity.com usługi federacyjne Menedżer ruchu, należy Zaktualizuj rekord zasobu DNS, aby być następująca:

    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

##<a name="test-the-routing-and-ad-fs-sign-in"></a>Testowanie routingu i logowania AD FS   

###<a name="routing-test"></a>Kierowanie test

Bardzo podstawowy test dla routingu należy wypróbować ping nazwy federacyjnych usługi DNS na komputerze w każdego regionu geograficznego. W zależności od wybranej metody routingu punkt końcowy, które rzeczywiście pinguje zostaną odzwierciedlone w interfejsie ping. Na przykład w przypadku wybrania routingu wydajności, następnie punkt końcowy znajdujący się najbliżej obszaru klienta będą się z Tobą. Poniżej znajduje się migawki dwoma pakietami od dwóch komputerów klienckich innego regionu: jedna w regionie EastAsia, a druga w USA Zachód. 

![Kierowanie test](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

###<a name="ad-fs-sign-in-test"></a>AD FS logowania testowych

Najprostszym sposobem na testowanie usług AD FS jest za pomocą strony IdpInitiatedSignon.aspx. Aby móc wykonać, że jest niezbędne w celu umożliwienia IdpInitiatedSignOn we właściwościach usług AD FS. Postępuj zgodnie z instrukcjami poniżej, aby sprawdzić ustawienia usług AD FS
 
1. Uruchamianie poniżej polecenia cmdlet na serwerze usług AD FS, włącz je przy użyciu programu PowerShell. Ustawianie AdfsProperties - EnableIdPInitiatedSignonPage $true
2. Z dowolnej https:// dostępu zewnętrznego machine<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Powinien zostać wyświetlony na stronie usług AD FS, takich jak poniżej:

    ![Test ADFS - wezwanie uwierzytelniania](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)

    i na pomyślne logowania, jego umożliwi komunikat o powodzeniu tak jak pokazano poniżej:

    ![Test ADFS - sukcesu uwierzytelniania](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)
 
##<a name="related-links"></a>Łącza pokrewne
* [Podstawy wdrażania usług AD FS platformy Azure](active-directory-aadconnect-azure-adfs.md)
* [Menedżer ruchu platformy Microsoft Azure](../traffic-manager/traffic-manager-overview.md)
* [Metody routingu ruchu Menedżer ruchu](../traffic-manager/traffic-manager-routing-methods.md)

##<a name="next-steps"></a>Następne kroki
* [Zarządzanie profilu Menedżer ruchu Azure](../traffic-manager/traffic-manager-manage-profiles.md)
* [Dodawanie, wyłączanie, włączanie i usuwanie punktów końcowych](../traffic-manager/traffic-manager-endpoints.md) 

