<properties 
    pageTitle="Jak zaimplementować awarii przy użyciu usługi kopii zapasowej i przywracać w zarządzaniu interfejsu API Azure | Microsoft Azure" 
    description="Dowiedz się, jak za pomocą kopii zapasowych i przywracanie do wykonywania awarii w zarządzaniu interfejsu API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Jak zaimplementować awarii przy użyciu usługi kopii zapasowej i przywracać w zarządzaniu interfejsu API platformy Azure

Wybierając do publikowania i zarządzanie do interfejsów API za pośrednictwem interfejsu API usługi zarządzania Azure są korzystanie z wielu odporność na uszkodzenia i możliwości infrastruktury, które w przeciwnym razie należy projektowanie i implementowanie oraz zarządzanie. Platformy Azure zmniejsza zagrożenie duża część potencjalne błędy w ułamek kosztów.

Aby odzyskać dostępność problemów wpływających na region, gdzie Zarządzanie interfejsu API usługi hostowanej możesz powinny być gotowy do odtworzenia usługi w innym regionie w dowolnym momencie. W zależności od swojej dostępności i cel czasu odzyskiwania można zarezerwować kopii zapasowej usługi w jednej lub większej liczbie regionów i próby zachowania ich zawartości i konfiguracji synchronizacji z usługą active. Wykonywanie kopii zapasowej usług i funkcji przywracania zawiera niezbędne blok konstrukcyjny stosowania usługi strategii odzyskiwania danych.

Ten przewodnik pokazano, jak do uwierzytelniania żądań Azure Menedżera zasobów i jak Kopia zapasowa i przywracanie programu wystąpień usługi zarządzania interfejsu API.

>[AZURE.NOTE] Proces tworzenia kopii zapasowych i przywracanie wystąpienia usług zarządzania interfejsu API awarii można również replikacji wystąpień usługi zarządzania interfejsu API scenariuszach, takich jak tymczasowego.
>
>Należy zauważyć, że każda kopia zapasowa Dezaktualizuje się po 7 dni. Przy próbie kopii zapasowej po wygaśnięciu okresu wygaśnięcia 7 dni, przywracanie zakończy się niepowodzeniem z `Cannot restore: backup expired` wiadomości.

## <a name="authenticating-azure-resource-manager-requests"></a>Żądania uwierzytelniania Menedżera zasobów Azure

>[AZURE.IMPORTANT] Interfejsu API usługi REST kopii zapasowych i przywracanie używa Menedżera zasobów Azure i ma mechanizm uwierzytelniania inny niż pozostałe interfejsy API zarządzania obiekty interfejsu API zarządzania. W tej sekcji opisano sposób uwierzytelniania żądań Azure Menedżera zasobów. Aby uzyskać więcej informacji zobacz [żądania uwierzytelniania Azure Menedżera zasobów](http://msdn.microsoft.com/library/azure/dn790557.aspx).

Wszystkie zadania, które można wykonywać zasobów za pomocą Menedżera zasobów Azure muszą zostać uwierzytelnione z usługi Azure Active Directory, wykonaj następujące czynności.

-   Dodawanie aplikacji do dzierżawy usługi Azure Active Directory.
-   Ustawianie uprawnień dla aplikacji, w której zostały dodane.
-   Uzyskaj tokenu uwierzytelniania żądania do Menedżera zasobów Azure.

Pierwszym krokiem jest utworzenie aplikacji usługi Azure Active Directory. Zaloguj się do [Klasyczny Portal Azure](http://manage.windowsazure.com/) za pomocą subskrypcji, która zawiera wystąpienia usługi zarządzania interfejsu API i przejdź do karty **aplikacji** dla usługi Azure Active Directory domyślną.

>[AZURE.NOTE] Jeśli domyślne Azure Active directory nie jest widoczny dla Twojego konta, skontaktuj się z administratorem Azure subskrypcji, aby udzielić uprawnień wymaganych do Twojego konta. Aby uzyskać informacje o lokalizacji domyślny katalog zobacz [Lokalizowanie domyślny katalog](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-portal).

![Tworzenie aplikacji usługi Azure Active Directory][api-management-add-aad-application]

Kliknij przycisk **Dodaj**, **Dodaj aplikację, którą rozwija się w mojej organizacji**i wybierz **aplikację Native client**. Wprowadź opisową nazwę, a następnie kliknij strzałkę następnego. Wprowadź symbolu zastępczego adresu URL, na przykład `http://resources` dla **Przekierowania URI**jego pole jest wymagane, ale nie jest używana wartość później. Kliknij pole wyboru, aby zapisać aplikację.

Po zapisaniu aplikacji, kliknij przycisk **Konfiguruj**, przewiń w dół do sekcji **uprawnienia do innych aplikacji** , a następnie kliknij pozycję **Dodaj aplikację**.

![Dodaj uprawnienia][api-management-aad-permissions-add]

Zaznacz **systemu Windows** **Azure usługi zarządzania API** i kliknij pole wyboru, aby dodać aplikację.

![Dodaj uprawnienia][api-management-aad-permissions]

Kliknij przycisk **Uprawnienia delegowane** obok nowo dodany aplikacji **systemu Windows** **Azure usługi zarządzania API** , zaznacz pole wyboru obok **Zarządzanie usługą Azure programu Access (wersja preview)**i kliknij przycisk **Zapisz**.

![Dodaj uprawnienia][api-management-aad-delegated-permissions]

Przed wywoływanie interfejsów API Generowanie kopii zapasowej i przywrócenie jej, należy uzyskać token. W poniższym przykładzie użyto pakietu nuget [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) do pobrania tokenu.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }

                Console.WriteLine(result.AccessToken);

                Console.ReadLine();
            }
        }
    }

Zamienianie `{tentand id}`, `{application id}`, i `{redirect uri}` za pomocą poniższych instrukcji.

Zamienianie `{tenant id}` o identyfikatorze dzierżawy aplikacji usługi Azure Active Directory została właśnie utworzona. Masz dostęp do identyfikatora, klikając pozycję **Widok punktów końcowych**.

![Punkty końcowe][api-management-aad-default-directory]

![Punkty końcowe][api-management-endpoint]

Zamienianie `{application id}` i `{redirect uri}` za pomocą pola **Identyfikator klienta** i adres URL z sekcji **Przekierować URI** z karty **Konfigurowanie** aplikacji usługi Azure Active Directory. 

![Zasoby][api-management-aad-resources]

Gdy określono wartości, przykład kodu powinna zwrócić token podobny do następującego przykładu.

![Tokenu][api-management-arm-token]

Zanim wywoływanie kopia zapasowa i przywracanie czynności opisane w poniższych sekcjach, ustaw nagłówku żądania autoryzacji dla pozostałych połączenia.

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"> </a>Kopia zapasowa usługi zarządzania interfejsu API
Aby utworzyć kopię zapasową zarządzania interfejsu API usługi problemu następujące żądanie HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

w przypadku gdy:

* `subscriptionId`-Identyfikator subskrypcji zawierającej usługę Zarządzanie interfejsu API próbujesz do wykonania kopii zapasowej
* `resourceGroupName`-ciąg w postaci "Interfejsu Api - Default — {usługa region}" miejsce, w którym `service-region` identyfikuje Azure region, w którym zarządzania interfejsu API usługi próbujesz kopii zapasowej jest obsługiwana, np.`North-Central-US`
* `serviceName`-Nazwa usługi zarządzania interfejsu API wykonywania kopii zapasowej określony w momencie jego utworzenia
* `api-version`-Zamień`2014-02-14`

W treści wezwania na Określ nazwę konta magazynu platformy Azure docelowej, klawisz dostępu, nazwa kontenera obiektów blob i nazwa kopii zapasowej:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Ustaw wartość `Content-Type` posługując się `application/json`.

Kopia zapasowa jest długotrwałych operacji, które może potrwać kilka minut.  Jeśli żądanie zakończyło się pomyślnie i wykonywania kopii zapasowej została zainicjowana otrzymasz `202 Accepted` kod stanu odpowiedzi z `Location` nagłówek.  Upewnij się, "GET" żądania do adresu URL w `Location` nagłówek, aby sprawdzić stan operacji. W trakcie wykonywania kopii zapasowej nadal będziesz otrzymywać kodu stanu "zaakceptowane 202". Kod odpowiedzi `200 OK` wskaże pomyślnego zakończenia wykonywanie kopii zapasowej.

**Uwaga**:

- **Kontener** określony w treści wezwania **musi istnieć**.
* Podczas Trwa wykonywanie kopii zapasowej możesz **nie powinny być prób wszystkie operacje zarządzania usługi** takie jak SKU uaktualnienia lub starszą wersję, zmiany nazwy domeny, itd. 
* Przywracanie z **kopii zapasowej jest gwarantowana tylko przez 7 dni** od momentu jego utworzenia. 
* Służy do tworzenia analizy **danych dotyczących użycia** raportów **jest Nieuwzględnione** w kopii zapasowej. Okresowo pobieranie raportów analizy dla przechowywanie za pomocą [Interfejsu API usługi REST zarządzania interfejsu API Azure][] .
* Częstotliwość, z którym możesz wykonywać kopie zapasowe usługi wpłynie na usługi odzyskiwania punktów cel. Aby zminimalizować warto implementacji regularnego wykonywania kopii zapasowych, a także wykonywanie kopii zapasowych na żądanie po wprowadzeniu istotnych zmianach w usłudze zarządzania interfejsu API.
* **Zmiany** wprowadzone w konfiguracji usługi (interfejsy API, zasady, wygląd portalu Deweloper) podczas operacji wykonywania kopii zapasowej jest w toku **mogą nie być uwzględnione w kopii zapasowej i w związku z tym zostaną utracone**.

## <a name="step2"> </a>Przywracanie usługi zarządzania interfejsu API
Aby przywrócić zarządzania interfejsu API usługi z wcześniej utworzonej kopii zapasowej upewnij następujące żądanie HTTP:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

w przypadku gdy:

* `subscriptionId`-Identyfikator subskrypcji zawierającej usługę Zarządzanie interfejsu API przywracane z kopii zapasowej do
* `resourceGroupName`-ciąg w postaci "Interfejsu Api - Default — {usługa region}" miejsce, w którym `service-region` identyfikuje Azure region, gdzie znajduje się z usługą zarządzania interfejsu API przywracane z kopii zapasowej do, np.`North-Central-US`
* `serviceName`-nazwę zarządzania interfejsu API usługi Trwa przywracanie do określonego w momencie jego utworzenia
* `api-version`-Zamień`2014-02-14`

W treści wezwania Określ lokalizację pliku kopii zapasowej, to znaczy nazwę konta magazynu platformy Azure, klawisz dostępu, nazwa kontenera obiektów blob i nazwa kopii zapasowej:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

Ustaw wartość `Content-Type` posługując się `application/json`.

Przywracanie jest długotrwałych operacji, które może potrwać do 30 lub kilku minut do wykonania.  Jeśli żądanie zakończyło się pomyślnie, a proces przywracania została zainicjowana otrzymasz `202 Accepted` kod stanu odpowiedzi z `Location` nagłówka.  Upewnij się, "GET" żądania do adresu URL w `Location` nagłówek, aby sprawdzić stan operacji. Gdy trwa przywracanie nadal będziesz otrzymywać "zaakceptowane 202" kodu stanu. Kod odpowiedzi `200 OK` wskaże pomyślnego zakończenia operacji przywracania.

>[AZURE.IMPORTANT] **Jednostka SKU** są usługi przywrócone do **musi odpowiadać** SKU kopii zapasowej usługi są przywracane.
>
>**Zmiany** wprowadzone w konfiguracji usługi (interfejsy API, zasady, wygląd portalu Deweloper) podczas operacji przywracania znajduje się w toku, **może zostać zastąpione**.

## <a name="next-steps"></a>Następne kroki
Zapoznaj się z następujących blogów firmy Microsoft dla dwóch różnych instruktaży procesu tworzenia kopii zapasowych i przywracania.

-   [Powielić Azure interfejsu API zarządzania kontami](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   Dziękujemy do Gisela dla swojego udziału w tym artykule.
-   [Zarządzanie Azure interfejsu API: Wykonywanie kopii zapasowych i przywracanie konfiguracji](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   Metody szczegółowe przez Stuartowi nie ma odpowiednika oficjalnym porad, ale jest bardzo interesujące.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Zarządzanie Azure interfejsu API interfejsu API usługi REST]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 
