<properties
    pageTitle="Jak przeprowadzić migrację z RemoteApp VNET do VNET Azure | Microsoft Azure"
    description="Dowiedz się, jak przeprowadzić migrację z RemoteApp VNET do VNET Azure"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a>Jak przeprowadzić migrację zbioru hybrydowego z RemoteApp VNET do VNET Azure

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Dobra wiadomość! Firma Microsoft umożliwiły na wdrożenie hybrydowe RemoteApp zbiory bezpośrednio do istniejącego Azure wirtualnej sieci (VNETs) zamiast VNETs specyficzne dla RemoteApp. Pozwala korzystać z najnowszych funkcji VNET (na przykład ExpressRoute) i nadaj kolekcji hybrydowych sieci bezpośredni dostęp do innych usług Azure i maszyn wirtualnych wdrożony tego VNET.  (Dzięki temu możesz lepszą wydajność i konfigurowanie łatwiejsze niż konfiguracji VNET do VNET).


Załóżmy, że masz już utworzony hybrydowych zbioru RemoteApp o nazwie *OriginalCollection* z RemoteApp VNET o nazwie *RemoteAppVNET*. Poniżej przedstawiono czynności, aby przeprowadzić migrację do nowego VNET Azure o nazwie *AzureVNET*.

1.  Na karcie **sieci** w [portalu zarządzania](http://manage.windowsazure.com/)tworzenie VNET o nazwie *AzureVNET*, za pomocą tej samej lokalizacji, konfiguracji systemu DNS i przestrzeni adresów (dla co najmniej jednej podsieci *AzureVNET* ) podczas dla *RemoteAppVNET*.
2.  Konfigurowanie *AzureVNET* albo hosta lub ma łączności sieciowej wdrożenia usługi Active Directory, która *OriginalCollection* jest dołączony do domeny.
3.  Na karcie **RemoteApps** utworzyć nowy zbiór RemoteApp o nazwie *Nowej kolekcji*. (Opcja **Utwórz z VNET** , nie **Szybkie tworzenie**.)
3.  Konfigurowanie *NewCollection* do wdrożenia na podsieć w *AzureVNET*.
4.  Konfigurowanie *NewCollection* korzystać z tego samego obrazu i informacje o dołączaniu do domeny podczas dla *OriginalCollection*.
5.  Po kilka godzin *NewCollection* pojawi się na liście zbioru ze stanem aktywna.

Teraz Jeśli nie potrzebujesz przeprowadzić migrację wszelkie informacje o użytkowniku z oryginalnym zbiorze do nowej kolekcji, wykonaj te kroki, dalej:

6.  Usuwanie *OriginalCollection*.
7.  Usuwanie *RemoteAppVNET*.

I gotowe!

Ewentualnie Jeśli musisz przeprowadzić migrację informacje o użytkowniku z oryginalnym zbiorze do nowej kolekcji, wykonaj te kroki, dalej:

6.  Wysyłanie wiadomości e-mail do [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) z identyfikator subskrypcji Azure, nazwę oryginalnego zbioru i nazwę nowej kolekcji i poproś o migrację informacje o użytkowniku.
7.  W ciągu 2 dni roboczych zespołu RemoteApp spowoduje przeniesienie listy dostępu użytkowników i wszystkie dokumenty użytkownika i ustawień użytkownika z oryginalnym zbiorze do nowej kolekcji.
8.  Usuwanie *OriginalCollection*.
9.  Usuwanie *RemoteAppVNET*.

I, gotowe!

Jeśli masz pytania lub potrzebujesz pomocy specjalne, Wyślij wiadomość e-mail [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).
