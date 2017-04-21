# <a name="using-cdn-for-azure"></a>Za pomocą sieci CDN dla Azure

Sieci dostarczania zawartości Azure (CDN) oferuje deweloperów rozwiązania globalnego dostarczania zawartości wysokiej przepustowości przez buforowanie obiektów blob i zawartość statyczną wystąpień obliczeń w węzłach fizycznie w Stanach Zjednoczonych, Europa, Azja, Australia i Ameryki Południowej. Listę bieżącej lokalizacji węzeł CDN zobacz [Azure CDN węzeł lokalizacje].

To zadanie obejmuje następujące kroki:

* [Krok 1: Utwórz konto miejsca do magazynowania](#Step1)
* [Krok 2: Utworzyć nowy punkt końcowy CDN dla Twojego konta miejsca do magazynowania](#Step2)
* [Krok 3: Uzyskiwania dostępu do zawartości sieci CDN](#Step3)
* [Krok 4: Usuwanie zawartości sieci CDN](#Step4)

Pamięci podręcznej danych Azure za pomocą sieci CDN zalety:

-   Poprawić ich wydajność i użytkownika możliwości obsługi użytkowników końcowych, którzy są daleko od źródło zawartości, a są używane aplikacje miejsce, w którym wiele "internet podróży" są wymagane do załadowania treści
-   Dużą skalę rozłożone lepiej obsługiwać chwilową wysokie obciążenie, powiedzieć na początku zdarzenia, takie jak produktu

Obecnych klientów sieci CDN za pomocą można teraz Azure CDN w [portal Azure klasyczny]. Sieci CDN to funkcja dodatku do subskrypcji i ma osobnych [rozliczeń planu].

<a id="Step1"> </a>
<h2>Krok 1: Utwórz konto miejsca do magazynowania</h2>

Poniższa procedura umożliwia utworzenie nowego konta miejsca do magazynowania dla subskrypcji Azure. Konto miejsca do magazynowania zapewnia dostęp do usług Azure magazynu. Konto miejsca do magazynowania reprezentuje najwyższego poziomu w obszarze nazw dostęp do wszystkich składników usługi Azure miejsca do magazynowania: Blob usługi, kolejki usług i usług tabeli. Aby uzyskać więcej informacji na temat usług Azure miejsca do magazynowania zobacz [Korzystanie z usług przechowywania Azure](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Aby utworzyć konto miejsca do magazynowania, musi być administratorem usługi lub administratorem współpracujących skojarzone subskrypcji.

> [AZURE.NOTE] Aby dowiedzieć się, jak tej operacji za pomocą interfejsu API zarządzania usługi Azure zobacz temat informacje [Utwórz konto miejsca do magazynowania](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) .

**Aby utworzyć konto miejsca do magazynowania dla subskrypcji usługi Azure**

1.  Zaloguj się do [portalu klasyczny Azure].
2.  W lewym dolnym rogu kliknij przycisk **Nowy**. W oknie dialogowym **Nowy** wybierz **Usług danych**, a następnie kliknij **miejsca do magazynowania**, następnie **Szybkiego tworzenia**.

    Zostanie wyświetlone okno dialogowe **Utwórz konto miejsca do magazynowania** .

    ![Tworzenie konta miejsca do magazynowania][create-new-storage-account]

4. W polu **adres URL** wpisz nazwę poddomeny. Ten wpis może zawierać z 3-24 małe litery i cyfry.

    Ta wartość staje się nazwa hosta w identyfikator URI, który odnosi się do obiektów Blob, kolejki lub tabeli zasobów dla subskrypcji. Aby adres zasobu kontenera w usłudze obiektów Blob, należy użyć identyfikatora URI w następującym formacie, gdzie * &lt;StorageAccountLabel&gt; * odwołuje się do wartości wpisane w polu **Wprowadź adres URL**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Ważne:** Etykieta adresu URL formularzami poddomeny konta miejsca do magazynowania URI i musi być unikatowa wśród wszystkich usług obsługiwanych w Azure.

    Ta wartość służy także jako nazwę konta magazynu w portalu lub gdy programowy dostęp do tego konta.

5.  Z listy rozwijanej **Region i koligacji grupy** zaznacz region lub grupa koligacji dla konta miejsca do magazynowania. Zaznacz grupę koligacji zamiast regionu, jeśli chcesz usług miejsca do magazynowania w tym samym centrum danych z innymi usługami Windows Azure, których używasz. Może zwiększyć wydajność i opłaty nie są poniesione w związku z wyjściowego.  

    **Uwaga:** Aby utworzyć grupę koligacji, Otwórz obszar **Ustawienia** portalu zarządzania, kliknij pozycję **grupy koligacji**, a następnie kliknij **Dodaj grupę koligacji** lub **Dodaj**. Można również utworzyć i zarządzanie grupami koligacji przy użyciu interfejsu API systemu Windows Azure usługi zarządzania. Aby uzyskać więcej informacji zobacz [operacje na grupach koligacji].

6. Na liście rozwijanej **Subskrypcja** Wybierz subskrypcję, używanego konta miejsca do magazynowania z.
7.  Kliknij przycisk **Utwórz konto miejsca do magazynowania**. Proces tworzenia konta miejsca do magazynowania może potrwać kilka minut.
8.  Aby Sprawdź, czy konto miejsca do magazynowania został utworzony pomyślnie, upewnij się, że konto pojawia się w przypadku elementów na liście do **przechowywania** ze stanem **Online**.

<a id="Step2"> </a>
<h2>Krok 2: Utworzyć nowy punkt końcowy CDN dla Twojego konta miejsca do magazynowania</h2>

Po CDN dostęp do konta miejsca do magazynowania lub hostowanej usługi, wszystkie obiekty publicznie kwalifikuje się do sieci CDN krawędzi pamięci podręcznej. Po zmodyfikowaniu obiektu, który jest obecnie w pamięci podręcznej CDN nową zawartość nie będzie dostępna za pośrednictwem sieci CDN aż CDN Odświeża zawartość po wygaśnięciu zawartości time to live okresu buforowania.

**Aby utworzyć nowy punkt końcowy CDN dla Twojego konta miejsca do magazynowania**

1. W [portalu klasyczny Azure], w okienku nawigacji kliknij pozycję **CDN**.

2. Na wstążce kliknij przycisk **Nowy**. W oknie dialogowym **Nowy** wybierz **Usług aplikacji**, a następnie **CDN**, a następnie **Szybkie tworzenie**.

3. Na liście rozwijanej **Domeny pochodzenia** wybierz konto miejsca do magazynowania, utworzonego w poprzedniej sekcji na liście kont dostępnego miejsca. 

4. Kliknij przycisk **Utwórz** , aby utworzyć nowy punkt końcowy.

5. Po utworzeniu punkt końcowy zostanie wyświetlony na liście punkty końcowe dla subskrypcji. Widok listy zawiera adres URL, który umożliwia dostęp do zawartości pamięci podręcznej, a także domenie pochodzenia. 

    Domena pochodzenia jest lokalizacja, z której CDN buforowanie zawartości. Domeny pochodzenia może być konto miejsca do magazynowania lub usługi w chmurze; Konto miejsca do magazynowania jest używane na potrzeby tego przykładu. Zawartość miejsca do magazynowania jest buforowany serwery graniczne zgodnie z określone ustawienie kontroli pamięci podręcznej, albo heurystykę domyślnego buforowania sieci. 


    > [AZURE.NOTE] Konfiguracja utworzone dla punktu końcowego natychmiast będą niedostępne; może potrwać do 60 minut propagowanie za pośrednictwem sieci CDN rejestracji. Użytkownicy, którzy próbie użycia nazwy domeny sieci CDN natychmiast może zostać wyświetlony kod stanu 400 (niewłaściwe żądanie) do momentu ich zawartość jest dostępna za pośrednictwem sieci CDN.

<a id="Step3"> </a>
<h2>Krok 3: Dostępu do zawartości sieci CDN</h2> 

Aby uzyskać dostęp do zawartości pamięci podręcznej na CDN, użyj adresu URL CDN opisane w portalu. Adres pamięci podręcznej obiektów blob będzie podobny do następującego:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>
<h2>Krok 4: Usuwanie zawartości z sieci CDN</h2>

Jeśli nie chcesz już pamięci podręcznej obiektu w zawartości sieci dostarczenia (CDN) Azure, można wykonać jedną z następujących czynności:

-   Dla obiektów blob platformy Azure możesz usunąć to z publicznej kontenera.
-   Można wprowadzić kontenerze prywatna, zamiast podmiot. Aby uzyskać więcej informacji, zobacz [Ograniczanie dostępu do kontenerów i obiektów blob](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) .
-   Możesz wyłączyć lub usunąć punkt końcowy CDN za pomocą portalu zarządzania.
-   Można modyfikować usługi hostowanej przestanie odpowiadać na żądania obiektu.

Obiekt już przechowywanych w pamięci podręcznej w sieci CDN pozostaną pamięci podręcznej do wygaśnięcia okresu czasu wygaśnięcia dla obiektu. Po wygaśnięciu okresu time to live CDN sprawdza czy punkt końcowy CDN jest nadal ważna i nadal anonimowo dostępne obiektu. Jeśli nie jest dostępne, obiekt nie jest już być buforowane.

Możliwość natychmiast usunąć zawartość nie jest obecnie obsługiwane w portalu zarządzania Azure. Skontaktuj się z [obsługuje Azure](https://azure.microsoft.com/support/options/) Jeśli chcesz od razu Wyczyść zawartość. 

## <a name="additional-resources"></a>Dodatkowe zasoby

-   [Jak utworzyć grupę koligacji platformy Azure]
-   [Jak: Zarządzanie kontami miejsca do magazynowania dla subskrypcji usługi Azure]
-   [Zarządzanie usługi interfejsu API — informacje]
-   [Jak do zawartości sieci CDN mapy niestandardowej domeny]

  [Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
  [Lokalizacje węzeł Azure CDN]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
  [Portal Azure klasyczny]: https://manage.windowsazure.com/
  [plan rozliczeń]: /pricing/calculator/?scenario=full
  [Jak utworzyć grupę koligacji platformy Azure]: http://msdn.microsoft.com/library/azure/ee460798.aspx
  [Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
  [Informacje o zarządzaniu usługi interfejsu API]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
  [Jak do zawartości sieci CDN mapy niestandardowej domeny]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
