<properties
   pageTitle="Tworzenie łączy w artykułach promocji cenowych" description="Wyjaśniono, jak crosslinks w promocji cenowych kodu." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# <a name="linking-guidance-for-azure-technical-content"></a>Łączenie wskazówki dotyczące Azure treść techniczna

### <a name="links-from-one-acom-article-to-another"></a>Łączy z jednego artykułu ACOM do innego

Aby utworzyć łącze w tekście z artykułu techniczne ACOM na inny artykuł techniczne ACOM, użyj następującej składni łącza:  

- Artykuł w łączy katalogu usługi, na inny artykuł w tym samym katalogu usługi:

  `[link text](article-name.md)`

- Łącza do artykułów z podkatalogów usług do artykułu w katalogu głównym:

  `[link text](../article-name.md)`

- Artykuł w obszarze łączy katalogu głównego do artykułu podkatalogów usługi: 

  `[link text](./service-directory/article-name.md)`

- Artykuł w usługi podkatalogów łącza do artykułów w innej podkatalogów usługi:

  `[link text](../service-directory/article-name.md)`
 

## <a name="links-to-anchors"></a>Łącza do zakotwiczenia

Nie musisz utworzyć zakotwiczenia — są generowane automatycznie na czas dla wszystkich nagłówków H2 publikowania. Jedynym elementem, które trzeba wykonać jest tworzenie łączy do sekcji H2.

- Aby utworzyć łącze do nagłówka w ramach tego samego artykułu:

  `[link](#the-text-of-the-H2-section-separated-by-hyphens)`  
  `[Create cache](#create-cache)`

- Aby utworzyć łącze do kotwicy w artykule innego w tym samym podkatalogów:

  `[link text](article-name.md#anchor-name)`
  `[Configure your profile](media-services-create-account.md#configure-your-profile)`

- Aby utworzyć łącze do kotwicy w innym podkatalogów usługi:

  `[link text](../service-directory/article-name.md#anchor-name)`
  `[Configure your profile](../service-directory/media-services-create-account.md#configure-your-profile)`

Jednym ze sposobów zautomatyzować tworzenie łączy w artykuły do kotwicy generowane automatycznie łączy jest [MarkdownAnchorLinkGenerator — narzędzie do generowania kotwicy łącza ACOM we właściwym formacie](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/ACOMMarkdownAnchorLinkGenerator).

## <a name="links-from-includes"></a>Zawiera łącza z

Ponieważ obejmują pliki znajdują się w innym katalogu, będzie konieczne używanie już ścieżek względnych, tak jak pokazano poniżej. Aby utworzyć łącze do artykułu z pliku dołączanego, należy użyć następującego formatu:

    [link text](../articles/service-folder/article-name.md)
    
Dowiedz się więcej o używaniu pliku dołączenia w [wskazówki rozszerzenia promocji cenowych niestandardowe](custom-markdown-extensions.md#includes).

## <a name="links-in-selectors"></a>Łącza w selektory

Jeśli masz selektory, które są osadzone w polu Dołącz, należy użyć tego rodzaju łączenia: 

    > [AZURE. SELEKTOR listy (Dropdown1 | Dropdown2)]     -  [(Tekst1 | Example1)](../articles/service-folder/article-name1.md)
    - [(Tekst1 | Przykład2)](../articles/service-folder/article-name2.md)
    - [(Tekst2 | Example3)](../articles/service-folder/article-name3.md)
    - [(Tekst2 | Example4)](../articles/service-folder/article-name4.md)


## <a name="reference-style-links"></a>Styl odwołania łączy

Za pomocą stylów odnośniki aby ułatwić czytanie zawartości źródeł. Styl odnośniki Zamień składni łącze w tekście uproszczone składni, która umożliwia przenoszenie długie adresy na końcu tego artykułu. Oto przykład Daring Fireball:

Funkcje tekstowe w tekście

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

Łącza na końcu tego artykułu:

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

Upewnij się, że uwzględniane miejsce po dwukropek, przed łącze. Podczas tworzenia łącza do innych artykuły techniczne, Jeśli zapomnisz uwzględniane miejsce, w artykule opublikowanych będzie podzielony łącze. 

## <a name="link-to-acom-pages-that-are-not-part-of-the-technical-documentation-set"></a>Łącze do strony ACOM, które nie są częścią zestawu dokumentów technicznych

Aby utworzyć łącze do strony w ACOM (na przykład cennik strony, strona Umowa dotycząca poziomu usług lub pozostałej zawartości, która nie jest artykuł dokumentacja), należy użyć bezwzględny adres URL, ale pominąć ustawień regionalnych. W tym miejscu celem jest działające łącza w GitHub oraz w witrynie renderowanych:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)


## <a name="link-to-msdn-or-technet"></a>Łącze do witryny MSDN lub TechNet

Jeśli chcesz utworzyć łącze do witryny MSDN lub TechNet, użyj pełnego łącza do tematu i Usuń en-us ustawień regionalnych języka za pomocą łącza. 

### <a name="use-friendly-link-text-for-all-links"></a>Używanie tekstu łącza przyjazne dla wszystkich łączy

Wyrazy, które możesz umieścić w łączu powinny być przyjazny — innymi słowy, powinien być normalny wyrazów angielskich lub tytułu strony, do którego jest tworzone połączenie. Nie należy używać "kliknij tutaj". Jest nieprawidłowe dla funkcji optymalizacji aparatu wyszukiwania i odpowiednio nie opisują docelowej.

**Naprawianie:**

- `For more information, see the [contributor guide index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`

- `For more details, see the [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx) reference.`

**Niepoprawne:**

- `For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).`

- `For more information, click [here](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`


## <a name="fwlinks"></a>Linki FW

Unikaj linki FW (system przekierowania) w zawartości azure.microsoft.com. Powinny być używane tylko jako ostatniej z możliwości gdy trzeba utworzyć łącze do strony adresu URL, którego nie znasz jeszcze. Prawie nigdy nie są potrzebne. Dla ACOM możesz zdefiniować nazwę pliku, dzięki czemu łatwo ustalisz, będzie wyprzedzeniem. Dla tematu biblioteki, który nie został jeszcze opublikowany można utworzyć łącze, które używa tematu GUID, dzięki czemu nie trzeba używać łącze FW.

Jeśli na stronie sieci web, należy użyć łącze FW, należy dodać parametr P nawiązać stałe Przekierowanie:

    http://go.microsoft.com/fwlink/p/?LinkId=389595

Po wklejeniu docelowy adres URL do narzędzia łącze FW Pamiętaj, aby usunąć ustawienia regionalne, jeśli łącze docelowej jest biblioteka ACOM, lub w witrynie MSDN lub TechNet.

## <a name="remember-the-azure-library-chrome"></a>Pamiętaj chrome biblioteki Azure!
Jeśli chcesz utworzyć łącze do tematu Azure biblioteki, który znajduje się w [tym węźle](https://msdn.microsoft.com/library/azure), pamiętaj, aby określić Azure chrome w polu łącze (/ azure /). Azure chrome udostępnia opcje nawigacji ACOM i jest wyświetlana tylko Azure zawartość biblioteki w witrynie MSDN. Łącze prawidłowo zakresu wygląda następująco:

    http://msdn.microsoft.com/library/azure/dd163896.aspx

W przeciwnym razie strony są wyświetlane w standardowym widoku w witrynie MSDN z całego drzewa w witrynie MSDN wyświetlane.

### <a name="contributors-guide-links"></a>Współautorzy przewodnik łącza

- [Artykuł Omówienie](./../README.md)
- [Indeks artykułów ze wskazówkami dotyczącymi](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
