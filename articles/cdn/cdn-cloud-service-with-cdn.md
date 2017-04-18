<properties
    pageTitle="Integracja usługi w chmurze z Azure CDN | Microsoft Azure"
    description="Samouczek opisujący sposób wdrażania usługi w chmurze służy zawartości z zintegrowane końcowy Azure CDN"
    services="cdn, cloud-services"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor="tysonn"/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="intro"></a>Integracja usługi w chmurze z Azure CDN

Usługa w chmurze można zintegrować z CDN Azure, obsługujących zawartość z lokalizacji usługi w chmurze. Ta metoda zapewnia następujące korzyści:

- Łatwe wdrażanie i aktualizowanie obrazów, skrypty i arkusze stylów w katalogu usługi cloud projektu
- Łatwo uaktualnić pakiety NuGet w swojej usługi w chmurze, takiej jak jQuery lub uruchamiania wersji
- Zarządzanie aplikacji sieci Web i usługi obsługiwane CDN wszystkie zawartości z tego samego interfejsu programu Visual Studio
- Ujednolicony wdrożenie przepływu pracy dla aplikacji sieci Web i obsługiwane CDN zawartości
- Integracja programu ASP.NET grupowania i minification z Azure CDN

## <a name="what-you-will-learn"></a>Będzie więcej ##

W tym samouczku dowiesz się jak:

-   [Integracja punktu końcowego Azure CDN z usługi w chmurze oraz udostępniać zawartość statyczną na stronach sieci Web z sieci CDN Azure](#deploy)
-   [Konfigurowanie ustawień pamięci podręcznej na zawartość statyczną w usłudze w chmurze](#caching)
-   [Udostępniać zawartość z akcji kontroler za pośrednictwem sieci CDN Azure](#controller)
-   [Służą połączone i minified zawartości za pośrednictwem sieci CDN Azure przy zachowaniu skryptu debugowania w programie Visual Studio](#bundling)
-   [Konfigurowanie alternatywnej skryptów i arkuszy CSS po sieci CDN Azure jest w trybie offline](#fallback)

## <a name="what-you-will-build"></a>Zostanie utworzona ##

Wdrażania rolę chmury usługi sieci Web przy użyciu domyślnego szablonu ASP.NET MVC, Dodaj kod, aby udostępniać zawartość z zintegrowane CDN Azure, takich jak obraz, wyniki działania kontroler i domyślnych plików CSS i JavaScript i także napisać kod, aby skonfigurować alternatywnych mechanizm wiązki obsługiwane w przypadku, gdy CDN jest w trybie offline.

## <a name="what-you-will-need"></a>Co będzie potrzebne ##

Ten samouczek ma następujące wymagania:

-   Aktywne [konto Microsoft Azure](/account/)
-   Visual Studio 2015 z [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [AZURE.NOTE] Potrzebne jest konto Azure do użycia tego samouczka:
> + Możesz [otworzyć konto Azure bezpłatnie](/pricing/free-trial/) — pobranie środków Umożliwia wypróbowanie płatnych usług Azure, a nawet w przypadku, gdy są używane nawet przechowujesz konta i użyj wolny Azure usług, takich jak witryny sieci Web.
> + Można [aktywować zalet subskrybentów MSDN](/pricing/member-offers/msdn-benefits-details/) — subskrypcji MSDN i zapewnia środków co miesiąc, używanej usługi Azure płatnej.

<a name="deploy"></a>
## <a name="deploy-a-cloud-service"></a>Wdrażanie usługi w chmurze ##

W tej sekcji będą wdrażanie domyślnego szablonu aplikacji ASP.NET MVC w Visual Studio 2015 do roli chmury usługi sieci Web, a następnie zintegrować z nowy punkt końcowy CDN. Postępuj zgodnie z instrukcjami poniżej:

1. W Visual Studio 2015 r., tworzyć nowe usługi w chmurze Azure na pasku menu, przechodząc do **Plik > Nowy > Projekt > chmury > Usługa w chmurze Azure**. Nadaj plikowi nazwę i kliknij **przycisk OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. Wybierz **Rolę sieci Web programu ASP.NET** , a następnie kliknij pozycję **>** przycisk. Kliknij przycisk OK.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. Zaznacz **MVC** , a następnie kliknij **przycisk OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. Teraz publikować tej roli sieci Web w usłudze Azure chmury. Kliknij prawym przyciskiem myszy projektu usługi cloud i wybierz pozycję **Publikuj**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. Jeśli masz nie nastąpiło do Microsoft Azure, kliknij listę rozwijaną **Dodaj konto...** i kliknij polecenie **Dodaj konto** .

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. Na stronie logowania Zaloguj się przy użyciu konta Microsoft, którego użyto do aktywować konto Azure.
7. Po zalogowaniu się, kliknij przycisk **Dalej**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. Przy założeniu, że nie utworzono konto w chmurze usługi lub miejsca do magazynowania, Visual Studio pomoże Ci utworzyć obu. W oknie dialogowym **Tworzenie usługa w chmurze i konta** wpisz nazwę żądanej usługi, a następnie wybierz odpowiedni region. Następnie kliknij przycisk **Utwórz**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. Na stronie Ustawienia publikowania sprawdź konfigurację, a następnie kliknij pozycję **Publikuj**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)

    >[AZURE.NOTE] Proces publikowania dla usług w chmurze zajmuje dużo czasu. Włączanie Web wdrażanie dla wszystkich opcji role mogą być debugowania usługi cloud znacznie szybsze, dostarczając szybkie (ale tymczasowe) aktualizacje role w sieci Web. Aby uzyskać więcej informacji na temat tej opcji zobacz [Publikowanie usługi w chmurze za pomocą narzędzi Azure](http://msdn.microsoft.com/library/ff683672.aspx).

    **Microsoft Azure dziennik** pokazuje, że stan publikowania jest **wykonane**, utworzysz punkt końcowy CDN, który jest zintegrowany z tej usługi w chmurze.

    >[AZURE.WARNING] Jeśli po opublikowaniu usług w chmurze wdrożonym zostanie wyświetlony komunikat o błędzie, prawdopodobnie ponieważ korzysta z usługi w chmurze, który został wdrożony [gościa, za pomocą systemu operacyjnego, który nie zawiera programu .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  Wdrażając [.NET 4.5.2 jako zadanie uruchamiania](../cloud-services/cloud-services-dotnet-install-dotnet.md)można obejścia tego problemu.

## <a name="create-a-new-cdn-profile"></a>Utwórz nowy profil sieci CDN

Profil CDN jest zbiorem CDN punkty końcowe.  Każdy profil zawiera co najmniej jeden CDN punktów końcowych.  Możesz organizowanie CDN punktów końcowych przez internet domeny, aplikacji sieci web lub niektóre inne kryteria, za pomocą profili.

> [AZURE.TIP] Jeśli masz już profil CDN, który ma być używany dla tego samouczka, przejdź do [Utwórz nowy punkt końcowy CDN](#create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Utwórz nowy punkt końcowy sieci CDN

**Aby utworzyć nowy punkt końcowy CDN dla Twojego konta miejsca do magazynowania**

1. W [Portalu zarządzania Azure](https://portal.azure.com)przejdź do swojego profilu CDN.  Możesz może mieć przypięta go do pulpitu nawigacyjnego w poprzednim kroku.  Jeśli użytkownik nie możesz znaleźć go, klikając pozycję **Wyszukaj**, a następnie **CDN profile**, a następnie kliknij na chcesz dodać punkt końcowy do profilu.

    Zostanie wyświetlona karta CDN profilu.

    ![Sieci CDN profilu][cdn-profile-settings]

2. Kliknij przycisk **Dodaj punkt końcowy** .

    ![Dodawanie przycisku punktu końcowego][cdn-new-endpoint-button]

    Zostanie wyświetlona karta **Dodaj punktu końcowego** .

    ![Dodawanie karta punktu końcowego][cdn-add-endpoint]

3. Wprowadź **nazwę** dla tego punktu końcowego CDN.  Ta nazwa będzie używana dostęp do swojej pamięci podręcznej zasobów w domenie `<EndpointName>.azureedge.net`.

4. Na liście rozwijanej **Origin typ** wybierz pozycję *Usługa w chmurze*.  

5. Na liście rozwijanej **Origin hostname** wybierz pozycję Usługa w chmurze.

6. Pozostaw ustawienia domyślne dla **Origin ścieżkę**, **Nagłówek hosta Origin**i **Protokół/Origin portu**.  Należy określić co najmniej jeden protokół (HTTP lub HTTPS).

7. Kliknij przycisk **Dodaj** , aby utworzyć nowy punkt końcowy.

8. Po utworzeniu punkt końcowy zostanie wyświetlony na liście punkty końcowe w profilu. Widok listy zawiera adres URL, który umożliwia dostęp do zawartości pamięci podręcznej, a także domenie pochodzenia.

    ![Punkt końcowy sieci CDN][cdn-endpoint-success]

    > [AZURE.NOTE] Punkt końcowy nie natychmiast będą dostępne do użycia.  Może potrwać do 90 minut rejestracji propagowanie za pośrednictwem sieci CDN. Użytkownicy, którzy próbie użycia nazwy domeny sieci CDN od razu może zostać wyświetlony kod stanu 404 do momentu ich zawartość jest dostępna za pośrednictwem sieci CDN.

## <a name="test-the-cdn-endpoint"></a>Testowanie sieci CDN punktu końcowego

Gdy stan publikowania jest **wykonane**, Otwórz okno przeglądarki i przejdź do * *http://<cdnName>*.azureedge.net/Content/bootstrap.css**. Moje ustawienia ten adres URL jest:

    http://camservice.azureedge.net/Content/bootstrap.css

Które odpowiada następujące źródłowy adres URL w sieci CDN punkt końcowy:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Po przejściu do * *http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, w zależności od przeglądarki, zostanie wyświetlony monit na pobieranie i otwieranie bootstrap.css, dostarczonej razem z opublikowanych aplikacji sieci Web.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

Podobnie możesz uzyskać dostęp do dowolnego publicznie adresu URL w * *http://*&lt;NazwaUsługi >*.cloudapp.net/** bezpośrednio z sieci CDN punktu końcowego. Na przykład:

-   Plik js z ścieżce/Script
-   Dowolny plik zawartości z/Content ścieżki
-   Kontroler i działania
-   Po włączeniu ciągu kwerendy na punkt końcowy CDN dowolny adres URL z ciągi kwerend

W rzeczywistości, przy użyciu podanych konfiguracji mogą obsługiwać usługę całą chmurą od * *http://*&lt;cdnName >*.azureedge.net/**. Jeśli I przejdź do **http://camservice.azureedge.net/ ** uzyskać wynik akcji z Narzędzia główne i indeksu.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Oznacza to, jednak, że zawsze jest dobrym pomysłem (lub ogólnie dobrze) do obsługi usługi całą chmurą za pośrednictwem sieci CDN Azure. Ostrzeżenia, należą:

-   Ta metoda wymaga całej witryny, aby być publiczne, ponieważ Azure CDN nie może służyć dowolny prywatną zawartość w tej chwili.
-   Jeśli z jakiegoś powodu punkt końcowy CDN przechodzi do trybu offline, czy zaplanowanych lub błąd użytkownika usługi całą chmurą przechodzi do trybu offline, chyba że klienci mogą być przekierowywane do źródłowy adres URL * *http://*&lt;NazwaUsługi >*.cloudapp.net/**.
-   Nawet w przypadku ustawienia pamięci podręcznej Kontrolki niestandardowe (zobacz [Konfigurowanie opcji buforowania dla plików statycznych w usłudze w chmurze](#caching)) punkt końcowy CDN nie poprawia wydajności zawartości wysoce dynamiczne. Jeśli próbujesz ładowanie strony głównej od punktu końcowego sieci CDN jak przedstawiony powyżej, należy zauważyć, że zajęło co najmniej 5 sekund aby załadować domyślnej strony głównej po raz pierwszy, czyli dość proste strony. Załóżmy, co się stanie do obsługi klienta, jeśli ta strona zawiera zawartość dynamiczna, który należy zaktualizować co minutę. Serwowania zawartości dynamicznej z punkt końcowy CDN wymaga wygaśnięcie krótki pamięci podręcznej, co oznacza Chybienia w pamięci podręcznej częste w punkcie końcowym CDN. Spowoduje to powinna wydajność usługi w chmurze i defeats przeznaczenie CDN.

Alternatywny należy określić, jaka zawartość ma być służyć z Azure CDN na podstawie przez przypadek w usłudze w chmurze. W tym celu masz już widoczne jak uzyskać dostęp do pojedynczych plików zawartości od punktu końcowego CDN. I procedurach pokazano, jak obsługiwać akcję kontroler określone przez punkt końcowy CDN w [udostępniać zawartość z akcji kontroler za pośrednictwem sieci CDN Azure](#controller).

<a name="caching"></a>
## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Konfigurowanie opcji buforowania plików statycznych w usłudze w chmurze ##

Przy użyciu integracji Azure CDN w usłudze w chmurze można określić, jak ma zawartość statyczną pamięci podręcznej w sieci CDN punktu końcowego. Aby to zrobić, otwórz *plik Web.config* z Twojego projektu roli sieci Web (na przykład WebRole1) i dodawanie `<staticContent>` element, aby `<system.webServer>`. XML poniższe konfiguruje pamięci podręcznej wygaśnie w ciągu 3 dni.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Po wykonaniu tej czynności wszystkich plików statycznych w usłudze w chmurze odbywa się w tej samej reguły w pamięci podręcznej CDN. Aby uzyskać większą kontrolę nad ustawień pamięci podręcznej Dodawanie pliku *Web.config* do folderu i dodawanie ustawień. Na przykład dodać plik *Web.config* do folderu *\Content* i zastąpić zawartości XML następujące czynności:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

To ustawienie powoduje, że wszystkie pliki statyczne z folderu *\Content* pamięci podręcznej przez 15 dni.

Aby uzyskać więcej informacji na temat konfigurowania `<clientCache>` elementu, zobacz [pamięci podręcznej klienta &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

W [udostępniać zawartość z akcji kontroler za pośrednictwem sieci CDN Azure](#controller)można także przedstawiono, jak można skonfigurować ustawienia pamięci podręcznej kontrolera wyniki akcji w pamięci podręcznej CDN.

<a name="controller"></a>
## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Udostępniać zawartość z akcji kontroler za pośrednictwem sieci CDN Azure ##

Rolę chmury usługi sieci Web można zintegrować z Azure CDN, jest stosunkowo łatwo udostępniać zawartość z akcji kontroler za pośrednictwem sieci CDN Azure. Inne niż służące do usługi w chmurze bezpośrednio za pośrednictwem sieci CDN Azure (pokazano powyżej), [Maarten Balliauw](https://twitter.com/maartenballiauw) pokazano, jak to zrobić z Wesołych kontroler MemeGenerator [Skracanie](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN)opóźnienia w sieci web za pomocą sieci CDN Azure. Można będzie po prostu odtworzyć go w tym miejscu.

Załóżmy, że w Twojej usłudze w chmurze chcesz wygenerować memes na podstawie małych obrazu Norris uchwytach (zdjęcie światło [Alan](http://www.flickr.com/photos/alan-light/218493788/)) tak jak poniżej:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Masz prostą `Index` akcję, która pozwala określić najwyższego przymiotników na obrazie, następnie generuje meme po przesyłać do akcji. Ponieważ jest Norris uchwytach, można oczekiwać strony, aby globalnie stają się wildly popularne. To jest dobrym przykładem serwowania półstrukturalnych dynamicznej zawartości z Azure CDN.

Wykonaj powyższe kroki, aby ta akcja kontroler konfiguracji:

1. W folderze *\Controllers* utworzenie nowego pliku CS o nazwie *MemeGeneratorController.cs* i zastąpić zawartość poniższy kod. Pamiętaj zamienić wyróżnione część nazwy CDN.  

        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;

        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();

                public ActionResult Index()
                {
                    return View();
                }

                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }

                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }

                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }

                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }

                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);

                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }

                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }

                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);

                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }

                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }

2. Kliknij prawym przyciskiem myszy w domyślnym `Index()` Akcja i wybierz pozycję **Dodaj widok**.

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  Zaakceptuj poniższe ustawienia i kliknij przycisk **Dodaj**.

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. Otwórz nowy *Views\MemeGenerator\Index.cshtml* i zastąpić poniższy kod HTML prosty składania najwyższego przymiotników zawartości:

        <h2>Meme Generator</h2>

        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>

5. Ponownie opublikuj usług w chmurze i przejdź do strony * *http://*&lt;NazwaUsługi >*.cloudapp.net/MemeGenerator/Index** w przeglądarce.

Po przesłaniu wartości formularza do `/MemeGenerator/Index`, `Index_Post` metoda akcji zwraca łącze do `Show` metody akcję z odpowiednich identyfikatorem wprowadzania danych. Po kliknięciu łącza zostanie wyświetlona następujący kod:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Jeśli dołączony do lokalnych debugowania zostanie wyświetlony pracę zwykłą debugowania w programie lokalne przekierowywania. Jeśli jest uruchomiony w usłudze w chmurze, następnie nastąpi przekierowanie do:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Które odpowiada następujące źródłowy adres URL w Twojej sieci CDN punkt końcowy:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Możesz użyć `OutputCacheAttribute` atrybutów na `Generate` metodę, aby określić, jak wynik działania powinny być buforowane, które będą przestrzegać Azure CDN. Poniższy kod określić wygaśnięcie pamięci podręcznej 1 godzina (3600 sekund).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Analogicznie możesz może służyć zawartości od wszelkich działań kontroler w usłudze w chmurze za pośrednictwem sieci CDN Azure, z odpowiednią opcję buforowania.

W następnej sekcji I procedurach pokazano, jak obsługiwać powiązane i minified skrypty i arkuszy CSS za pośrednictwem sieci CDN Azure.

<a name="bundling"></a>
## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>Integracja programu ASP.NET grupowania i minification z Azure CDN ##

Arkusze stylów CSS i skryptów są głównym kandydatów pamięci podręcznej Azure CDN i zmienianie rzadko. Serwowania całej ról w sieci Web za pośrednictwem sieci CDN Azure jest najprostszym sposobem integracji grupowania i minification z Azure CDN. Jednak podczas nie można to zrobić, I procedurach pokazano, jak to zrobić, gdy zachowanie środowiska programistyczne odpowiedniej ASP.NET grupowania i minification, takie jak:

-   Środowisko tryb debugowania profesjonalnych
-   Usprawniony wdrażania
-   Natychmiastowe aktualizacje klientów dotyczące uaktualnień wersji skryptu i arkuszy CSS
-   Mechanizm alternatywnych w przypadku niepowodzenia punkt końcowy sieci CDN
-   Minimalizowanie modyfikacja kodu

W programie project **WebRole1** , utworzony w [Integracja punkt końcowy Azure CDN z usługi Azure witryny sieci Web i służą zawartość statyczną na stronach sieci Web z sieci CDN Azure](#deploy), otwórz *App_Start\BundleConfig.cs* i zapoznaj się `bundles.Add()` metody połączenia.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

Pierwszy `bundles.Add()` instrukcja dodaje pakiet skryptu w katalogu wirtualnego `~/bundles/jquery`. Następnie należy otworzyć *Views\Shared\_Layout.cshtml* aby zobaczyć, jak renderowanie tagu pakietu. Być może znaleźć następujący wiersz kodu Razor:

    @Scripts.Render("~/bundles/jquery")

Po uruchomieniu tego kodu Razor w roli sieci Web Azure będą renderowane `<script>` znacznika pakietu skrypt podobny do następującego:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Jednak, gdy jest uruchamiana w programie Visual Studio, wpisując `F5`, jej spowoduje, że każdy plik skryptu pakietu pojedynczo (w przypadku powyżej, plik skryptu tylko jeden jest w pakiecie):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Umożliwia debugowania kodu JavaScript w środowisku rozwoju podczas zmniejszania połączenia równoczesne klienta (łączenie) i poprawę plik Pobierz wydajności (minification) w produkcji. Jest doskonałych funkcji w celu zachowania przy użyciu integracji Azure CDN. Ponadto, ponieważ pakiet renderowanych zawiera już automatycznie wygenerowanego wersja, mają być replikowane funkcja dlatego po zaktualizowaniu wersji dostępne za pośrednictwem NuGet go można aktualizować po stronie klienta tak szybko, jak to możliwe.

Postępuj zgodnie z instrukcjami poniżej integracji programu ASP.NET grupowania i minification z punktem końcowym sieci CDN.

1. Po powrocie do *App_Start\BundleConfig.cs*modyfikowanie `bundles.Add()` sposoby za pomocą różnych [konstruktora pakietu](http://msdn.microsoft.com/library/jj646464.aspx), która określa adres CDN. W tym celu należy zastąpić `RegisterBundles` definicja metody z następujący kod:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Pamiętaj zamienić `<yourCDNName>` z nazwą swojego CDN Azure.

    Prostymi słowami ustawiasz `bundles.UseCdn = true` i dodane starannie zredagowanej CDN adresu URL do każdego pakietu. Na przykład pierwszy konstruktora w kodzie:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

    jest taka sama jak:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

    Ten konstruktor informuje ASP.NET grupowania i minification do wyświetlania plików pojedynczego skryptu po debugowania lokalnie, ale dostępu skrypt w danym za pomocą określonego adresu sieci CDN. Należy jednak zwrócić uwagę dwie ważne cechy z tym starannie zredagowanej adresem URL CDN:

    -   Dla tego adresu URL CDN pochodzi `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, która jest faktycznie katalogu wirtualnego wiązki skrypt w usłudze w chmurze.
    -   Ponieważ w przypadku korzystania z konstruktora CDN tagu CDN dla pakietu nie zawiera już automatycznie wygenerowanego wersja renderowanych adres URL. Ciąg unikatowe wersji należy ręcznie wygenerować każdorazowo pakietu skrypt jest modyfikować tak, aby wymusić nie trafienia pamięci podręcznej w sieci CDN Azure. W tym samym czasie ten ciąg unikatowe wersji musi być stała za pośrednictwem życia wdrażania maksymalizować trafień w pamięci podręcznej w sieci CDN Azure po wdrożeniu pakietu.
    -   Ciąg kwerendy v = ściąga < W.X.Y.Z > z *Properties\AssemblyInfo.cs* w projekcie roli sieci Web. Możesz mieć wdrożenie przepływu pracy, który zawiera, zwiększając wersję zestawu w każdym publikowaniu Azure. Lub po prostu zmodyfikować *Properties\AssemblyInfo.cs* w projekcie, aby automatycznie zwiększane ciąg wersji, zawsze możesz utworzyć przy użyciu znaku wieloznacznego "*". Na przykład:

            [assembly: AssemblyVersion("1.0.0.*")]

        Strategii usprawnianie generuje unikatowy ciąg dla życia wdrożeniu będzie działać w tym miejscu.

3. Ponowne publikowanie usług w chmurze i uzyskać dostęp do strony głównej.

4. Wyświetlanie kodu HTML na stronie. Powinny być widoczne adres URL sieci CDN renderowanie ciągiem wersji unikatowe każdorazowo opublikować zmiany do usługi w chmurze. Na przykład:  

        ...

        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>

        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>

        ...

        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>

        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>

        ...

5. W programie Visual Studio debugowania usługi w chmurze w programie Visual Studio, wpisując `F5`.,

6. Wyświetlanie kodu HTML na stronie. Ty nadal widzisz każdy plik skryptu pojedynczo renderowanie, dzięki czemu będziesz mieć zgodne programem występują w programie Visual Studio.  

        ...

            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>

            <script src="/Scripts/modernizr-2.6.2.js"></script>

        ...

            <script src="/Scripts/jquery-1.10.2.js"></script>

            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>

        ...   

<a name="fallback"></a>
## <a name="fallback-mechanism-for-cdn-urls"></a>Mechanizm alternatywnych adresów URL sieci CDN ##

Gdy punkt końcowy Azure CDN kończy się niepowodzeniem z dowolnego powodu, ma strony sieci Web jako inteligentny uzyskać dostęp do serwera sieci Web origin jako opcja alternatywnych ładowania JavaScript lub początkowego. Jest poważne utracić obrazy w witrynie sieci Web z powodu niedostępności CDN, ale bardziej poważne utracić ważnych strony funkcjonalność skryptów i arkusze stylów.

Klasa [pakietu](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) zawiera właściwość o nazwie [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) umożliwia konfigurowanie alternatywnych mechanizm CDN błąd. Aby użyć tej właściwości, wykonaj następujące czynności:

1. W projekcie roli sieci Web otwórz *App_Start\BundleConfig.cs*, gdzie w każdej [konstruktora pakietu](http://msdn.microsoft.com/library/jj646464.aspx)dodano adres URL sieci CDN i wprowadzić następujące zmiany wyróżnione Dodawanie mechanizmu powrotu do wiązki domyślne:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                "~/Scripts/bootstrap.js",
                                "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Gdy `CdnFallbackExpression` jest nie zawiera wartości null, skrypt dodane do formatu HTML, aby sprawdzić, czy załadowany pakietu i, jeśli nie dostęp do pakietu bezpośrednio z serwerem sieci Web origin. Ta właściwość musi być ustawiona na wyrażenie języka JavaScript, która sprawdza, czy odpowiednich pakietu CDN jest ładowana poprawnie. Wyrażenie, aby przetestować każdego pakietu różni się stosownie do zawartości. Aby uzyskać wiązki domyślne powyżej:

    -   `window.jquery`jest zdefiniowana w js jquery-{wersji}
    -   `$.validator`jest zdefiniowana w jquery.validate.js
    -   `window.Modernizr`jest zdefiniowana w js modernizer-{wersji}
    -   `$.fn.modal`jest zdefiniowana w bootstrap.js

    Zwróć uwagę, że nie ustawił I CdnFallbackExpression dla `~/Cointent/css` pakietu. Jest to, ponieważ obecnie jest to [błąd w System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) wstawia go `<script>` znacznika alternatywnych CSS zamiast oczekiwanych `<link>` znacznik.

    Istnieje jednak warto [Powrót pakietu styl](https://github.com/EmberConsultingGroup/StyleBundleFallback) oferowanych przez [Grupy doradcze Członkowskimi](https://github.com/EmberConsultingGroup).

2. Aby zastosować obejście CSS, utworzenie nowego pliku CS w folderze *App_Start* projektu roli sieci Web o nazwie *StyleBundleExtensions.cs*i Zastąp zawartość [z GitHub kodu](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).

4. W *App_Start\StyleFundleExtensions.cs*Zmień nazwę obszaru nazw nazwę swojej roli sieci Web (np. **WebRole1**).

3. Wrócić do `App_Start\BundleConfig.cs` i modyfikowanie ostatniego `bundles.Add` instrukcji z wyróżnionym poniższy kod:  

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    Tej nowej metody rozszerzenia korzysta z tym samym ogólny obraz do dodania skryptu w formacie HTML, aby sprawdzić DOM dla nazwy klasy, nazwa reguły i wartość reguły zdefiniowane w pakiecie arkuszy CSS i serwer sieci Web origin kompetencjach, jeśli go nie powiedzie się znaleźć dopasowanie.

4. Publikowanie usług w chmurze ponownie i uzyskać dostęp do strony głównej.

5. Wyświetlanie kodu HTML na stronie. Należy odnaleźć wprowadzoną skryptów podobny do następującego:    

        ...

        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>

        ...

            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>

        ...


    Zauważ, że skrypt wprowadzoną dla pakietu CSS nadal zawiera wadliwe pozostałych z `CdnFallbackExpression` właściwość w wierszu:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Ponieważ pierwsza część, ale || wyrażenie zawsze zwraca wartość PRAWDA (w wierszu bezpośrednio powyżej), funkcja wywołania document.write() nigdy nie działają.

## <a name="more-information"></a>Więcej informacji ##
- [Omówienie sieci Azure dostarczania zawartości (CDN)](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [Za pomocą Azure CDN](cdn-create-new-endpoint.md)
- [Łączenie programu ASP.NET i Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
