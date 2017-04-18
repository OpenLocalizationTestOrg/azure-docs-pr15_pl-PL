<properties
     pageTitle="Za pomocą Azure CDN | Microsoft Azure"
     description="W tym temacie przedstawiono sposób umożliwiający sieci dostarczania zawartości (CDN) Azure. Samouczek opisano tworzenie nowego profilu CDN i punkt końcowy."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="07/28/2016" 
     ms.author="casoper"/>

# <a name="using-azure-cdn"></a>Za pomocą Azure CDN  

W tym temacie opisano włączanie Azure CDN przez utworzenie nowego profilu CDN i punkt końcowy.

>[AZURE.IMPORTANT] Wprowadzenie do sieci CDN działanie, a także wyświetlić listę funkcji zobacz [Omówienie CDN](./cdn-overview.md).

## <a name="create-a-new-cdn-profile"></a>Utwórz nowy profil sieci CDN

Profil CDN jest zbiorem CDN punkty końcowe.  Każdy profil zawiera co najmniej jeden CDN punktów końcowych.  Możesz organizowanie CDN punktów końcowych przez internet domeny, aplikacji sieci web lub niektóre inne kryteria, za pomocą profili.

> [AZURE.NOTE] Domyślnie pojedynczej subskrypcji Azure jest ograniczone do ośmiu profile CDN. Każdy profil CDN jest ograniczone do dziesięciu CDN punktów końcowych.
>
> Cennik CDN jest stosowana na poziomie profilu CDN. Jeśli chcesz używać różnych poziomów cennik Azure CDN, konieczne będzie CDN profili.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Utwórz nowy punkt końcowy sieci CDN

**Aby utworzyć nowy punkt końcowy sieci CDN**

1. W [Azure Portal](https://portal.azure.com)przejdź do swojego profilu CDN.  Możesz może mieć przypięta go do pulpitu nawigacyjnego w poprzednim kroku.  Jeśli użytkownik nie możesz znaleźć go, klikając pozycję **Wyszukaj**, a następnie **CDN profile**, a następnie kliknij na chcesz dodać punkt końcowy do profilu.

    Zostanie wyświetlona karta CDN profilu.

    ![Sieci CDN profilu][cdn-profile-settings]

2. Kliknij przycisk **Dodaj punkt końcowy** .

    ![Dodawanie przycisku punktu końcowego][cdn-new-endpoint-button]

    Zostanie wyświetlona karta **Dodaj punktu końcowego** .

    ![Dodawanie karta punktu końcowego][cdn-add-endpoint]

3. Wprowadź **nazwę** dla tego punktu końcowego CDN.  Ta nazwa będzie używana dostęp do swojej pamięci podręcznej zasobów w domenie `<endpointname>.azureedge.net`.

4. Na liście rozwijanej **Origin typ** wybierz typ pochodzenia.  Wybierz pozycję **miejsca do magazynowania** w ramach konta usługi Azure miejsca do magazynowania, **Usługa w chmurze** dla usługi Azure w chmurze, **Aplikacji sieci Web** dla aplikacji sieci Web Azure lub **origin niestandardowe** dla każdego innego źródła serwer sieci web publicznie (obsługiwanych w Azure lub innym programie) i.

    ![Typ origin CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
        
5. Na liście rozwijanej **Origin hostname** zaznacz lub wpisz nazwę swojej domeny pochodzenia.  Menu rozwijane spowoduje wyświetlenie listy wszystkich dostępnych miejsc pochodzenia typu określony w kroku 4.  W przypadku wybrania *origin niestandardowe* jako usługi **Wpisz Origin**zostaną wpisane domena pochodzenia niestandardowe.

6. W polu tekstowym **ścieżka Origin** wprowadź ścieżkę do zasobów do pamięci podręcznej lub pozostaw puste pole, aby umożliwić pamięci podręcznej wszystkich zasobów w domenie, które określony w kroku 5.

7. **Nagłówek hosta Origin**wprowadź nagłówek hosta ma CDN do wysłania z każdego żądania lub pozostaw nazwę domyślną.

    > [AZURE.WARNING] Niektóre typy źródeł, takich jak magazyn Azure i aplikacje sieci Web, wymagają nagłówka hosta, aby pasował do domeny punktu początkowego. Jeśli nie masz pochodzenie wymaga nagłówek hosta różnych ze swojej domeny, należy pozostawić wartość domyślna.

8. **Protokół** i **Origin port**Określ protokołów i portów używać do uzyskiwania dostępu do zasobów na początku.  Co najmniej jeden protokół (HTTP lub HTTPS) musi być zaznaczone.
    
    > [AZURE.NOTE] **Origin port** dotyczy tylko co portu używa punktu końcowego do pobierania informacji z punktu początkowego.  Punkt końcowy się tylko będzie dostępny do zakończenia klientów na domyślny HTTP i HTTPS porty 80 i 443, bez względu na **pochodzenie port**.  
    >
    > Punkty końcowe **CDN Azure z Akamai** gamę port TCP dla miejsc pochodzenia nie jest możliwe.  Aby uzyskać listę portów pochodzenia, które nie są dozwolone zobacz [CDN Azure z Akamai dozwolone Origin porty](https://msdn.microsoft.com/library/mt757337.aspx).  
    >
    > Uzyskiwanie dostępu do sieci CDN zawartości przy użyciu protokołu HTTPS występują następujące ograniczenia:
    > 
    > - Należy użyć certyfikat SSL dostarczony przez CDN. Certyfikaty innych firm nie są obsługiwane.
    > - Należy użyć domeny dostarczonego przez CDN (`<endpointname>.azureedge.net`) do zawartości HTTPS programu access. Obsługa HTTPS nie jest dostępna dla nazwy domeny niestandardowej (rekordów CNAME), ponieważ CDN nie obsługuje niestandardowych certyfikatów w tej chwili.

9. Kliknij przycisk **Dodaj** , aby utworzyć nowy punkt końcowy.

10. Po utworzeniu punkt końcowy zostanie wyświetlony na liście punkty końcowe w profilu. Widok listy zawiera adres URL, który umożliwia dostęp do zawartości pamięci podręcznej, a także domenie pochodzenia.

    ![Punkt końcowy sieci CDN][cdn-endpoint-success]

    > [AZURE.IMPORTANT] Punkt końcowy nie natychmiast będą dostępne do użycia, jak czas rejestracji propagowanie za pośrednictwem sieci CDN.  Profilów <b>CDN Azure z Akamai</b> propagowanie zakończy się zazwyczaj w ciągu minuty.  Profilów <b>CDN Azure z Verizon</b> propagowanie zakończy się zazwyczaj w ciągu 90 minut, ale w niektórych przypadkach może trwać dłużej.
    >    
    > Użytkownicy, którzy próbie użycia nazwy domeny sieci CDN przed konfiguracji punktu końcowego jest propagowana do punkty obecności otrzymają kody odpowiedzi HTTP 404.  Ile zostało kilka godzin, ponieważ został utworzony punkt końcowy i nadal błędzie 404 odpowiedzi, zobacz [Rozwiązywanie problemów z sieci CDN punkty końcowe zwracanie statusy 404](cdn-troubleshoot-endpoint.md).


##<a name="see-also"></a>Zobacz też
- [Sterowanie buforowania zachowanie żądania z ciągi kwerend](cdn-query-string.md)
- [Jak do zawartości sieci CDN mapy niestandardowej domeny](cdn-map-content-to-custom-domain.md)
- [Wstępnie ładowania zasobów dla punktu końcowego Azure CDN](cdn-preload-endpoint.md)
- [Trwałe usuwanie punktu końcowego Azure CDN](cdn-purge-endpoint.md)
- [Rozwiązywanie problemów z punktów końcowych CDN zwracanie statusy 404](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
