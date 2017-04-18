<properties
    pageTitle="Konfigurowanie nazwy domeny dla punkt końcowy magazyn obiektów Blob | Microsoft Azure"
    description="Dowiedz się, jak mapowanie domeny niestandardowej użytkownika do punktu końcowego magazyn obiektów Blob dla konta Azure miejsca do magazynowania w portalu klasyczny Azure."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Konfigurowanie niestandardowej nazwy domeny dla usługi punktu końcowego usługi Magazyn obiektów Blob

## <a name="overview"></a>Omówienie

Możesz skonfigurować domenę niestandardową do uzyskiwania dostępu do danych typu blob na koncie Azure miejsca do magazynowania. Domyślny punkt końcowy dla magazyn obiektów Blob `<storage-account-name>.blob.core.windows.net`. Jeśli mapowania niestandardowej domeny i poddomeny, takiej jak **www.contoso.com** punkt końcowy obiektów blob dla Twojego konta miejsca do magazynowania, a następnie usługi Użytkownicy mogą również dostęp do danych obiektów blob na koncie miejsca do magazynowania przy użyciu tej domeny.

>[AZURE.IMPORTANT] Magazyn Azure nie obsługuje jeszcze HTTPS z domen niestandardowych. Pracujemy pamiętać, że klienci też chcą korzystać z tej funkcji i będzie on dostępny w przyszłej wersji.

Istnieją dwa sposoby wskaż punkt końcowy obiektów blob dla Twojego konta magazynu domeny niestandardowej. Najłatwiejszym sposobem jest do utworzenia rekordu CNAME, mapowanie niestandardowej domeny i poddomeny do punktu końcowego obiektów blob. Rekord CNAME jest funkcją DNS, która map domeny źródłowej w domenie docelowej. W tym przypadku domena źródłowa jest własnej domeny i poddomeny — należy zauważyć, że poddomeny nie zawsze jest wymagane. W domenie docelowej jest punktu końcowego usługi obiektów Blob usługi.

Proces mapowania domeny niestandardowej na punkt końcowy obiektów blob można jednak spowoduje krótki okres przestoje dla domeny podczas rejestrujesz się domeny w [Klasycznym Portal Azure](https://manage.windowsazure.com). Jeśli własnej domeny jest obecnie obsługiwane aplikacji z umowy poziomu usług (SLA) wymaga, aby nie być brak przestojów, można użyć poddomeny Azure **asverify** zapewnienie krok rejestracji pośrednie, tak aby użytkownicy będą mieli dostęp do swojej domeny podczas mapowania ma miejsce DNS.

W poniższej tabeli pokazano przykładowe adresy URL do uzyskiwania dostępu do danych typu blob na koncie miejsca do magazynowania o nazwie **mystorageaccount**. Niestandardowej domeny zarejestrowane dla konta miejsca do magazynowania jest **www.contoso.com**:

Typ zasobu|Formatów adresu URL
---|---
Konto miejsca do magazynowania|**Domyślny adres URL:** http://mystorageaccount.blob.core.windows.net<p>**Adres URL domeny niestandardowe:** http://www.contoso.com</td>
Obiektów blob|**Domyślny adres URL:** http://mystorageaccount.blob.core.windows.net/mycontainer/myblob<p>**Adres URL domeny niestandardowe:** http://www.contoso.com/mycontainer/myblob
Kontener główny|**Domyślny adres URL:** http://mystorageaccount.blob.core.windows.net/myblob lub http://mystorageaccount.blob.core.windows.net/$ główny myblob<p>**Adres URL domeny niestandardowe:** http://www.contoso.com/myblob lub http://www.contoso.com/$ główny myblob

## <a name="register-a-custom-domain-for-your-storage-account"></a>Zarejestruj się domeny niestandardowej dla konta miejsca do magazynowania

Wykonaj poniższą procedurę, aby zarejestrować własnej domeny, jeśli nie masz obaw o możliwości domeny być krótko niedostępne dla użytkowników lub domeny niestandardowej nie jest obecnie obsługiwana aplikacja.

Jeśli własnej domeny jest obecnie obsługiwane aplikacji, która nie może mieć dowolną przestoje, następnie wykonaj procedurę opisaną w <a href="#register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain">rejestrze domeny niestandardowej dla konta miejsca do magazynowania przy użyciu poddomeny asverify pośredniczącego</a>.

Aby skonfigurować niestandardową nazwę domeny, musisz utworzyć nowy rekord CNAME z rejestratorem Twojej domeny. Rekord CNAME określa aliasu dla nazwy domeny; w tym przypadku mapowania adresu domeny niestandardowej punkt końcowy magazyn obiektów Blob dla Twojego konta miejsca do magazynowania.

Każdy rejestrator zawiera podobne, ale nieco inną metodę określania rekordu CNAME, ale pojęcia jest taki sam. Należy zauważyć, że wiele pakietów rejestracji podstawowe domeny nie oferuje konfiguracji systemu DNS, konieczne może być uaktualnienie pakietu rejestracji domeny przed utworzeniem rekordu CNAME.

1.  W [Klasycznym Portal Azure](https://manage.windowsazure.com)przejdź do karty **miejsca do magazynowania** .

2.  Na karcie **miejsca do magazynowania** kliknij nazwę konta magazynu, dla którego chcesz zamapować domeny niestandardowej.

3.  Kliknij kartę **Konfiguruj** .

4.  U dołu ekranu kliknij **Zarządzania domeną** , aby wyświetlić okno dialogowe **Zarządzanie domeny niestandardowe** . W polu tekst w górnej części okna dialogowego pojawi się informacje na temat tworzenia rekordu CNAME. Na potrzeby tej procedury Ignoruj tekst, który odwołuje się do poddomeny **asverify** .

5.  Zaloguj się do witryny sieci Web rejestratora DNS i przejdź do strony Zarządzanie DNS. Może to znaleźć w sekcji, takich jak **Nazwy domeny**, **DNS**lub **Zarządzanie serwerem nazw**.

6.  Znajdź sekcję do zarządzania rekordów CNAME. Może być konieczne przejdź do strony ustawień zaawansowanych i poszukaj wyrazów **CNAME** **Alias**lub **poddomeny**.

7.  Utwórz nowy rekord CNAME i podaj alias poddomena, na przykład **"www"** lub **zdjęcia**. Następnie należy podać nazwę hosta, czyli obiektów Blob końcowy usługi, w formacie **mystorageaccount.blob.core.windows.net** (gdzie **mystorageaccount** jest nazwę konta magazynu). Nazwa hosta, aby użyć jest dostarczany w polu Tekst okna dialogowego **Zarządzanie domeny niestandardowe** .

8.  Po utworzeniu rekordu CNAME powrócić do okna dialogowego **Zarządzanie domeny niestandardowej** , a następnie wprowadź nazwę domeny niestandardowej, w tym poddomeny, w polu **Niestandardowej nazwy domeny** . Na przykład jeśli Twoja domena jest **contoso.com** i usługi poddomena jest **"www"**, wprowadź **www.contoso.com**; Jeśli do poddomena jest **zdjęć**, wprowadź **photos.contoso.com**. Należy zauważyć, że poddomena jest wymagane.

9. Kliknij przycisk **Zarejestruj** , aby zarejestrować domeny niestandardowej.

    Jeśli rejestracja się pomyślnie, zostanie wyświetlony komunikat **z domeny niestandardowej jest aktywna**. Użytkownicy mogą teraz wyświetlanie obiektów blob danych w domenie niestandardowej, ile mają odpowiednie uprawnienia.

## <a name="register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain"></a>Zarejestruj się w niestandardowej domeny dla Twojego konta miejsca do magazynowania przy użyciu poddomeny asverify pośredniczącego

Ta procedura służy do rejestrowania własnej domeny jeśli domeny niestandardowej aktualnie obsługiwanych aplikacji z Umowa dotycząca poziomu usług, który wymaga, aby być brak przestojów. Tworząc CNAME, który wskazuje ze asverify. &lt;poddomeny&gt;. &lt;customdomain&gt; do asverify. &lt;storageaccount&gt;. blob.core.windows.net, przed zarejestrowaniem domeny w usłudze Azure. Następnie można utworzyć drugą CNAME wskazującego z &lt;poddomeny&gt;. &lt;customdomain&gt; do &lt;storageaccount&gt;. blob.core.windows.net, w którym ruch do Twojej domeny niestandardowej będzie kierowany do punktu końcowego usługi obiektów blob.

Poddomena asverify jest specjalne poddomeny rozpoznał Azure. Dołączając **asverify** do własnych poddomeny o zezwolenia Azure rozpoznanie domeny niestandardowej bez modyfikowania rekordu DNS dla domeny. Po zmodyfikowaniu rekordu DNS dla domeny, zostanie zamapowane do punktu końcowego obiektów blob bez przestojów.

1.  W [Klasycznym Portal Azure](https://manage.windowsazure.com)przejdź do karty **miejsca do magazynowania** .

2.  Na karcie **miejsca do magazynowania** kliknij nazwę konta magazynu, dla którego chcesz zamapować domeny niestandardowej.

3.  Kliknij kartę **Konfiguruj** .

4.  U dołu ekranu kliknij **Zarządzania domeną** , aby wyświetlić okno dialogowe **Zarządzanie domeny niestandardowe** . W polu tekst w górnej części okna dialogowego pojawi się informacje na temat tworzenia rekordu CNAME, przy użyciu poddomeny **asverify** .

5.  Zaloguj się do witryny sieci Web rejestratora DNS i przejdź do strony Zarządzanie DNS. Może to znaleźć w sekcji, takich jak **Nazwy domeny**, **DNS**lub **Zarządzanie serwerem nazw**.

6.  Znajdź sekcję do zarządzania rekordów CNAME. Może być konieczne przejdź do strony ustawień zaawansowanych i poszukaj wyrazów **CNAME** **Alias**lub **poddomeny**.

7.  Utwórz nowy rekord CNAME i podaj alias poddomeny, który zawiera poddomeny asverify. Na przykład poddomena zadanej będzie w formacie **asverify.www** lub **asverify.photos**. Następnie należy podać nazwę hosta, czyli obiektów Blob końcowy usługi, w formacie **asverify.mystorageaccount.blob.core.windows.net** (gdzie **mystorageaccount** jest nazwę konta magazynu). Nazwa hosta, aby użyć jest dostarczany w polu Tekst okna dialogowego **Zarządzanie domeny niestandardowe** .

8.  Po utworzeniu rekordu CNAME powrócić do okna dialogowego **Zarządzanie domeny niestandardowej** , a następnie wprowadź nazwę domeny niestandardowej w polu **Niestandardowej nazwy domeny** . Na przykład jeśli Twoja domena jest **contoso.com** i usługi poddomena jest **"www"**, wprowadź **www.contoso.com**; Jeśli do poddomena jest **zdjęć**, wprowadź **photos.contoso.com**. Należy zauważyć, że poddomena jest wymagane.

9.  Kliknij pole wyboru informacją, że **Zaawansowane: preregister mojej domeny niestandardowej za pomocą poddomeny "asverify"**.

10. Kliknij przycisk **Zarejestruj** , aby preregister domeny niestandardowej.

    Jeśli wstępnej rejestracji zakończyło się pomyślnie, zostanie wyświetlony komunikat **z domeny niestandardowej jest aktywna**.

11. W tym momencie domeny niestandardowej została zweryfikowana przez Azure, ale nie ruch do Twojej domeny są jeszcze kierowane do swojego konta miejsca do magazynowania. Aby ukończyć proces, powróć do witryny sieci Web rejestratora DNS i utwórz inny rekord CNAME map usługi poddomeny do punktu końcowego usługi obiektów Blob usługi. Na przykład określić poddomeny jako **ciągu "www"** lub **zdjęć**i hostname jako **mystorageaccount.blob.core.windows.net** (gdzie **mystorageaccount** jest nazwę konta magazynu). Ten krok zakończeniu rejestracji własnej domeny.

12. Na koniec możesz usunąć rekord CNAME utworzono przy użyciu **asverify**jako konieczne tylko jako etapie pośredniczącym.

Użytkownicy mogą teraz wyświetlanie obiektów blob danych w domenie niestandardowej, ile mają odpowiednie uprawnienia.

## <a name="verify-that-the-custom-domain-references-your-blob-service-endpoint"></a>Sprawdź, czy domeny niestandardowej odwołuje się do punktu końcowego usługi obiektów Blob

Aby sprawdzić, czy domeny niestandardowej w rzeczywistości są mapowane do punktu końcowego usługi obiektów Blob usługi, należy utworzyć obiektów blob w kontenerze publicznej w ramach konta miejsca do magazynowania. Następnie w przeglądarce sieci web za pomocą identyfikatora URI w następującym formacie dostępu to:

-   http://<*subdomain.customdomain*>/<*mycontainer*>/<*myblob*>

Za pomocą następujących identyfikatora URI może być na przykład uzyskać dostęp do formularza sieci web za pomocą niestandardowych poddomena **photos.contoso.com** i map do obiektów blob w kontenerze usługi **myforms** :

-   http://Photos.contoso.com/myforms/applicationform.htm

## <a name="unregister-a-custom-domain-from-your-storage-account"></a>Unregister domenę niestandardową z Twojego konta miejsca do magazynowania

Aby unregister domeny niestandardowej, wykonaj następujące czynności: 

1. Zaloguj się do [portalu klasyczny Azure](https://manage.windowsazure.com). 

2. W okienku nawigacji kliknij pozycję **miejsca do magazynowania**. 

3. Na stronie **Magazyn** kliknij nazwę konta magazynu do wyświetlania na pulpicie nawigacyjnym. 

5. Na wstążce kliknij przycisk **Zarządzania domeną**. 

6. W oknie dialogowym **Zarządzania domeną niestandardowy** kliknij pozycję **Unregister**. 


## <a name="additional-resources"></a>Dodatkowe zasoby

-   [Jak mapować domeny niestandardowe do punktu końcowego sieci dostarczania zawartości (CDN)](../cdn/cdn-map-content-to-custom-domain.md)
