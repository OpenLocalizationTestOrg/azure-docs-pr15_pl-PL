<properties
    pageTitle="Tworzenie rozwiązania IoT przy użyciu analizy strumieniu | Microsoft Azure"
    description="Wprowadzenie do samouczka rozwiązania IoT analizy strumieniu scenariusza budki"
    keywords="rozwiązanie iot, funkcje okna"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Tworzenie rozwiązania IoT przy użyciu analizy strumieniu

## <a name="introduction"></a>Wprowadzenie

W tym samouczku dowiesz się, jak za pomocą analizy strumieniu Azure uzyskiwanie wniosków w czasie rzeczywistym na podstawie danych. Deweloperzy mogą łatwo łączyć strumieni danych, takich jak strumienie kliknij pozycję Dzienniki i zdarzenia wygenerowane przez urządzenia, z historycznych rekordów lub dane odwołanie do uzyskania wniosków firm. Jako usługa obliczeń w pełni zarządzane, w czasie rzeczywistym strumienia obsługiwanej w programie Microsoft Azure analizy strumieniu Azure zapewnia elastyczność wbudowanych, krótki czas oczekiwania i skalowalność na uruchomienie czasu w minutach.

Po zakończeniu tego samouczka, będą mogli:

-   Zapoznaj się z portal Azure analizy strumieniu.
-   Konfigurowanie i wdrażanie przesyłanie strumieniowe zadania.
-   Zwrócone rzeczywistych problemów i rozwiązywanie ich za pomocą języka kwerend analizy strumieniu.
-   Można opracowywać streaming rozwiązań dla klientów za pomocą analizy strumieniu sprawne.
-   Rozwiązywanie problemów za pomocą monitorowania i rejestrowania środowiska.

## <a name="prerequisites"></a>Wymagania wstępne

Potrzebne są następujące wymagania wstępne dotyczące użycia tego samouczka:

-   Najnowszą wersję programu [Azure PowerShell](../powershell-install-configure.md)
-   Visual Studio 2015 lub bezpłatne [Społeczności programu Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
-   [Azure subskrypcji](https://azure.microsoft.com/pricing/free-trial/)
-   Uprawnienia administracyjne na tym komputerze
-   Pobierz [TollApp.zip](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) z Centrum pobierania Microsoft
-   Opcjonalnie: Kod źródłowy generator zdarzenia TollApp w [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>Scenariusz wprowadzenie: "Witaj, płatne!"


Stacja płatne jest typowych zjawiska. Wystąpienia je na wiele trasy szybkiego ruchu, mosty i tunele całym świecie. Każda stacja płatne ma wiele kabiny płatne. Ręczne kabiny Jeśli chcesz zatrzymać zapłacić płatnego attendant. Na automatyczne kabiny czujnik u góry każdej stoiska skanuje Karta RFID, w której jest umieszczony na szyby pojazdu, jak przekazać stoiska płatne. Jest proste wizualizacji fragment pojazdy przez stację tych płatne jako strumień zdarzenia, w którym interesujące operacje mogą być wykonywane.

![Obraz przedstawiający samochodów w płatne kabiny](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Dane przychodzące

Ten samouczek współdziała z dwoma strumieniami danych. Czujniki zainstalowane w wejście i wyjście stacji płatne utworzenia pierwszego strumienia. Druga strumień jest statyczne odnośnika zestawu danych, zawierający dane rejestracji pojazdu.

### <a name="entry-data-stream"></a>Wpis strumienia danych

Wpis strumienia danych zawiera informacje o samochody mogą wejść do stacji płatne.


| TollID | EntryTime               | LicensePlate | Województwo | Wprowadź   | Model   | VehicleType | VehicleWeight | Płatne | Znacznik       |
|--------|-------------------------|--------------|-------|--------|---------|-------------|---------------|------|-----------|
| 1      | 12:01:00.000 2014-09-10 | JNB 7001     | NY    | Honda  | CRV     | 1           | 0             | 7    |           |
| 1      | 12:02:00.000 2014-09-10 | YXZ 1001     | NY    | Toyota | Camry   | 1           | 0             | 4    | 123456789 |
| 3      | 12:02:00.000 2014-09-10 | ABC 1004     | CT    | Ford   | Taurus  | 1           | 0             | 5    | 456789123 |
| 2      | 12:03:00.000 2014-09-10 | XYZ 1003     | CT    | Toyota | Corolla | 1           | 0             | 4    |           |
| 1      | 12:03:00.000 2014-09-10 | BNJ 1007     | NY    | Honda  | CRV     | 1           | 0             | 5    | 789123456 |
| 2      | 12:05:00.000 2014-09-10 | CDE 1007     | NJ    | Toyota | 4 x 4     | 1           | 0             | 6    | 321987654 |


Oto krótki opis kolumn:

| Kolumny | Opis |
|--------------|--------------------------------------------------------------------|
| TollID       | Identyfikator stoiska płatne, który identyfikuje stoiska płatne            |
| EntryTime    | Data i godzina wejścia pojazdu stoiska płatne w czasie UTC |
| LicensePlate | Liczba licencji drukarskiej pojazdu                            |
| Województwo        | Stan w Stanach Zjednoczonych                                           |
| Wprowadź         | Producent samochodów                                 |
| Model        | Numer modelu samochodów                                 |
| VehicleType  | 1 dla osobowego lub 2 dla pojazdy handlowych       |
| WeightType   | Waga pojazdu w ton; 0 dla osobowego                   |
| Płatne         | Wartość płatne USD                                              |
| Znacznik          | E etykieta na samochód, który pozwala zautomatyzować płatności; puste miejsce, w którym płatności wykonano ręcznie |


### <a name="exit-data-stream"></a>Zakończ strumienia danych

Zakończ strumienia danych zawiera informacje o samochody opuszczania stacji płatne.


| **TollId** | **ExitTime**                 | **LicensePlate** |
|------------|------------------------------|------------------|
| 1          | 2014-09-10T12:03:00.0000000Z | JNB 7001         |
| 1          | 2014-09-10T12:03:00.0000000Z | YXZ 1001         |
| 3          | 2014-09-10T12:04:00.0000000Z | ABC 1004         |
| 2          | 2014-09-10T12:07:00.0000000Z | XYZ 1003         |
| 1          | 2014-09-10T12:08:00.0000000Z | BNJ 1007         |
| 2          | 2014-09-10T12:07:00.0000000Z | CDE 1007         |

Oto krótki opis kolumn:


| Kolumny | Opis |
|--------------|-----------------------------------------------------------------|
| TollID       | Identyfikator stoiska płatne, który identyfikuje stoiska płatne         |
| ExitTime     | Data i godzina wyjścia pojazdu z stoiska płatne w czasie UTC |
| LicensePlate | Liczba licencji drukarskiej pojazdu                         |

### <a name="commercial-vehicle-registration-data"></a>Dane rejestracji pojazdu handlowych

W samouczku statycznej migawkę bazy danych rejestracji pojazdu komercyjnych.


| LicensePlate | Atrybutów RegistrationId | Wygasła |
|--------------|----------------|---------|
| SVT 6023     | 285429838      | 1       |
| XLZ 3463     | 362715656      | 0       |
| BK 1005     | 876133137      | 1       |
| RIV 8632     | 992711956      | 0       |
| SNY 7188     | 592133890      | 0       |
| ELH 9896     | 678427724      | 1       |

Oto krótki opis kolumn:


| Kolumny | Opis |
|--------------|-----------------------------------------------------------------|
| LicensePlate | Liczba licencji drukarskiej pojazdu                         |
| Atrybutów RegistrationId     | Identyfikator rejestracji pojazdu |
| Wygasła | Stan rejestracji pojazdu: 0, jeśli rejestracja pojazdu jest aktywne, 1, jeśli wygasła rejestracji                 |


## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Konfigurowanie środowiska dla analizy strumieniu Azure

Aby użyć tego samouczka, należy subskrypcji Microsoft Azure. Firma Microsoft udostępnia bezpłatną wersję próbną usługi Microsoft Azure.

Jeśli nie masz konta usługi Azure, możesz [żądanie bezpłatną wersję próbną](http://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Aby utworzyć konto w bezpłatnej wersji próbnej, należy urządzenia przenośnego, który może odbierać wiadomości SMS i prawidłowy kartą kredytową.

Pamiętaj o postępuj zgodnie z instrukcjami w sekcji "Oczyść Azure konta" na końcu tego artykułu, tak, aby można było wprowadzać najlepsze wykorzystanie 200 zł wolny Azure kredytowej.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Zapewnij Azure zasoby wymagane do samouczka

Ten samouczek wymaga dwóch koncentratory zdarzeń do odbierania *wpis* i *opuszczanie* strumienia danych. Baza danych SQL Azure wyświetla wyniki analizy strumieniu zadań. Magazyn Azure przechowuje dane odwołanie o rejestracji pojazdu.

Skrypt Setup.ps1 w folderze TollApp na GitHub umożliwia tworzenie wszystkich wymaganych zasobów. W celu czas zalecamy, uruchom go. Jeśli chcesz dowiedzieć się więcej na temat sposobu konfigurowania tych zasobów w portalu Azure, zapoznaj się z dodatku "Konfigurowanie samouczka zasobów w Azure portal".

Pobierz i Zapisz pomocniczego [TollApp](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) i plików.

Otwórz **Program Microsoft Azure programu PowerShell** okna _jako administrator_. Jeśli nie masz jeszcze Azure programu PowerShell, postępuj zgodnie z instrukcjami [instalowania i konfigurowania środowiska PowerShell Azure](../powershell-install-configure.md) go zainstalować.

Ponieważ system Windows automatycznie blokuje .ps1, dll i plików .exe, należy ustawić zasady wykonywania przed uruchomieniem skryptu. Upewnij się, że okno programu PowerShell Azure działa _jako administrator_. Uruchom **zestaw ExecutionPolicy bez ograniczeń**. Po wyświetleniu monitu wpisz **Y**.

![Zrzut ekranu: "Set-ExecutionPolicy nieograniczony" uruchomiony w oknie Azure programu PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Uruchom **Get-ExecutionPolicy** , aby upewnić się, że polecenie powiodło.

![Zrzut ekranu: "Get-ExecutionPolicy" uruchomiony w oknie Azure programu PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Przejdź do katalogu, w którym ma skrypty i generator aplikacji.

![Zrzut ekranu: "cd .\TollApp\TollApp" uruchomiony w oknie Azure programu PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Typ **.\\ Setup.ps1** Aby skonfigurować konto Azure, tworzenie i konfigurowanie wszystkich wymaganych zasobów i Rozpocznij, aby wygenerować zdarzeń. Skrypt przejmuje regionie losowo, aby utworzyć zasoby. Aby jawnie określić obszar, można przekazać **-lokalizacji** parametru, tak jak w poniższym przykładzie:

**. \\Setup.ps1-lokalizacji "Centralnej US"**

![Zrzut ekranu przedstawiający stronę logowania jest Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

Skrypt zostanie wyświetlona strona **Logowania w** dla Microsoft Azure. Wprowadź poświadczenia konta.

> [AZURE.NOTE] Jeśli Twoje konto ma dostęp do wiele subskrypcji, zostanie wyświetlony monit, aby wprowadzić nazwę subskrypcji, która ma być używany dla samouczka.

Skrypt może potrwać kilka minut, aby uruchomić. Po zakończeniu wynik powinna wyglądać podobnie do następujących zrzut ekranu.

![Zrzut ekranu przedstawiający dane wyjściowe skrypt w oknie Azure programu PowerShell](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Pojawi się także inne okno, który jest podobny do następujących zrzut ekranu. Ta aplikacja wysyła zdarzenia do koncentratorów zdarzenia Azure, który jest wymagany do uruchamiania samouczka. Tak nie Zatrzymaj aplikację lub zamknij to okno, aż do zakończenia samouczka.

![Zrzut ekranu: "Wysyłanie zdarzenia centrum danych"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Czy można teraz zobaczyć wszystkie zasoby utworzony w portalu Azure. Przejdź do <https://manage.windowsazure.com>i zaloguj się przy użyciu poświadczeń konta.

### <a name="azure-event-hubs"></a>Koncentratory Azure zdarzenia

Kliknij pozycję **Usługa BUS** po lewej stronie Azure portal Zobacz koncentratorów zdarzenia, które skrypt utworzony w poprzedniej sekcji.

![Usługa Bus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Zostaną wyświetlone wszystkie przestrzenie nazw dostępne w ramach subskrypcji. Kliknij ten, który zaczyna się od *tolldata* (tolldata4637388511 w naszym przykładzie). Kliknij kartę **KONCENTRATORY zdarzenia** .

![Karta koncentratory wydarzenia w portalu Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

Zostanie wyświetlona dwóch koncentratory zdarzenia o nazwie *wpis* i *opuszczanie* utworzone w tym obszarze nazw.

![Zrzut ekranu: "wpis" i "Wyjście" koncentratory wydarzenia w portalu Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)

### <a name="azure-storage-container"></a>Azure kontenera magazynu

1. Kliknij pozycję **Magazyn** po lewej stronie Azure portal Aby wyświetlić kontener Azure miejsca do magazynowania, który jest używany w samouczku.

    ![Element menu miejsca do magazynowania](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)

2. Kliknij wybrany efekt, które zaczynają się *tolldata* (tolldata4637388511 w naszym przykładzie). Kliknij kartę **kontenerów** , aby wyświetlić utworzony kontener.

    ![Karta kontenerów w portalu Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

3. Kliknij kontener **tolldata** , aby wyświetlić przekazane plik JSON, zawierający dane rejestracji pojazdu.

    ![Zrzut ekranu przedstawiający plik registration.json w kontenerze](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image13.png)

### <a name="azure-sql-database"></a>Baza danych SQL Azure

1. Kliknij pozycję **Bazy danych programu SQL** po lewej stronie Azure portal Aby wyświetlić bazy danych SQL, która będzie używana w samouczku.

    ![Bazy danych SQL](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image14.png)

2. Kliknij pozycję **tolldatadb**.

    ![Zrzut ekranu przedstawiający utworzonego bazy danych SQL](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)

3. Skopiuj nazwy serwera bez numer portu (*nazwa_serwera*. database.windows.net, na przykład).

## <a name="connect-to-the-database-from-visual-studio"></a>Nawiązywanie połączenia z bazą danych z programu Visual Studio

Aby uzyskać dostęp do wyników kwerendy w bazie danych wyjściowych za pomocą programu Visual Studio.

Nawiązywanie połączenia z bazą danych SQL (miejsce docelowe) z programu Visual Studio:

1. Otwórz program Visual Studio, a następnie kliknij pozycję **Narzędzia** > **Nawiązywanie połączenia z bazą danych**.

2. Jeśli zostanie wyświetlony monit, kliknij pozycję **Microsoft SQL Server** jako źródła danych.

    ![Okno dialogowe Zmienianie źródła danych](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)

3. W polu **Nazwa serwera** Wklej nazwę, który został skopiowany w poprzedniej sekcji z portalu Azure (czyli *nazwa_serwera*. database.windows.net).

4. Kliknij pozycję **Użyj uwierzytelniania serwera SQL**.

5. W polu **Nazwa użytkownika** wprowadź **tolladmin** i **123toll!** w polu **hasło** .

6. Kliknij przycisk **Wybierz lub wprowadź nazwę bazy danych**, a następnie wybierz **TollDataDB** jako bazę danych.

    ![Dodawanie połączenia, okno dialogowe](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)

7. Kliknij **przycisk OK**.

8. Otwórz Eksploratora serwera.

    ![Eksplorator serwera](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)

9. Zobacz czterech tabel w bazie danych TollDataDB.

    ![Tabele w bazie danych TollDataDB](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Generator zdarzenia: TollApp przykładowy projekt

Skrypt programu PowerShell zostanie uruchomiony automatycznie wysyłania zdarzeń przy użyciu próbki TollApp aplikacja. Nie musisz wykonywać żadnych dodatkowych kroków.

Jednak jeśli interesuje Cię szczegóły dotyczące implementacji, możesz znaleźć kodu źródłowego aplikacji TollApp w GitHub [Próbki TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Zrzut ekranu przedstawiający przykładowy kod wyświetlane w programie Visual Studio](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Utwórz zadanie analizy strumieniu

1. W portalu usługi Azure analizy strumieniu Otwórz, a następnie kliknij przycisk **Nowy** w lewym dolnym rogu strony, aby utworzyć nowe zadanie analizy.

    ![Przycisk Nowy](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)

2. Kliknij przycisk **Utwórz szybkie**. Wybierz pozycję tego samego regionu, w których inne zasoby są tworzone przez skrypt.

3. Ustawienia **Regionalne monitorowanie miejsca do magazynowania konta** wybierz pozycję **Utwórz nowe konto miejsca do magazynowania** i nadać mu unikatową nazwę. Przy użyciu tego konta Azure analizy strumieniu przechowywania informacji monitorowania dla wszystkich przyszłych zadań.

4. Kliknij przycisk **Utwórz zadanie analizy STRUMIENIU** u dołu strony.

    ![Tworzenie zadania analizy strumieniu opcji](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Definiowanie źródeł danych wejściowych

1. Kliknij zadanie analizy utworzony w portalu.

    ![Zrzut ekranu przedstawiający zadania analizy w portalu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image23.jpg)

2. Kliknij kartę **dane wejściowe** , aby zdefiniować dane źródłowe.

    ![Na karcie danych wejściowych](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.jpg)

3. Kliknij przycisk **Dodaj dane wejściowe**.

    ![Dodaj opcję wprowadzania](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)

4. Kliknij pozycję **strumienia danych** na pierwszej stronie.

    ![Opcja strumienia danych](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image26.png)

5. Kliknij **Zdarzenie Centrum** na drugiej stronie kreatora.

    ![Opcja Centrum zdarzenia](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image27.png)

6. Wprowadź **EntryStream** jako **ALIAS wprowadzania danych**.

7. Kliknij listę rozwijaną **Centrum zdarzenia** , a następnie wybierz ten, który zaczyna się od "TollData" (na przykład TollData9518658221).

8. Wybierz **pozycję** jako nazwa Centrum zdarzeń i **Wszystkie** nazwy zasad Centrum zdarzenia.

    Ustawienia będzie wyglądać:

    ![Ustawienia Centrum zdarzeń](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

9. Na następnej stronie wybierz **JSON** dla **Zdarzenia FORMAT SZEREGOWANIA** i **UTF8** **kodowanie**.

    ![Ustawienia szeregowania](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image29.png)

10. Kliknij **przycisk OK** w dolnej części strony, aby zakończyć działanie kreatora.

    ![Zrzut ekranu przedstawiający EntryStream wprowadzania w portalu Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image30.jpg)

    Teraz, gdy został utworzony w strumieniu wpis, będzie wykonaj te same kroki, aby utworzyć strumienia wyjścia. Trzecia strona kreatora upewnij się wprowadzić wartości jako na następujące zrzut ekranu.

    ![Ustawienia dla strumienia wyjścia](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)

    Zdefiniowano dwoma strumieniami wprowadzania:

    ![Zdefiniowane wejściowych strumieni w portalu Azure](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.jpg)

    Następnie należy dodać odwołanie danych wejściowych dla pliku obiektów blob, który zawiera dane rejestracji samochodu.

11. Kliknij pozycję **Dodaj danych wejściowych**, a następnie kliknij **Danych ŹRÓDŁOWYCH**.

    !["Dodaj dane wejściowe" Opcje zaznaczone dane odwołania](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image33.png)

12. Na następnej stronie wybierz konto miejsca do magazynowania, który zaczyna się od **tolldata**. Nazwa kontenera powinna być **tolldata**, a nazwa obiektów blob w obszarze **ŚCIEŻKA wzorzec** powinna być **registration.json**. Nazwa pliku jest uwzględniana wielkość liter i powinny być małe litery.

    ![Ustawienia przechowywania blogu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)

13. Wybierz wartości, jak pokazano w poniższej zrzut ekranu na następnej stronie, a następnie kliknij przycisk **OK** , aby zakończyć działanie kreatora.

    ![Wybór JSON "nawet szeregowania format" i UTF8 "Kodowanie"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image35.png)

Teraz wszystkie dane wejściowe są definiowane.

![Zrzut ekranu przedstawiający trzy zdefiniowane wartości wejściowych](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image36.jpg)

## <a name="define-output"></a>Definiowanie danych wyjściowych

1. Kliknij kartę **dane wyjściowe** , a następnie kliknij przycisk **Dodaj dane wyjściowe**.

    ![Karta dane wyjściowe i opcja "Dodaj wynik"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.jpg)

2. Kliknij pozycję **Baza danych SQL**.

3. Wybierz nazwę serwera, użytej w sekcji "Łączenie do bazy danych z programu Visual Studio" tego artykułu. Nazwa bazy danych jest **TollDataDB**.

4. W polu **Nazwa użytkownika** wprowadź **tolladmin** **123toll!** w polu **hasło** i **TollDataRefJoin** w polu **Tabela** .

    ![Ustawienia bazy danych SQL](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.jpg)

## <a name="azure-stream-analytics-query"></a>Azure zapytania analizy strumieniu

Karta **Kwerenda** zawiera zapytania SQL przekształcenia przychodzących danych.

![Kwerendy dodane do kartą kwerenda](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.jpg)

Pozwala podjąć próbę odpowiedzi na szereg pytań firm związanych z płatny kwerend analizy strumieniu danych i konstrukcji, których można używać do analizy strumieniu Azure zapewnienie odpowiednich odpowiedzi tego samouczka.

Przed rozpoczęciem pierwszego zadania analizy strumieniu Przyjrzyjmy się kilku scenariuszy i składnię kwerendy.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Wprowadzenie do języka kwerend analizy strumieniu Azure
-----------------------------------------------------

Załóżmy, że trzeba zliczyć pojazdy, które wprowadź stoiska płatne. Ponieważ jest to ciągły zdarzeń, należy zdefiniować "okres." Załóżmy Modyfikuj pytanie, aby być "ile pojazdy wprowadź stoiska płatne co trzy minuty?". Jest to często nazywane statystykę tumbling.

Przyjrzyjmy się kwerendę analizy strumieniu Azure, która zawiera odpowiedzi na to pytanie:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Jak widać, analizy strumieniu Azure używa języka kwerend, takich jak SQL i dodaje kilka rozszerzenia, aby określić czas aspektów kwerendy.

Aby uzyskać więcej informacji przeczytaj o konstrukcji [Zarządzanie czasem](https://msdn.microsoft.com/library/azure/mt582045.aspx) i [Obsługa okien](https://msdn.microsoft.com/library/azure/dn835019.aspx) używane w kwerendzie w witrynie MSDN.

## <a name="testing-azure-stream-analytics-queries"></a>Testowanie kwerendy analizy strumieniu Azure

Teraz, gdy zostały zapisane z pierwszego zapytania analizy strumieniu Azure, nadszedł czas na przetestować go za pomocą przykładowe pliki danych znajduje się w folderze TollApp w ścieżce następujących czynności:

**.. \\TollApp\\TollApp\\danych**

Ten folder zawiera następujące pliki:

- Entry.JSON
- Exit.JSON
- Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Pytanie 1: Liczba pojazdy stoiska płatne

1. Otwórz Azure portal i przejdź do utworzonego zadania Azure analizy strumieniu. Kliknij kartę **kwerendy** i Wklej kwerendy z poprzedniej sekcji.

    ![Kwerendy wklejone kartą kwerenda](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image40.png)

2. Aby sprawdzić poprawność to zapytanie przykładowe dane, kliknij przycisk **Testuj** . W otwartym oknie dialogowym Przejdź do Entry.json, pliku, który zawiera dane przykładowe ze **EntryTime** strumienia zdarzenia.

    ![Zrzut ekranu przedstawiający plik Entry.json](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)

3. Sprawdź, czy wyniki kwerendy jest zgodnie z oczekiwaniami:

    ![Wyniki testu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.jpg)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>Pytania 2: Całkowity czas każdego samochodu za pośrednictwem stoiska płatne raport

Średni czas wymaganego dla samochodu za pośrednictwem płatnego umożliwia ocenianie efektywności procesu i obsługi klienta.

Aby znaleźć całkowity czas, należy dołączyć strumienia EntryTime ze strumieniem ExitTime. Połączą strumieni dla kolumny TollId i LicencePlate. **Dołączanie do** operatora należy określić czasowy swobodę, opisującą różnicę czasu przyjęcia między połączonych zdarzeń. Aby określić, że zdarzenia powinny być nie więcej niż 15 minut od siebie, będzie używana funkcja **DATEDIFF** . Stosuje się również funkcja **DATEDIFF** , aby zakończyć i godzin wpis do obliczenia rzeczywisty czas samochodu spędza w stacji płatne. Zwróć uwagę różnica stosowania **DATEDIFF** , gdy jest używana w instrukcji **SELECT** zamiast warunku **Dołączanie** .

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. Aby przetestować tę kwerendę, należy zaktualizować kwerendy na karcie **Kwerenda** zadania:

    ![Zaktualizowane kwerendy na karcie zapytania](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.jpg)

2. Kliknij przycisk **Testuj** i określ przykładowe pliki wprowadzania EntryTime i ExitTime.

    ![Zrzut ekranu: wybrane pliki wprowadzania danych](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image44.png)

3. Zaznacz pole wyboru, aby przetestować kwerendę i wyświetlić wyniki:

    ![Wynik testu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Pytania 3: Raport wszystkie pojazdy komercyjnych z wygaśnięcie rejestracji

Azure analizy strumieniu umożliwia dołączanie za pomocą strumieni danych czasowy statyczne migawek danych. Wykazać tę funkcję, należy użyć następujących pytania przykładowe.

Pojazdu komercyjnych zarejestrowanej w firmie płatne, można przekazać za pośrednictwem stoiska płatne bez zatrzymanie inspekcji. Tabeli odnośników rejestracji pojazdu komercyjnych będzie używana do identyfikowania wszystkie pojazdy komercyjnych, które wygasły rejestracji.

    SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN Registration
    ON EntryStream.LicensePlate = Registration.LicensePlate
    WHERE Registration.Expired = '1'

Aby przetestować kwerendę przy użyciu danych źródłowych, należy zdefiniować źródło wprowadzania dla danych źródłowych, które zostało jeszcze wykonane.

1. Aby przetestować tę kwerendę, Wklej kwerendy na karcie **kwerendy** , kliknij pozycję **Przetestuj**i określ dwóch źródeł danych wejściowych:

    ![Zrzut ekranu: wybrane pliki wprowadzania danych](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

2. Wyświetlić wyniki kwerendy:

    ![Zrzut ekranu przedstawiający wyniki kwerendy](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image47.png)

## <a name="start-the-stream-analytics-job"></a>Uruchom zadanie analizy strumieniu


Nadszedł czas, aby zakończyć konfigurację i uruchomić zadanie. Zapisz kwerendę z 3 pytanie, które da wynik pasującego schematu tabeli wyników **TollDataRefJoin** .

Przejdź do zadania **pulpitu NAWIGACYJNEGO**, a następnie kliknij przycisk **START**.

![Zrzut ekranu przedstawiający przycisk Start na pulpicie nawigacyjnym zadania](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.jpg)

W wyświetlonym oknie dialogowym Zmień czas **START wyjściowy** na **czas niestandardowe**. Zmień godziny na godzinę przed bieżącą godzinę. Ta zmiana zapewnia, że wszystkie zdarzenia z poziomu Centrum wydarzenia są przetwarzane od momentu rozpoczęcia do wygenerowania zdarzenia na początku samouczka. Teraz kliknij znacznik wyboru, aby uruchomić zadanie.

![Zaznaczenie niestandardowe czasu](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

Uruchamianie zadania może potrwać kilka minut. Można wyświetlić stan na stronie najwyższego poziomu dla analizy strumieniu.

![Zrzut ekranu przedstawiający stan zadania](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.jpg)

## <a name="check-results-in-visual-studio"></a>Wyniki sprawdzania w programie Visual Studio

1. Otwórz program Visual Studio Server Explorer, a następnie kliknij prawym przyciskiem myszy tabelę **TollDataRefJoin** .
2. Kliknij pozycję **Pokaż dane w tabeli** , aby wyświetlane zadania.

    ![Wybór "Pokaż tabelę danych" w Eksploratorze serwera](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Zadania Azure analizy strumieniu możliwość skalowania

Azure analizy strumieniu elastically skali dzięki temu może obsługiwać wiele danych. Kwerenda analizy strumieniu Azure umożliwia klauzulę **PARTITION** sprawdzić system, który będzie skalowania ten krok. **Identyfikator_partycji** jest kolumnę specjalnych, które system doda zgodnie z identyfikator wprowadzania (zdarzenia Centrum).

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Zatrzymać bieżące zadanie, aktualizacji kwerendy na karcie **kwerendy** , a następnie otwórz kartę **Skala** .

    **Przesyłanie strumieniowe jednostki** zdefiniuj ilość energii obliczeń, który może odbierać zadania.

2. Przesuń suwak do 6.

    ![Zrzut ekranu: Wybieranie 6 przesyłanie strumieniowe jednostki](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.jpg)

3. Przejdź do karty **wyników** i Zmień nazwę tabeli SQL na **TollDataTumblingCountPartitioned**.

Jeśli zadanie zostanie uruchomione teraz Azure analizy strumieniu można rozłożenie pracy w większej ilości zasobów obliczeń i osiągnięcia większa przepustowość. Pamiętaj, że aplikacja TollApp również wysyła zdarzenia podzielona według TollId.

## <a name="monitor"></a>Monitorowanie

Na karcie **MONITOR** zawiera statystykę uruchomione zadanie.

![Zrzut ekranu przedstawiający statystyki dotyczące wykonywania zadań](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image53.png)

Na karcie **pulpitu NAWIGACYJNEGO** można korzystać z **Dzienników operacji** .

![Opcja "Dzienniki operacji"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image54.jpg)

![Zrzut ekranu: dzienniki operacji możesz wyświetlać stan zadań](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image55.png)

Aby wyświetlić dodatkowe informacje dotyczące określonego zdarzenia, kliknij zdarzenie, a następnie kliknij przycisk **Szczegóły** .

![Zrzut ekranu przedstawiający szczegóły dotyczące zaznaczonego zdarzenia](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image56.png)

## <a name="conclusion"></a>Wnioski

Ten samouczek wprowadzeniem do usługi Azure analizy strumieniu. Jak skonfigurować dane wejściowe i wyjściowe zadania analizy strumieniu go wykazać. Korzystając z tego scenariusza danych płatne, samouczka omówienie typowych problemów, które mogą pojawić się w obszarze danych w ruchu i jak można rozwiązać za pomocą prostych kwerend SQL podobne do analizy strumieniu Azure. Samouczek opisane konstrukcji rozszerzenia SQL do pracy z danymi czasowy. Jego pokazano, jak dołączyć do strumieni danych, jak wzbogacić strumienia danych z danych źródłowych statyczne oraz sposobu skalowania kwerendy w celu uzyskania wyższej przepustowości.

Mimo że ten samouczek zawiera wprowadzenie dobre, nie została ukończona w dowolny sposób. Można znaleźć więcej wzorców kwerendy przy użyciu języka SAQL u [kwerendy przykłady typowych upodobania analizy strumieniu](stream-analytics-stream-analytics-query-patterns.md).
Można znaleźć w [dokumentacji online](https://azure.microsoft.com/documentation/services/stream-analytics/) , aby dowiedzieć się więcej na temat analizy strumieniu Azure.

## <a name="clean-up-your-azure-account"></a>Oczyść konto Azure

1. Zatrzymaj zadanie analizy strumieniu w portalu Azure.

    Skrypt Setup.ps1 tworzy dwa koncentratory zdarzeń i bazy danych SQL. Poniższe instrukcje pomóc Ci oczyścić zasoby na koniec samouczka.

2. W oknie programu PowerShell wpisz **.\\ CleanUp.ps1** uruchomić skrypt, który usuwa zasobów używanych w samouczku.

    > [AZURE.NOTE] Zasoby są oznaczane nazwę. Upewnij się, że dokładnie przejrzyj poszczególne elementy przed potwierdzeniem usunięcia.

    ![Zrzut ekranu przedstawiający proces oczyszczania](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image57.png)
