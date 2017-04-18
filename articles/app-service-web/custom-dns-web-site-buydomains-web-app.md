<properties
    pageTitle="Jak kupić nazwę domeny niestandardowej w aplikacjach sieci Web usługi aplikacji Azure"
    description="Dowiedz się, jak kupić nazwę domeny niestandardowej aplikacji sieci web w usłudze Azure aplikacji."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="buy-and-configure-a-custom-domain-name-in-azure-app-service"></a>Zakup i konfigurowanie niestandardowej nazwy domeny w usłudze Azure aplikacji

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Gdy tworzysz aplikację sieci web, Azure przypisuje go do poddomeną azurewebsites.net. Na przykład jeśli aplikacji sieci web ma nazwę **firmy contoso**, adres URL jest **contoso.azurewebsites.net**. Azure przypisuje wirtualnego adresu IP.

Dla aplikacji sieci web produkcji prawdopodobnie ma mieć możliwość Zobacz niestandardowej nazwy domeny. W tym artykule wyjaśniono, jak kupić i konfigurowanie domeny niestandardowej z [Aplikacji sieci Web usługi](http://go.microsoft.com/fwlink/?LinkId=529714). 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## <a name="overview"></a>Omówienie

Jeśli nie masz nazwy domeny dla aplikacji sieci web, możesz ją łatwo kupić w [Azure Portal](https://portal.azure.com/). Podczas procesu zakupu możesz mieć rekordy DNS sieci Web i główny domeny automatycznie mapowane do aplikacji sieci web. Możesz również zarządzać swoich domen bezpośrednio w Azure Portal.


Wykonaj następujące czynności, aby kupić nazw domen i przypisać do aplikacji sieci web.

1. Otwórz [Azure Portal](https://portal.azure.com/)w przeglądarce.

2. Na karcie **Aplikacje sieci Web** kliknij nazwę aplikacji sieci web, wybierz pozycję **Ustawienia**, a następnie wybierz pozycję **domen niestandardowych**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. Karta **domen niestandardowych** kliknij **kupowanie domeny**.

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. W karta **Kupowanie domeny** przy użyciu pola tekstowego do wpisz nazwę domeny, którą chcesz kupić, a następnie naciśnij klawisz Enter. Sugerowane dostępnych domen będą wyświetlane poniżej pola tekstowego. Wybierz domenę, które chcesz kupić. Możesz kupić jednocześnie wiele domen. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. Kliknij pozycję **Informacje o kontakcie** i wypełnij formularz informacji kontaktowych domeny.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

    > [AZURE.NOTE] Jest bardzo ważne, wypełnij wszystkie wymagane pola z tyle dokładność jak to możliwe, zwłaszcza adres e-mail. W przypadku kupowania domeny bez "Ochrony prywatności", może zostać wyświetlony monit o sprawdzenie poczty e-mail przed aktywnym domeny. W niektórych przypadkach nieprawidłowych danych, aby uzyskać więcej informacji spowoduje błąd kupienie domeny. 

6. Teraz możesz wybrać

    ) "automatyczne odnawianie" domeny każdego roku
    
    b) opcjonalnych w "Ochrony prywatności", który jest dołączony bezpłatnie cena zakupu (z wyjątkiem domeny najwyższego poziomu kto rejestru nie obsługuje prywatności. For example:. co.in,. co.uk itp.)  
    
    c) "przypisywanie domyślnej nazwy hostów" WWW i główne domeny do bieżącej aplikacji sieci Web. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
    > [AZURE.NOTE] Opcja C konfiguruje powiązań DNS i powiązania Hostname automatycznie dla Ciebie.  Dzięki temu aplikacji sieci Web jest możliwy przy użyciu domeny niestandardowej, jak zakup zakończeniu (baring DNS propagowanie opóźnienia w niektórych przypadkach). W przypadku aplikacji sieci Web jest za Menedżer ruchu Azure, możesz nie zobaczą opcję, aby przypisać domeny głównej, zgodnie z rekordów nie działają przy użyciu Menedżera ruch. Możesz zawsze przypisać domeny i podrzędne-domains zakupionych za pośrednictwem jednej aplikacji sieci Web do innej aplikacji sieci Web i na odwrót. Zobacz krok 8, aby uzyskać więcej informacji. 
    
7. Kliknij przycisk **Zaznacz** na karta **Kupowanie domeny** , a następnie informacje o zakupu będą wyświetlane na karta **Potwierdzenie zakupu** . Jeśli Zaakceptuj warunki prawne, a następnie kliknij przycisk **Kup**, zostaną przesłane zamówienia i można monitorować procesu zakupów **powiadomienia**o. Kupowanie domeny może potrwać kilka minut. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. Jeśli zamówiono pomyślnie domeny, możesz zarządzać domeną i przypisać do aplikacji sieci web. Kliknij przycisk **"..."** po prawej stronie domeny. Następnie możesz **anulować zakupu** lub **Zarządzanie domeny**. Kliknij pozycję **Zarządzaj domeny**, następnie możemy powiązać **poddomeny** z naszych aplikacji sieci web na karta **Zarządzanie domeny** . Jeśli chcesz powiązać **poddomeny** do innej aplikacji sieci Web następnie wykonać ten krok z w kontekście odpowiednich aplikacji sieci Web. W tym miejscu można wybrać w celu przypisać domeny do punktu końcowego Menedżer ruchu (w przypadku aplikacji Web App za TM), po prostu Wybieranie Menedżer ruchu nazwę z menu rozwijanego. Dzięki temu domeny i poddomeny są automatycznie przypisywane do wszystkich aplikacji sieci Web za tego punktu końcowego Menedżer ruchu. 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

    > [AZURE.NOTE] Możesz "anulować zakupu" w ciągu 5 dni do pełnego zwrotu. Po 5 dni, które będą nie będą mogli "Anuluj zakupu", zamiast tego pojawi się opcja "Usuwania" domeny. Usunięcie domeny spowoduje usunięcie go z subskrypcji bez zwrotu kosztów i staną się dostępne domeny. 

Po zakończeniu konfiguracji niestandardowej nazwy domeny zostaną zapisane w sekcji **Nazwa hosta powiązań** aplikacji sieci web.

W tym momencie powinno być możliwe wprowadź nazwę domeny niestandardowej w przeglądarce i zobacz, że pomyślnie przejście do aplikacji sieci web.
 
## <a name="what-happens-to-the-custom-domain-you-bought"></a>Co się dzieje z domeny niestandardowej, który został zakupiony

Domeny niestandardowej, który został zakupiony w karta **domen niestandardowych i SSL** jest związany z subskrypcją Azure. Co zasób Azure za pomocą aplikacji usługi aplikacji, która najpierw zakupiono domeny dla tej domeny niestandardowej jest oddzielne i niezależne. Oznacza to, że:

- W portalu Azure można użyć domeny niestandardowej, który został zakupiony dla więcej niż jedną aplikację usługi aplikacji, a nie tylko dla aplikacji, którą najpierw zakupiono niestandardowej domeny dla. 
- Możesz zarządzać wszystkich domen niestandardowych nabycia Azure subskrypcji, przechodząc do **domen niestandardowych i SSL** karta *any* aplikacji aplikacji usługi w tej subskrypcji.
- Możesz przypisać dowolnej aplikacji dla aplikacji usługi z tej samej subskrypcji Azure poddomeny w danej domenie niestandardowej.
- Jeśli zdecydujesz się usunąć aplikację usługi aplikacji, możesz nie chcesz usunąć domeny niestandardowej, który jest powiązana, jeśli chcesz nadal jej używać w przypadku innych aplikacji.

## <a name="if-you-cant-see-the-custom-domain-you-bought"></a>Jeśli nie widzisz domeny niestandardowej zakupiono

Jeśli kupiły niestandardowej domeny w obrębie karta **domen niestandardowych i SSL** , ale nie widać domeny niestandardowej w obszarze **domeny zarządzane**, sprawdź następujące elementy:

- Nie zakończyło utworzenia własnej domeny. Sprawdź mila powiadomienia u góry Azure portal postęp.
- Nie można utworzyć niestandardowej domeny może mieć jakiegoś powodu. Sprawdź mila powiadomienia u góry Azure portal postęp.
- Domeny niestandardowej zakończyło się pomyślnie, ale karta może nie zostaną odświeżone jeszcze. Spróbuj ponownie otworzyć karta **domen niestandardowych i SSL** .
- Domeny niestandardowej w pewnym momencie może być usunięte. Sprawdź dzienniki inspekcji, klikając **Ustawienia** > **Dzienniki inspekcji** w głównym karta Twojej aplikacji. 
- Karta **domen niestandardowych i SSL** , którą chcesz odnaleźć w mogą należeć do aplikacji utworzony w innej subskrypcji Azure. Przełącz się do innej aplikacji w innej subskrypcji i sprawdź jego karta **domen niestandardowych i SSL** .  
  W portalu nie będą mogli Zobacz lub Zarządzanie domenami niestandardowy utworzony w innej subskrypcji Azure niż aplikacji. Jednak po kliknięciu przycisku **Zaawansowane zarządzanie** w karta **Zarządzanie domeny** domeny nastąpi przekierowanie do witryny sieci Web dostawcy domeny, której będziesz mieć możliwość   [Ręczne konfigurowanie domeny niestandardowej, na przykład wszystkie zewnętrzne domeny niestandardowej](web-sites-custom-domain-name.md)  
   w przypadku aplikacji utworzony w innej subskrypcji Azure. 


