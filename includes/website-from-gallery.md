Azure Marketplace udostępnia szeroką gamę aplikacji web popularne opracowane przez firmę Microsoft, innej firmy i inicjatywy oprogramowania Otwórz źródło. Aplikacje sieci Web utworzone na podstawie Azure Marketplace nie wymagają instalacji oprogramowania, poza przeglądarką używane do łączenia się z [Portal Azure Podgląd](http://go.microsoft.com/fwlink/?LinkId=529715). 

W tym samouczku dowiesz się:

- Jak utworzyć nową aplikację sieci web za pośrednictwem usługi Azure Marketplace.

- Jak wdrożyć aplikację sieci web za pośrednictwem portalu Podgląd Azure.
 
Będzie tworzyć blogu WordPress, która korzysta z szablonu domyślnego. Na poniższej ilustracji przedstawiono złożonym aplikacji:


![WordPress blog][13]

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="create-a-web-app-in-the-portal"></a>Tworzenie aplikacji sieci web w portalu

1. Zaloguj się do portalu Azure Podgląd.

2. Otwórz Azure Marketplace, klikając ikonę **Marketplace** :

    ![Ikona Marketplace][marketplace]

    Lub, klikając ikonę **Nowa** w prawym górnym rogu pulpitu nawigacyjnego i wybierając **Marketplace** u bottow listy.
    
    ![Utwórz nowe][5]
    
3. Zaznacz **Web + Mobile**. Wyszukaj **WordPress** i kliknij ikonę **WordPress** .

    ![WordPress z listy][7]
    
5. Po przeczytaniu opis aplikacji WordPress, wybierz pozycję **Utwórz**.

6. Kliknij w **aplikacji sieci Web**i podaj wartości wymaganych do konfigurowania aplikacji sieci web.
    
    ![Konfigurowanie aplikacji][8]

7. Kliknij w **bazie danych**i podaj wartości wymaganych do konfigurowania bazy danych MySQL. 

    ![Konfigurowanie bazy danych][database]

8. Podaj nazwę dla nowej grupy zasobów.

    ![Grupy zasobów][groupname]

8. W razie potrzeby kliknij **SUBSKRYPCJĘ**, a następnie określ subskrypcję za pomocą. 

7. Po zakończeniu definiowania aplikacji sieci web kliknij pozycję **Utwórz**i zaczekaj, aż zostanie utworzona nowej aplikacji sieci web.

   Po utworzeniu aplikacji zostanie wyświetlona grupa zasobów, zawierające aplikacji sieci web i bazy danych.

   ![Grupa Pokazywanie][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Uruchamianie i zarządzanie nimi aplikacji sieci web WordPress
    
1. Polecenie z nowej aplikacji sieci web, aby wyświetlić szczegółowe informacje o aplikacji.

    ![Uruchamianie pulpitu nawigacyjnego][10]

2. Na stronie **Essentials** kliknij łącze lub **Przejdź** pod **adres Url** , aby otworzyć strony powitalnej aplikacji sieci web.

    ![adres URL witryny][browse]

3. Jeśli nie zainstalowano WordPress, wprowadź odpowiednią konfigurację informacje wymagane przez WordPress, a następnie kliknij przycisk **Zainstaluj WordPress** finalizowanie konfiguracji i Otwórz stronę logowania aplikacji sieci web.

4. Kliknij przycisk **Zaloguj** i wprowadź poświadczenia.  

5. Masz nową aplikację sieci web WordPress, które wyglądają podobnie do poniższych aplikacji sieci web.    

    ![witryny WordPress][13]






[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png
