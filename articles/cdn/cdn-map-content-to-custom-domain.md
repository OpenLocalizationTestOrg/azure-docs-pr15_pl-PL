<properties
     pageTitle="Jak mapowanie Azure dostarczania zawartości (CDN) sieci zawartości na niestandardowej domeny | Microsoft Azure"
     description="W tym temacie pokazano sposób Mapowanie zawartości CDN domeny niestandardowe."
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
     ms.topic="article"
    ms.date="07/28/2016"
     ms.author="casoper"/>

# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>Jak mapować domeny niestandardowe do punktu końcowego sieci dostarczania zawartości (CDN)
Można mapować domenę niestandardową do końcowego CDN w celu używania nazwy domeny w adresach URL zawartości pamięci podręcznej, a nie przy użyciu poddomeny azureedge.net.

Istnieją dwa sposoby mapowania domeny niestandardowej do końcowego CDN:

1. [Utwórz rekord CNAME z rejestratorem Twojej domeny i utworzyć jej mapę niestandardowej domeny i poddomeny do punktu końcowego sieci CDN](#register-a-custom-domain-for-an-azure-cdn-endpoint)

    Rekord CNAME to funkcja DNS map domeny źródłowej, takich jak `www.contosocdn.com` lub `cdn.contoso.com`, w domenie docelowej. W tym przypadku domena źródłowa jest niestandardowej domeny i poddomeny (poddomeny, takie jak **"www"** lub **cdn** zawsze jest wymagany). W domenie docelowej jest punkt końcowy CDN.  

    Proces mapowania domeny niestandardowej na punkt końcowy CDN można jednak spowoduje krótki okres przestoje dla domeny podczas rejestrujesz się domeny w Azure Portal.

2. [Dodawanie kroku pośredniego rejestracji z **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)

    Jeśli własnej domeny jest obecnie obsługiwane aplikacji z umowy poziomu usług (SLA) wymaga, aby nie być brak przestojów, można użyć poddomeny Azure **cdnverify** zapewnienie krok rejestracji pośrednich, tak aby użytkownicy będą mieli dostęp do swojej domeny podczas mapowania ma miejsce DNS.  

Po zarejestrowaniu swojej domeny niestandardowej przy użyciu jednej z powyższych procedur należy [sprawdzić, którego niestandardowe poddomeny odwołuje się punkt końcowy CDN](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [AZURE.NOTE] Przy użyciu rejestratora domen w celu zamapowania domeny punkt końcowy CDN, musisz utworzyć rekord CNAME. Rekordy CNAME mapowanie określonych poddomen, takich jak `www.contoso.com` lub `cdn.contoso.com`. Nie jest możliwe w celu zamapowania rekord CNAME domeny głównej, takich jak `contoso.com`.
>    
> Poddomeny może być skojarzony tylko z jednej sieci CDN końcowym. Rekord CNAME, które są tworzone może kierować wszystkie skierowane do poddomeny do określony punkt końcowy.  Na przykład jeśli powiążesz `www.contoso.com` z punkt końcowy CDN następnie nie można skojarzyć go z innymi Azure punkty końcowe, takich jak punkt końcowy konta miejsca do magazynowania lub punktu końcowego usługi w chmurze. Jednak może zawierać różne poddomen z tej samej domeny dla punktów końcowych w innej usłudze. Można również mapować różnych poddomeny do samego punktu końcowego CDN.
>
> Dla punktów końcowych **CDN Azure z Verizon** (Standard i Premium) należy zauważyć, że zajmuje się do **90 minut** w przypadku zmiany domeny niestandardowej propagowanie CDN krawędzi węzły.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Zarejestruj się w niestandardowej domeny dla punktu końcowego Azure CDN

1.  Logowanie do [portalu Azure](https://portal.azure.com/).
2.  Kliknij przycisk **Przeglądaj**, następnie **Profile CDN**, następnie profil CDN z punktu końcowego, który chcesz zamapować na domenę niestandardową.  
3.  Karta **CDN profilu** kliknij punkt końcowy CDN, z którym chcesz skojarzyć poddomeny.
4.  W górnej części karta punktu końcowego kliknij przycisk **Dodaj domenę niestandardowe** .  W karta **dodać domenę niestandardową** pojawi się nazwa hosta punktu końcowego, pochodzące z punkt końcowy CDN używać podczas tworzenia nowego rekordu CNAME. Format adresu nazwa hosta będzie wyświetlany jako ** &lt;EndpointName >. azureedge.net**.  Można kopiować do użycia w utworzeniu rekordu CNAME nazwa hosta.  
5.  Przejdź do witryny sieci web rejestratora domen, a następnie odszukaj sekcję dotyczącymi tworzenia rekordów DNS. Może to znaleźć w sekcji, takich jak **Nazwy domeny**, **DNS**lub **Zarządzanie serwerem nazw**.
6.  Znajdź sekcję do zarządzania rekordów CNAME. Może być konieczne przejdź do strony ustawień zaawansowanych i poszukaj wyrazów, CNAME, aliasu lub poddomeny.
7.  Utwórz nowy rekord CNAME, który mapy z wybranym poddomeny (na przykład **"www"** lub **cdn**) do nazwy hosta opisane w karta **dodać domenę niestandardową** .
8.  Wróć do karta **dodać domenę niestandardową** i wprowadź Twoją domenę niestandardową, w tym poddomeny, w oknie dialogowym. Na przykład wprowadź nazwę domeny w formacie `www.contoso.com` lub `cdn.contoso.com`.   

    Azure będzie Sprawdź, czy rekord CNAME istnieje dla nazwy domeny, które zostały wprowadzone. Jeśli CNAME jest poprawny, sprawdzania poprawności domeny niestandardowej.  Dla punktów końcowych **CDN Azure z Verizon** (Standard i Premium) może upłynąć do 90 minut ustawienia domeny niestandardowej propagowanie do wszystkich węzłów krawędzi CDN, jednak.  

    Należy zauważyć, że w niektórych przypadkach może potrwać czasu dla rekordu CNAME propagowanie serwery nazw w Internecie. Jeśli domena nie jest sprawdzana poprawność natychmiast i uważasz, że rekord CNAME jest poprawny, poczekaj kilka minut i spróbuj ponownie.


## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Zarejestruj się w niestandardowej domeny dla punktu końcowego Azure CDN za pomocą poddomeny cdnverify pośrednie  

1. Logowanie do [portalu Azure](https://portal.azure.com/).
2. Kliknij przycisk **Przeglądaj**, następnie **Profile CDN**, następnie profil CDN z punktu końcowego, który chcesz zamapować na domenę niestandardową.  
3. Karta **CDN profilu** kliknij punkt końcowy CDN, z którym chcesz skojarzyć poddomeny.
4. W górnej części karta punktu końcowego kliknij przycisk **Dodaj domenę niestandardowe** .  W karta **dodać domenę niestandardową** pojawi się nazwa hosta punktu końcowego, pochodzące z punkt końcowy CDN używać podczas tworzenia nowego rekordu CNAME. Format adresu nazwa hosta będzie wyświetlany jako ** &lt;EndpointName >. azureedge.net**.  Można kopiować do użycia w utworzeniu rekordu CNAME nazwa hosta.
5. Przejdź do witryny sieci web rejestratora domen, a następnie odszukaj sekcję dotyczącymi tworzenia rekordów DNS. Może to znaleźć w sekcji, takich jak **Nazwy domeny**, **DNS**lub **Zarządzanie serwerem nazw**.
6. Znajdź sekcję do zarządzania rekordów CNAME. Może być konieczne przejdź do strony ustawień zaawansowanych i poszukaj wyrazów **CNAME** **Alias**lub **poddomeny**.
7. Utwórz nowy rekord CNAME i podaj alias poddomeny, który zawiera poddomeny **cdnverify** . Na przykład poddomena użytkownika będzie w formacie **cdnverify.www** lub **cdnverify.cdn**. Następnie wprowadź nazwę hosta, która jest punkt końcowy CDN w formacie **cdnverify.&lt; EndpointName >. azureedge.net**.
8. Wróć do karta **dodać domenę niestandardową** i wprowadź Twoją domenę niestandardową, w tym poddomeny, w oknie dialogowym. Na przykład wprowadź nazwę domeny w formacie `www.contoso.com` lub `cdn.contoso.com`. Należy zauważyć, że w tym kroku nie trzeba poprzedzić poddomeny z **cdnverify**.  

    Azure będzie Sprawdź, czy rekord CNAME istnieje dla nazwy domeny cdnverify, które zostały wprowadzone.
9. W tym momencie domeny niestandardowej została zweryfikowana przez Azure, ale nie ruch do Twojej domeny są jeszcze kierowane do sieci CDN punktu końcowego. Po oczekiwania długo do używania domeny niestandardowej propagowanie węzły krawędzi CDN (90 minut dla **CDN Azure z Verizon**, 1-2 minut **CDN Azure z Akamai**), powróć do witryny sieci web rejestratora DNS i Utwórz następny rekord CNAME który map usługi poddomeny sieci CDN punktu końcowego. Na przykład określić poddomeny jako **ciągu "www"** lub **cdn**i hostname jako ** &lt;EndpointName >. azureedge.net**. Ten krok zakończeniu rejestracji własnej domeny.
10. Ponadto można usunąć rekord CNAME zostanie utworzony przy użyciu **cdnverify**w sposób konieczne tylko jako etapie pośredniczącym.  


## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Sprawdź niestandardowe poddomeny odwołuje się punkt końcowy sieci CDN

- Po zakończeniu rejestracji domeny niestandardowej, masz dostęp do zawartości, która jest przechowywanych w pamięci podręcznej na punkt końcowy CDN przy użyciu domeny niestandardowej.
Najpierw upewnij się, że masz publicznej zawartości, która jest buforowana w punkt końcowy. Na przykład jeśli punkt końcowy CDN nie jest skojarzone z kontem miejsca do magazynowania, CDN buforowanie zawartości w kontenerach publicznej obiektów blob. Aby przetestować domeny niestandardowej, upewnij się, że do kontenera jest ustawiona na dostęp do publicznej i zawiera co najmniej jeden obiektów blob.
- W przeglądarce przejdź do adresu blob przy użyciu domeny niestandardowej. Na przykład w przypadku domeny niestandardowej `cdn.contoso.com`, adres URL umożliwiający pamięci podręcznej obiektów blob będzie wyglądać podobnie do następujący adres URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Zobacz też

[Jak włączyć sieci dostarczania zawartości (CDN) dla Azure](./cdn-create-new-endpoint.md)  
