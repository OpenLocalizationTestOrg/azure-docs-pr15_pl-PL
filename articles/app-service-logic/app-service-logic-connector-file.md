<properties
    pageTitle="Za pomocą łączników pliku w aplikacjach logiczny | Microsoft Azure aplikacji usługi"
    description="Jak utworzyć i skonfigurować łącznik pliku lub aplikacji interfejsu API i używać go w aplikacji dla logiki Azure aplikacji usługi"
    authors="rajeshramabathiran"
    manager="erikre"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="rajram"/>

# <a name="get-started-with-the-file-connector-and-add-it-to-your-logic-app"></a>Wprowadzenie do łącznika plik i dodać go do aplikacji warunków logicznych
>[AZURE.NOTE] Tej wersji artykuł dotyczy wersji schematu 2014-12-01-podgląd aplikacji logika.

Podłącz do systemu plików do przekazania, Pobierz i nie tylko do plików na komputerze obsługującym. Aplikacje logika może spowodować na podstawie różnych źródeł danych i łączników oferty pobieranie i przetwarzanie danych. Możesz dodać łącznik pliku do przepływu pracy i proces danych biznesowych w ramach tego przepływu pracy w aplikacji logicznych. 

Łącznik plik używa hybrydowych Connection Manager łączności hybrydowych w systemie plików hosta.

## <a name="creating-a-file-connector-for-your-logic-app"></a>Tworzenie pliku łącznik aplikacji logiki ##
Aby użyć łącznika w pliku, należy najpierw utworzyć wystąpienie aplikacji interfejsu API łącznik pliku. Można to zrobić w następujący sposób:

1.  Otwórz Azure Marketplace przy użyciu + nowa opcja po lewej stronie Azure Portal.
2.  Wyszukaj "plik łącznika".
3.  Wybierz **Plik łącznik (wersja preview)** z listy wyników wyszukiwania.
4.  Kliknij przycisk **Utwórz**
5.  Konfigurowanie łącznika w pliku w następujący sposób:  
![][1]

    - **Nazwa** — należy podać nazwę dla tego łącznika pliku
    - **Ustawienia dotyczące pakietu**
        - **Folder główny** - Określ ścieżka folderu głównego na komputerze hosta. Np. D:\FileConnectorTest
        - **Parametry połączenia Bus usługi** — zapewniają parametry połączenia Bus usługi. Upewnij się, że obszar nazw bus usługi jest typu standardowe i nie podstawowe umożliwiające korzystanie z usługi Bus przekaźniki.  Usługa Bus przekazywania jest używany do łączenia Menedżera połączeń hybrydowych.
    - **Plan usług aplikacji** — wybierz lub Utwórz plan usług aplikacji
    - **Ceny warstwa** — Wybierz cennik Warstwa łącznika
    - **Grupa zasobów** — wybierz lub Utwórz nową grupę zasobów, gdzie powinien znajdować się łącznik
    - **Subskrypcja** — Wybierz subskrypcję ma ten łącznik ma być utworzony w
    - **Lokalizacja** — wybierz lokalizację geograficzną miejsce, w którym chcesz łącznik do wdrożenia

4. Kliknij przycisk Utwórz. Zostanie utworzony nowy plik łącznika

## <a name="configure-hybrid-connection-manager"></a>Konfigurowanie hybrydowe Connection Manager ##
Po utworzeniu wystąpienia aplikacji interfejsu API, przejdź do jej pulpitu nawigacyjnego.  Można to zrobić, klikając pozycję Przeglądaj > aplikacje interfejsu API > zaznacz wybrany łącznik plik aplikacji interfejsu API.  W tym miejscu hybrydowych Connection Manager trzeba skonfigurować.
Aby uzyskać więcej informacji o konfigurowaniu i hybrydowego Connection Manager Rozwiązywanie problemów Zobacz [Korzystanie z Menedżera połączeń hybrydowych].

## <a name="using-the-file-connector-in-your-logic-app"></a>Przy użyciu łącznika pliku w aplikacji warunków logicznych ##
Po utworzeniu aplikacji interfejsu API teraz umożliwiają łącznik pliku jako akcji aplikacji logicznych. Aby to zrobić, musisz:

1.  Tworzenie nowej aplikacji logiczny, a następnie wybierz pozycję tej samej grupy zasobów, w której znajduje się plik łącznika. Wykonaj instrukcje do [utworzenia nowej aplikacji logicznych].

2.  Otwórz "Wyzwalacze i akcje" poziomu utworzonych aplikacji logiki do otwierania aplikacji logika projektanta i konfigurowanie usługi przepływu.

3.  Łącznik pliku pojawią się w sekcji "Interfejsu API aplikacje w tej grupie zasobów" w galerii po prawej stronie.

4.  Można usunąć aplikację plik łącznika interfejsu API do edytora, klikając pozycję łącznika"plik". Plik łącznika udostępnia jeden wyzwalacz i 4 akcje:  
![][5]

6.  Każdą z nich tych udostępnia niektóre właściwości. Na poniższej ilustracji przedstawiono właściwości wyzwalacza i Pobierz plik akcji:  
![][6]

7. Skonfigurowane tych wyzwalacza i akcji mogą być używane w Twojej przepływu. Podobnie inne akcje mogą być również skonfigurowane.

> [AZURE.NOTE] Wyzwalacza pliku spowoduje usunięcie pliku po pomyślnym jest odczytu z folderu.

## <a name="file-connector-rest-apis"></a>Plik łącznika interfejsy API pozostałych ##
Aby użyć łącznik pracy poza aplikacją logiczny, można użyć za pośrednictwem interfejsów API pozostałych ujawnionego przez łącznik. Użytkownik może widoku tej definicji interfejsu API za pomocą karty Przeglądaj -> interfejsu Api aplikacji -> plik łącznika. Teraz kliknij obiektywu definicji interfejsu API, aby wyświetlić wszystkie interfejsy API ujawnionego przez ten łącznik, w sekcji Podsumowanie:  
![][7]

Szczegóły interfejsów API można znaleźć w [pliku łącznik definicji interfejsu API].

## <a name="do-more-with-your-connector"></a>Więcej możliwości korzystania z tego łącznika
Teraz, gdy łącznik jest tworzony, możesz dodać go do przepływu pracy firm przy użyciu aplikacji logicznych. Zobacz [Co to są aplikacje logika?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z aplikacji logika Azure przed utworzeniem konta dla konta Azure, przejdź do [aplikacji spróbuj logiczny](https://tryappservice.azure.com/?appservice=logic), miejsce, w którym możesz od razu utworzyć krótkotrwałe starter logiczny aplikację w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

Wyświetlanie odwołania Swagger interfejsu API usługi REST [łączników](http://go.microsoft.com/fwlink/p/?LinkId=529766)i interfejsu API aplikacje odwołania.

Można także przeglądać wydajności statystyki i kontroli zabezpieczeń do łącznika. Zobacz [Monitorowanie wbudowanych łączników i aplikacjami interfejsu API i zarządzanie nimi](app-service-logic-monitor-your-connectors.md).

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Tworzenie nowej aplikacji warunków logicznych]: app-service-logic-create-a-logic-app.md
[Plik łącznika definicji interfejsu API]: https://msdn.microsoft.com/library/dn936296.aspx
[Korzystanie z Menedżera połączeń hybrydowego]: app-service-logic-hybrid-connection-manager.md
