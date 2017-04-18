<properties
    pageTitle="Odwołanie do nawigowania między Azure portal"
    description="Dowiedz się środowiska innego użytkownika dla aplikacji sieci Web usługi między portalu zarządzania i Azure Portal"
    services="app-service"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/26/2016"
    ms.author="jaime-espinosa"/>

# <a name="reference-for-navigating-the-azure-portal"></a>Odwołanie do nawigowania między Azure portal

Azure witryny teraz są nazywane [Aplikacji sieci Web usługi](http://go.microsoft.com/fwlink/?LinkId=529714). Aktualizujemy wszystkie naszą dokumentację w celu odzwierciedlenia tej zmiany nazwy i dostarczanie instrukcji Azure Portal. Do momentu ukończeniu tego procesu, dotyczące pracy z aplikacji Web Apps w portalu Azure za pomocą ten dokument jako szablon.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 
 
## <a name="the-future-of-the-azure-classic-portal"></a>Przyszłość Portal klasyczny Azure

Chociaż można zauważyć zmiany znakowania w portalu klasyczny Azure, przeszukiwanej są zastępowane przez Azure Portal. Jak portalu klasyczny jest jest kończony, fokusu w nowych wdrożeniach jest przesuwanie Portal Azure. Wszystkie informacje o nadchodzących nowe funkcje dla aplikacji sieci Web będzie dostępne w Azure Portal. Zacznij korzystać Azure Portal korzystanie z zalet najnowszej i największych, że aplikacje sieci Web do oferowania.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>Układ różnice między klasyczny Portal Azure i Azure Portal

W portalu klasyczny usług Azure znajdują się po lewej stronie. Nawigacja w portalu klasyczny wykonuje drzewa, gdzie rozpocząć z usługi i przejdź do każdego elementu. Ta struktura dobrym rozwiązaniem w przypadku, gdy zarządzania składnikami niezależnie. Jednak aplikacji utworzonych na Azure to zbiór usług połączone i tym drzewa nie jest idealne rozwiązanie w przypadku pracy z kolekcji usług. 

Azure portal ułatwia tworzenie aplikacji końca do końca ze składnikami z wielu usług. Portal usługi są zorganizowane jako *podróży*. *Podróży* jest szereg *karty*, które są kontenerami dla poszczególnych składników. Na przykład konfigurowania automatyczne skalowanie dla aplikacji sieci web jest *podróży* , która ma kilku karty jak pokazano w poniższym przykładzie: karta **witryny sieci web** , (że karta tytuł jeszcze zaktualizowana używać nowego terminologia), karta **Ustawienia** i karta **Skala się** . W tym przykładzie automatyczne skalowanie jest konfigurowany zależeć użycie Procesora, więc również jest karta **CPU procentowa** . Składniki w obrębie *karty* są nazywane *części*, w której wyglądała tak jak kafelki. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Przykład nawigacji: tworzenie aplikacji sieci web

Tworzenie nowej aplikacji sieci web jest nadal tak proste jak 1-2-3. Poniższa ilustracja przedstawia klasyczny portal i portalu przez siebie wykazać nie duża uległa zmianie w liczbie kroków, aby szybko aplikacji sieci web i uruchamiania. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

W portalu można z najbardziej typowych aplikacji sieci web, w tym galerii popularnych aplikacji, takich jak WordPress. Aby uzyskać pełną listę dostępnych aplikacji odwiedź witrynę [Usługi Azure Marketplace].

Gdy tworzysz aplikację sieci web, określ adres URL, plan usług aplikacji, a lokalizacji w portalu tylko, tak jak w przypadku portalu klasyczny. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Ponadto portalu pozwala na zdefiniowanie inne typowe ustawienia. Na przykład [grup zasobów](../azure-resource-manager/resource-group-overview.md) ułatwiają Zobacz i zarządzanie nimi zasoby pokrewne Azure. 

## <a name="navigation-example-settings-and-features"></a>Przykład nawigacji: ustawień i funkcji

Ustawieniami i funkcjami teraz logicznie zgrupowano w jednym karta, z której można przejść.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Na przykład można utworzyć domen niestandardowych, klikając pozycję **SSL i domen niestandardowych** w karta **Ustawienia** .

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

Aby skonfigurować alert monitorowania, kliknij pozycję **żądania i błędów** , a następnie **Dodać Alert**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Aby włączyć Diagnostyka, kliknij **Dzienniki diagnostyczne** karta **Ustawienia** .

![](./media/app-service-web-app-azure-portal/Diagnostics.png)
 
Aby skonfigurować ustawienia aplikacji, kliknij pozycję **Ustawienia aplikacji** w karta **Ustawienia** . 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Inne niż nazwę marki kilka rzeczy w portalu zostały zmienione lub inaczej pogrupowane w celu ułatwienia ich znajdowania. Na przykład, poniżej przedstawiono zrzut ekranu przedstawiający stronę odpowiednie dla aplikacji ustawienia (**Konfiguruj**) w portalu klasyczny.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Więcej zasobów

[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="whats-changed"></a>Informacje o zmianach
* Przewodnika do zmiany z witryn sieci Web do usługi aplikacji Zobacz: [Usługa Azure aplikacji i jego wpływ na istniejące usługi Azure](http://go.microsoft.com/fwlink/?LinkId=529714)
 
