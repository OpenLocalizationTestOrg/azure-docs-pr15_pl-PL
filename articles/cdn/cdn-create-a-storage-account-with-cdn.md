<properties
    pageTitle="Integracja z konta miejsca do magazynowania z sieci CDN | Microsoft Azure"
    description="Informacje o sposobie dostarczania zawartości wysokiej przepustowości przez buforowanie obiektów blob z magazynu Azure za pomocą sieci dostarczania zawartości Azure (CDN)."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="integrate-a-storage-account-with-cdn"></a>Integracja z konta miejsca do magazynowania z sieci CDN

Sieci CDN można włączyć do zawartości pamięci podręcznej w magazynie Azure. Zapewnia on programista globalnej rozwiązanie dostarczania zawartości wysokiej przepustowości przez buforowanie obiektów blob i zawartość statyczną wystąpień obliczeń w węzłach fizycznie w Stanach Zjednoczonych, Europa, Azja, Australia i Ameryki Południowej.


## <a name="step-1-create-a-storage-account"></a>Krok 1: Utwórz konto miejsca do magazynowania

Poniższa procedura umożliwia utworzenie nowego konta miejsca do magazynowania dla subskrypcji Azure. Konto miejsca do magazynowania zapewnia dostęp do usług Azure magazynu. Konto miejsca do magazynowania reprezentuje najwyższego poziomu w obszarze nazw dostęp do wszystkich składników usługi Azure miejsca do magazynowania: Blob usługi, kolejki usług i usług tabeli. Aby uzyskać więcej informacji zapoznaj się z [Wprowadzenie do programu Microsoft Azure miejsca do magazynowania](../storage/storage-introduction.md).

Aby utworzyć konto miejsca do magazynowania, musi być administratorem usługi lub administratorem Współtworzenie skojarzone subskrypcji.

> [AZURE.NOTE] Istnieje kilka metod, których można użyć do utworzenia konta miejsca do magazynowania, takich jak Azure Portal i programu Powershell.  Ten samouczek możemy obsługiwać za pomocą Azure Portal.  

**Aby utworzyć konto miejsca do magazynowania dla subskrypcji usługi Azure**

1.  Zaloguj się do [portalu Azure](https://portal.azure.com).
2.  W lewym górnym rogu wybierz pozycję **Nowy**. W oknie dialogowym **Nowy** wybierz **danych + miejsca do magazynowania**, a następnie kliknij pozycję **konto miejsca do magazynowania**.

    Zostanie wyświetlona karta **Tworzenie konta miejsca do magazynowania** .

    ![Tworzenie konta miejsca do magazynowania][create-new-storage-account]

4. W polu **Nazwa** wpisz nazwę poddomeny. Ten wpis może zawierać małe litery i cyfry 3-24.

    Ta wartość staje się nazwa hosta w identyfikator URI, który odnosi się do obiektów Blob, kolejki lub tabeli zasobów dla subskrypcji. Aby adres zasobu kontenera w usłudze obiektów Blob, należy użyć identyfikatora URI w następującym formacie, gdzie * &lt;StorageAccountLabel&gt; * odwołuje się do wartości wpisane w polu **Wprowadź adres URL**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Ważne:** Etykieta adresu URL formularzami poddomeny konta miejsca do magazynowania URI i musi być unikatowa wśród wszystkich usług obsługiwanych w Azure.

    Ta wartość służy także jako nazwę konta magazynu w portalu lub gdy programowy dostęp do tego konta.

5. Pozostaw ustawienia domyślne dla **modelu wdrażania**, **rodzaju konta**, **wydajności**i **replikacji**. 

6. Wybierz **subskrypcję** , używanego konta miejsca do magazynowania z.

7. Wybierz lub Utwórz nową **Grupę zasobów**.  Aby uzyskać więcej informacji dotyczących grup zasobów zobacz [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md#resource-groups).

8. Wybierz lokalizację dla Twojego konta miejsca do magazynowania.

8. Kliknij przycisk **Utwórz**. Proces tworzenia konta miejsca do magazynowania może potrwać kilka minut.


## <a name="step-2-create-a-new-cdn-profile"></a>Krok 2: Tworzenie nowego profilu sieci CDN

Profil CDN jest zbiorem CDN punkty końcowe.  Każdy profil zawiera co najmniej jeden CDN punktów końcowych.  Możesz organizowanie CDN punktów końcowych przez internet domeny, aplikacji sieci web lub niektóre inne kryteria, za pomocą profili.

> [AZURE.TIP] Jeśli masz już profil CDN, który ma być używany dla tego samouczka, przejdź do [kroku 3](#step-3-create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="step-3-create-a-new-cdn-endpoint"></a>Krok 3: Utwórz nowy punkt końcowy sieci CDN

**Aby utworzyć nowy punkt końcowy CDN dla Twojego konta miejsca do magazynowania**

1. W [Portalu zarządzania Azure](https://portal.azure.com)przejdź do swojego profilu CDN.  Możesz może mieć przypięta go do pulpitu nawigacyjnego w poprzednim kroku.  Jeśli użytkownik nie możesz znaleźć go, klikając pozycję **Wyszukaj**, a następnie **CDN profile**, a następnie kliknij na chcesz dodać punkt końcowy do profilu.

    Zostanie wyświetlona karta CDN profilu.

    ![Sieci CDN profilu][cdn-profile-settings]

2. Kliknij przycisk **Dodaj punkt końcowy** .

    ![Dodawanie przycisku punktu końcowego][cdn-new-endpoint-button]

    Zostanie wyświetlona karta **Dodaj punktu końcowego** .

    ![Dodawanie karta punktu końcowego][cdn-add-endpoint]

3. Wprowadź **nazwę** dla tego punktu końcowego CDN.  Ta nazwa będzie używana dostęp do swojej pamięci podręcznej zasobów w domenie `<endpointname>.azureedge.net`.

4. Na liście rozwijanej **Typ Origin** wybierz *miejsca do magazynowania*.  

5. Na liście rozwijanej **Origin hostname** wybierz swoje konto miejsca do magazynowania.

6. Pozostaw ustawienia domyślne dla **Origin ścieżkę**, **Nagłówek hosta pochodzenia**oraz **portu protokół/pochodzenia**.  Należy określić co najmniej jeden protokół (HTTP lub HTTPS).

    > [AZURE.NOTE] Konfiguracja ta pozwala wszystkich kontenerów widoczna publicznie na koncie przechowywania w pamięci podręcznej w sieci CDN.  Jeśli chcesz ograniczyć zakres do jednego kontenera, za pomocą **ścieżki Origin**.  Należy zauważyć, że kontener musi mieć jego widoczność Ustawianie do publicznej.

7. Kliknij przycisk **Dodaj** , aby utworzyć nowy punkt końcowy.

8. Po utworzeniu punkt końcowy zostanie wyświetlony na liście punkty końcowe w profilu. Widok listy zawiera adres URL, który umożliwia dostęp do zawartości pamięci podręcznej, a także domenie pochodzenia.

    ![Punkt końcowy sieci CDN][cdn-endpoint-success]

    > [AZURE.NOTE] Punkt końcowy nie natychmiast będą dostępne do użycia.  Może potrwać do 90 minut rejestracji propagowanie za pośrednictwem sieci CDN. Użytkownicy, którzy próbie użycia nazwy domeny sieci CDN od razu może zostać wyświetlony kod stanu 404 do momentu ich zawartość jest dostępna za pośrednictwem sieci CDN.


## <a name="step-4-access-cdn-content"></a>Krok 4: Dostępu do zawartości sieci CDN

Aby uzyskać dostęp do zawartości pamięci podręcznej na CDN, użyj adresu URL CDN opisane w portalu. Adres pamięci podręcznej obiektów blob będzie podobny do następującego:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [AZURE.NOTE] Po CDN dostęp do konta miejsca do magazynowania lub hostowanej usługi, wszystkie obiekty publicznie kwalifikuje się do sieci CDN krawędzi pamięci podręcznej. Po zmodyfikowaniu obiektu, który jest obecnie w pamięci podręcznej CDN nową zawartość nie będzie dostępna za pośrednictwem sieci CDN aż CDN Odświeża zawartość po wygaśnięciu zawartości time to live okresu buforowania.

## <a name="step-5-remove-content-from-the-cdn"></a>Krok 5: Usuwanie zawartości z sieci CDN

Jeśli nie chcesz już pamięci podręcznej obiektu w zawartości sieci dostarczania (CDN) Azure, można wykonać jedną z następujących czynności:

-   Można wprowadzić kontenerze prywatna, zamiast podmiot. Aby uzyskać więcej informacji, zobacz [Zarządzanie anonimowe dostęp do odczytu kontenerów i blob](../storage/storage-manage-access-to-resources.md) .
-   Możesz wyłączyć lub usunąć punkt końcowy CDN za pomocą portalu zarządzania.
-   Można modyfikować usługi hostowanej przestanie odpowiadać na żądania obiektu.

Obiekt już przechowywanych w pamięci podręcznej w sieci CDN pozostaną pamięci podręcznej do momentu upływem okresu time to live obiektu lub punkt końcowy zostanie usunięte. Po wygaśnięciu okresu time to live CDN sprawdza czy punkt końcowy CDN jest nadal ważna i nadal anonimowo dostępne obiektu. Jeśli nie jest dostępne, obiekt nie jest już być buforowane.


## <a name="additional-resources"></a>Dodatkowe zasoby

-   [Jak do zawartości sieci CDN mapy niestandardowej domeny](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png
