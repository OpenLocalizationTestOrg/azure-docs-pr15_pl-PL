<properties
    pageTitle="Wykonywanie kopii zapasowych i przywracanie aplikacji usługi aplikacji przy użyciu pozostałych"
    description="Dowiedz się, jak za pomocą interfejsu API RESTful wykonywanie kopii zapasowych i przywracanie aplikacji w usłudze Azure aplikacji"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>Wykonywanie kopii zapasowych i przywracanie aplikacji usługi aplikacji przy użyciu pozostałych

> [AZURE.SELECTOR]
- [Programu PowerShell](../app-service/app-service-powershell-backup.md)
- [INTERFEJSU API USŁUGI REST](websites-csm-backup.md)

[Aplikacje usługi aplikacji](https://azure.microsoft.com/services/app-service/web/) można kopii zapasowej jako obiektów blob w magazynie Azure. Wykonywanie kopii zapasowej może również zawierać bazach danych dla tej aplikacji. Jeśli kiedykolwiek przypadkowego usunięcia aplikacji lub aplikację do należy można przywrócić poprzednią wersję, może ona zostać przywrócona z dowolnej poprzedniej kopii zapasowej. Wykonywanie kopii zapasowych można w dowolnym momencie na żądanie lub kopie zapasowe mogą być planowane w odpowiednich odstępach.

W tym artykule wyjaśniono, jak Kopia zapasowa i przywracanie aplikacji żądaniami RESTful interfejsu API. Jeśli chcesz tworzyć i zarządzać nimi kopie zapasowe aplikacji graficzne portalu Azure, zobacz [Tworzenie kopii zapasowej aplikacji sieci web w usłudze Azure aplikacji](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>Wprowadzenie
Aby wysłać żądania usługi REST, należy wiedzieć **Nazwa**Twojej aplikacji, **Grupa zasobów**i **identyfikator subskrypcji**. Te informacje można znaleźć, klikając pozycję aplikacji w karta **Aplikacji usługi** [Azure portal](https://portal.azure.com). Aby zapoznać się z przykładami w tym artykule firma Microsoft konfigurowania witryny sieci Web **backuprestoreapiexamples.azurewebsites.net**. Znajduje się w grupie zasobów domyślnego-Web-WestUS, a na subskrypcję przy użyciu Identyfikatora 00001111-2222-3333-4444-555566667777 działa.

![Informacje o witrynie internetowej próbki][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>Kopia zapasowa i przywracanie interfejsu API usługi REST
Teraz możemy przedstawimy kilka przykładów jak za pomocą interfejsu API usługi REST kopia zapasowa i przywracanie aplikacji. Każdy przykład zawiera treść adresu URL i HTTP. Przykładowy adres URL zawiera symbole zastępcze zawinięty w nawiasy klamrowe, takich jak {identyfikator subskrypcji}. Zastępowanie symboli zastępczych odpowiednich informacji dla aplikacji. Odwołanie w tym miejscu znajdują się wyjaśnienia dotyczące każdego symbolu zastępczego, który pojawia się w adresy URL.

* Identyfikator subskrypcji — identyfikator Azure zawierającej aplikację
* zasób nazwę grupy — nazwę grupy zasobów zawierającej aplikację
* Nazwa — aplikacji
* Kopia zapasowa id — Identyfikator aplikacji kopii zapasowej

Pełna dokumentacja interfejsu API, łącznie z kilku parametrów opcjonalne, które mogą być zawarte w żądanie HTTP zobacz [Azure Eksploratora zasobów](https://resources.azure.com/).

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>Aby utworzyć kopię zapasową aplikacji na żądanie
Aby utworzyć kopię zapasową aplikacji od razu, Wyślij żądanie **WPIS** **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.

Poniżej przedstawiono, jak przy użyciu naszym przykładzie witryny sieci Web wygląda adres URL. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Obiekt JSON w treści żądania Określ, którego konta magazynu służy do przechowywania kopii zapasowej podany. Obiekt JSON musi mieć właściwość o nazwie **storageAccountUrl**, który zawiera [Adres URL skojarzeń zabezpieczeń](../storage/storage-dotnet-shared-access-signature-part-1.md) udzielanie zapisu kontener magazyn Azure, zawierający kopii zapasowej obiektów blob. Jeśli chcesz utworzyć kopię zapasową bazy danych, należy również podać listę zawierającą nazwy, typów i parametry połączenia bazy danych do wykonania kopii zapasowej.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Kopię zapasową aplikacji zaczyna się natychmiast po otrzymaniu żądania. Proces tworzenia kopii zapasowej może potrwać bardzo długo. Odpowiedź HTTP zawiera identyfikator, którego można używać w kolejnego żądania, aby wyświetlić stan kopii zapasowej. Oto przykład treści odpowiedzi HTTP żądanie kopii zapasowej.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] Komunikaty o błędach znajdują się we właściwości dziennika odpowiedzi HTTP.

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>Planowanie automatycznego wykonywania kopii zapasowych
Oprócz kopię zapasową aplikacji na żądanie, można również zaplanować kopii zapasowej zostanie przeprowadzona automatycznie.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Konfigurowanie nowego harmonogramu automatycznych kopii zapasowych
Aby skonfigurować harmonogram wykonywania kopii zapasowych, Wyślij żądanie **umieszczenie** **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.

Poniżej przedstawiono, jak wygląda adres URL naszym przykładzie witryny sieci Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

Treść żądania musi być obiekt JSON, który umożliwia określenie konfiguracji kopii zapasowej. Oto przykład z wszystkie wymagane parametry.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

W tym przykładzie konfiguruje aplikację, aby automatycznie zapasową co siedem dni. Parametry **frequencyInterval** i **frequencyUnit** razem Określ, jak często wystąpić kopie zapasowe. Prawidłowe wartości **frequencyUnit** są **godziny** i **dni**. Na przykład aby utworzyć kopię zapasową aplikacji co 12 godzin, ustaw frequencyInterval 12 i frequencyUnit na godzinę.

Starych kopii zapasowych są automatycznie usuwane z konta miejsca do magazynowania. Można kontrolować, ile kopii zapasowych może być przez ustawienie parametru **retentionPeriodInDays** . Jeśli chcesz, aby zawsze mieć co najmniej jedna kopia zapasowa zapisany, niezależnie od tego, jak stare ustawiana jest **keepAtLeastOneBackup** na wartość PRAWDA.

### <a name="get-the-automatic-backup-schedule"></a>Pobieranie automatyczne harmonogramu wykonywania kopii zapasowych
Aby pobrać aplikację konfiguracji kopii zapasowej, wysłać żądanie **WPIS** do adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.

Adres URL witryny przykład jest **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>Pobierz stan kopii zapasowej
W zależności od tego, jak duży jest aplikacji kopii zapasowej może potrwać do wykonania. Kopie zapasowe może również nie działać, limit czasu lub częściowo powiodła się. Aby wyświetlić stan wszystkich aplikacji wykonywania kopii zapasowych, wysłać żądanie **PRZEJŚĆ** do adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.

Aby wyświetlić stan kopii zapasowej, Wyślij żądanie GET do adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Poniżej przedstawiono, jak wygląda adres URL naszym przykładzie witryny sieci Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**

Treść odpowiedzi zawiera obiekt JSON podobnie jak w tym przykładzie.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

Stan kopii zapasowej jest typem wyliczenia. Oto co możliwe stany.

* 0 — w trakcie wykonywania: kopia zapasowa została uruchomiona, ale nie zostało jeszcze zakończone.
* 1 — nie powiodło się: Wykonywanie kopii zapasowej nie powiodło się.
* 2 — powiodło się: Pomyślnie wykonano kopię zapasową.
* 3 — upłynął limit czasu: Wykonywanie kopii zapasowej nie zostało zakończone w czasie i zostało anulowane.
* 4-tworzone: Żądanie kopii zapasowej znajduje się w kolejce, ale nie został uruchomiony.
* 5 — pominięte: Wykonywanie kopii zapasowej nie kontynuować z powodu serii rozłożonych w czasie powodujące zbyt dużą liczbą kopii zapasowych.
* 6 — PartiallySucceeded: Wykonywanie kopii zapasowej zakończyło się pomyślnie, ale niektóre pliki nie zostały kopii zapasowej, ponieważ nie można odczytać. Zazwyczaj dzieje się tak, ponieważ w trybie wyłączności umieszczono na plikach.
* 7 — DeleteInProgress: Wykonywanie kopii zapasowej zażądano zostanie usunięta, ale jeszcze nie zostały usunięte.
* 8 — DeleteFailed: Nie można usunąć kopii zapasowej. Może się to zdarzyć, ponieważ wygasła adres URL skojarzeń zabezpieczeń, który został użyty do utworzenia kopii zapasowej.
* 9 — usunięte: Kopia zapasowa została usunięta.

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>Przywracanie z kopii zapasowej aplikacji
Jeśli usunięto aplikacji lub jeśli chcesz przywrócić poprzednią wersję aplikacji, można przywrócić aplikacji z kopii zapasowej. Aby wywołać przywracania, wysłać żądanie **WPIS** do adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.

Poniżej przedstawiono, jak wygląda adres URL naszym przykładzie witryny sieci Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/Restore**

W treści wezwania Wyślij obiekt JSON, który zawiera właściwości dla operacji przywracania. Oto przykład zawierające wszystkie wymagane właściwości:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Przywracanie do nowej aplikacji
Czasem warto utworzenia nowej aplikacji, po przywróceniu kopii zapasowej, zamiast zastępowania już istniejącej aplikacji. W tym celu należy zmienić adres URL żądanie, aby wskazywały nowej aplikacji, którą chcesz utworzyć i zmień właściwość **zastąpić** w formacie JSON **FAŁSZ**.

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>Usuwanie aplikacji kopii zapasowej
Jeśli chcesz usunąć kopię zapasową, wysłać żądanie **Usuwanie** do adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Poniżej przedstawiono, jak wygląda adres URL naszym przykładzie witryny sieci Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>Zarządzanie adresu URL skojarzeń zabezpieczeń kopii zapasowej
Azure aplikacji usługi spróbuje usunąć kopii zapasowej z magazynu Azure za pomocą adresu URL skojarzeń zabezpieczeń otrzymany podczas tworzenia kopii zapasowej. Jeśli ten adres URL skojarzeń zabezpieczeń już nie jest prawidłowa, wykonywanie kopii zapasowej nie można usunąć za pomocą interfejsu API usługi REST. Jednak możesz zaktualizować adres URL skojarzeń zabezpieczeń skojarzonych z kopii zapasowej przez wysłanie żądania **POST** do adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Poniżej przedstawiono, jak wygląda adres URL naszym przykładzie witryny sieci Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backups/1/list**

W treści wezwania Wyślij obiekt JSON, zawierający nowy adres URL skojarzeń zabezpieczeń. Oto przykład.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] Ze względów bezpieczeństwa adres URL skojarzeń zabezpieczeń skojarzony z kopii zapasowej nie jest zwracana podczas wysyłania żądania GET dla określonej kopii zapasowej. Jeśli chcesz wyświetlić adres URL skojarzeń zabezpieczeń skojarzony z kopii zapasowej, Wyślij żądanie POST do tego samego adresu URL powyżej. Dołącz pustego obiektu JSON w treści wezwania. Odpowiedzi z serwera zawiera wszystkie informacje że kopia zapasowa, w tym adres URL skojarzeń zabezpieczeń.

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
