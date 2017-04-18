<properties
pageTitle="Używać danych z bazy danych programu SQL Server lokalnego w maszynowego uczenia | Azure"
description="Aby wykonać zaawansowanej analizy z Azure maszynowego uczenia, należy użyć danych z bazy danych programu SQL Server lokalnego."
services="machine-learning"
documentationCenter=""
authors="garyericson"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/16/2016"
ms.author="garye;krishnan"/>

# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Wykonywanie zaawansowanej analizy nauki komputera Azure za pomocą danych z bazy danych programu SQL Server lokalnego


Często przedsiębiorstw, które współpracują z danych lokalnych chcesz wykonać korzyści skali i elastyczność chmury dla ich maszynowego uczenia obciążenia. Ale nie chcesz przerwać bieżącą procesów biznesowych i przepływy pracy, przenosząc ich danych lokalnych z chmurą. Azure nauki komputer obsługuje teraz odczytywanie danych z bazy danych programu SQL Server w środowisku lokalnym, a następnie szkolenia i wyników modelu danych. Masz już ręczne kopiowanie i synchronizowanie danych między chmury i serwera lokalnego. Zamiast tego modułu **Importowanie danych** w Azure maszynowego uczenia Studio umożliwia teraz odczyt bezpośrednio z lokalnej bazy danych programu SQL Server do szkolenia i wyników zadania. 

Ten artykuł zawiera omówienie sposobu przedostania lokalnego danych programu SQL server do nauki maszynowego Azure. Przyjęto założenie, że znasz Azure maszynowego uczenia pojęcia, takie jak *obszary robocze, moduły, zestawy danych, doświadczeń itp*.

> [AZURE.NOTE] Ta funkcja nie jest dostępna dla bezpłatne obszarów roboczych. Aby uzyskać więcej informacji na temat maszynowego uczenia ceny i poziomów, zobacz [Azure maszynowego uczenia ceny](https://azure.microsoft.com/pricing/details/machine-learning/).

<!-- --> 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="install-the-microsoft-data-management-gateway"></a>Instalowanie bramy zarządzania danymi firmy Microsoft

Aby uzyskać dostęp do bazy danych programu SQL Server lokalnego w Azure maszynowego uczenia musisz pobrać i zainstalować bramę zarządzania danymi firmy Microsoft. Po skonfigurowaniu połączenia bramy w komputerze nauki Studio masz szansy sprzedaży, aby pobrać i zainstalować bramy przy użyciu okna dialogowego **plik do pobrania i bramy danych rejestru** opisane poniżej.

Możesz również zainstalować bramy zarządzania danymi wyprzedzeniem, pobierając i uruchomiony pakiet Instalatora MSI z [Centrum pobierania Microsoft](https://www.microsoft.com/download/details.aspx?id=39717). Wybierz najnowszą wersję, wybierając 32-bitowej lub 64-bitowej, zależnie od potrzeb dla Twojego komputera. Plik MSI można także uaktualniania istniejącej bramy zarządzania danymi do najnowszej wersji z wszystkimi ustawieniami zachowywane.

Brama ma następujące wymagania:

- Obsługiwane wersje systemu operacyjnego Windows są systemu Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 i Windows Server 2012 R2.
- Zalecana konfiguracja komputera bramy jest co najmniej 2 GHz, 4 rdzenie 8 GB pamięci RAM i dysk 80 GB.
- Jeśli na komputerze obsługującym w stan hibernacji, bramy nie będzie można reagować na żądania danych. W związku z tym należy skonfigurować plan odpowiednie uprawnienia na komputerze, przed zainstalowaniem bramy. Instalacja bramy wyświetli komunikat, jeśli komputer jest skonfigurowany do hibernacji.
- Ponieważ aktywności kopii występuje na określonych częstotliwości, użycie zasobów (Procesora, pamięci) na komputerze również zgodny ze wzorcem samej z Szczyt i czasu bezczynności. Wykorzystanie zasobów zależy również intensywnie ilość danych jest przenoszony. W przypadku wielu zadań kopiowania w toku będzie obserwować przejść o czasie Szczyt użycie zasobu. Podczas technicznego wystarcza minimalna konfiguracja wymienionych powyżej, być może zechcesz konfiguracji z większej ilości zasobów niż minimalna konfiguracja w zależności od usługi Obciążenie dla przenoszenia danych.

Należy rozważyć następujące czynności podczas konfigurowania i korzystania z bramy zarządzania danymi:

- Na jednym komputerze można zainstalować tylko jedno wystąpienie bramy zarządzania danymi.
- Za pomocą jednej bramy dla wielu lokalnych źródeł danych.
- Umożliwia nawiązanie połączenia wielu bram na różnych komputerach z tym samym źródłem danych lokalnych.
- Możesz skonfigurować bramę dla tylko jednego obszaru roboczego w danej chwili. Bram nie można udostępniać między obszarów roboczych w tej chwili.
- Możesz skonfigurować wielu bram dla pojedynczego obszaru roboczego. Na przykład można używać bramy, która jest podłączony do źródła danych test podczas opracowywania i bramy produkcji, gdy będzie gotowe do operationalize.
- Bramy nie musi znajdować się na tym samym komputerze jako źródła danych, ale utrzymywanie bliżej źródła danych skraca czas bramy połączyć się ze źródłem danych. Zaleca się zainstalowanie bramy na komputerze, na którym jest inny niż ten, który obsługuje lokalnego źródła danych, tak aby bramy i źródła danych nie współzawodniczenia dla zasobów.
- Jeśli masz już zainstalowany na Twoim komputerze serwowania scenariusze dotyczące usługi Power BI lub fabrycznych danych Azure bramy, zainstalować bramę osobnych Azure szkoleniowe na komputerze na innym komputerze. 

    > [AZURE.NOTE] Nie można uruchomić bramy zarządzania danymi i Power BI bramy na tym samym komputerze.

- Należy używać bramy zarządzania danymi dla Azure maszynowego uczenia, nawet jeśli korzystasz z platformy Azure ExpressRoute dla innych danych. Źródła danych powinna być traktować jako źródło danych lokalnych (który znajduje się za zaporą) nawet gdy używasz ExpressRoute, a ustanowić połączenie między maszynowego uczenia się i źródła danych za pomocą bramy zarządzania danymi. 

Szczegółowe informacje dotyczące wymagania wstępne dotyczące instalacji, kroki instalacji i porady dotyczące rozwiązywania problemów można znaleźć w artykule [Przenoszenie danych między lokalnych źródeł i chmura z bramy zarządzania danymi](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway), począwszy od sekcji [Zagadnienia dotyczące korzystania z bramy zarządzania danymi](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Ingress danych z bazy danych programu SQL Server lokalnego do nauki maszynowego Azure


W tym instruktażu będą Konfigurowanie bramy zarządzania danymi w obszarze roboczym Azure maszynowego uczenia, skonfiguruj go, a następnie przeczytaj danych z bazy danych programu SQL Server lokalnego.

> [AZURE.TIP] Zanim zaczniesz, wyłączanie blokowania wyskakujących okienek w przeglądarce w `studio.azureml.net`. Jeśli używasz przeglądarki Google Chrome, Pobierz i zainstaluj jedną z kilku wtyczki dostępnego magazynu sieci Web Google Chrome [Kliknij jeden raz aplikacji rozszerzenia](https://chrome.google.com/webstore/search/clickonce?_category=extensions).

### <a name="step-1-create-a-gateway"></a>Krok 1: Tworzenie bramy

Pierwszym krokiem jest tworzenie i konfigurowanie bramy, aby uzyskać dostęp do bazy danych SQL lokalnego.

1. Zaloguj się do [Azure maszynowego uczenia Studio](https://studio.azureml.net/Home/) i wybierz pozycję obszar roboczy, który chcesz pracować.

2. Kliknij pozycję Karta **Ustawienia** po lewej stronie, a następnie kliknij kartę **BRAM danych** u góry.

3. Kliknij pozycję **Nowa BRAMA danych** w dolnej części ekranu.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)

4. W oknie dialogowym **Nowa brama danych** wprowadź **Nazwę bramy** i opcjonalnie dodać **Opis**. Kliknij strzałkę w prawym dolnym rogu aby przejść do następnego kroku konfiguracji.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)

5. W oknie pobierania i Rejestruj danych bramy dialogowym skopiuj klucza rejestru bramy do Schowka.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)

6. <span id="note-1" class="anchor"></span>Jeśli jeszcze nie zostały pobrane i zainstalowane bramy zarządzania danymi firmy Microsoft, kliknij pozycję **Pobierz bramy zarządzania danymi**. Spowoduje to przejście do witryny Microsoft Download Center miejsce, w którym można wybrać wersję bramy, którego potrzebujesz, pobierz go i go zainstalować. Szczegółowe informacje dotyczące wymagania wstępne dotyczące instalacji, kroki instalacji i porady dotyczące rozwiązywania problemów można znaleźć w sekcji początku tego artykułu, [Przenoszenie danych między lokalnych źródeł i chmura z bramy zarządzania danymi](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

7. Po zainstalowaniu bramy Menedżer konfiguracji bramy zarządzania danymi zostanie otwarty i zostanie wyświetlone okno dialogowe **rejestrować bramę** . Wklej **Klucza rejestru bramy** , który został skopiowany do Schowka i kliknij przycisk **Zarejestruj**.

8. Jeśli masz już zainstalowany bramy, uruchom Menedżer konfiguracji bramy zarządzania danymi, kliknij pozycję **Zmień klucz**, Wklej  **Klucza rejestru bramy** , który został skopiowany do Schowka i kliknij **przycisk OK**.

9. Po zakończeniu instalacji okna dialogowego **rejestrować bramę** dla programu Microsoft danych Menedżer konfiguracji bramy zarządzania jest wyświetlany. Wklej klucza rejestracji bramy, który został skopiowany do Schowka powyżej i kliknij przycisk **Zarejestruj**.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)

10. Zakończeniu konfiguracji bramy, gdy następujące wartości są ustawione na karcie **Narzędzia główne** w programie Microsoft danych Menedżer konfiguracji bramy zarządzania:

    - **Nazwa bramy** i **nazwę wystąpienia** są ustawione na nazwie bramy.

    - **Rejestracja** jest ustawiona wartość **zarejestrowano**.

    - **Stan** jest ustawiony na **wprowadzenie**.

    - Na pasku stanu u dołu Wyświetla **Połączono z usługi w chmurze brama zarządzania danymi** wraz z zielony znacznik wyboru.

     ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

     Azure maszynowego uczenia Studio również aktualizowany po pomyślnym rejestracji.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-registered.png)

11. W oknie dialogowym **Pobieranie i rejestrować bramę danych** kliknij pole wyboru, aby zakończyć konfigurację. Na stronie **Ustawienia** jest wyświetlany stan bramy jako "W trybie Online". W okienku po prawej znajdziesz, stan i inne przydatne informacje.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-status.png)

12. W oknie Menedżer konfiguracji bramy zarządzania danymi firmy Microsoft przełączanie na karcie **certyfikatu** . Certyfikat określony na tej karcie jest używany do Szyfrowanie/odszyfrowywanie poświadczenia dla lokalnego magazynu danych przez użytkownika w portalu. To jest domyślny certyfikat wygenerowany. Firma Microsoft zaleca zmiana tej do własnego certyfikatu tworzenie kopii zapasowych w systemie zarządzania certyfikatu. Kliknij przycisk **Zmień** , aby użyć własnego certyfikatu.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-certificate.png)

13. (opcjonalnie) Jeśli chcesz włączyć pełne rejestrowanie w celu rozwiązywania problemów z bramy, w programie Microsoft danych Menedżer konfiguracji bramy zarządzania przejdź do karty **Narzędzia diagnostyczne** i zaznacz opcję **Włącz rejestrowanie na potrzeby rozwiązywania problemów** . Rejestrowanie informacji można znaleźć w Podglądzie zdarzeń systemu Windows w obszarze **Dzienniki aplikacji i usług**  - &gt; węzeł **Bramy zarządzania danymi** . Za pomocą karty **Narzędzia diagnostyczne** testowania połączenia do lokalnego źródła danych przy użyciu bramy.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-verbose-logging.png)

Na tym kończy proces w Azure maszynowego uczenia bramy.
Teraz możesz użyć danych lokalnych.

Można utworzyć i skonfigurować wielu bram w Studio dla każdego obszaru roboczego. Na przykład może być bramy, którą chcesz nawiązać połączenie źródła danych test podczas opracowywania i różnych bramy dla źródła danych produkcji. Azure nauki komputera pozwala Konfigurowanie wielu bram w zależności od firmie. Obecnie nie mogą współużytkować bramy między obszarów roboczych i tylko jednej bramy, które można zainstalować na jednym komputerze. Aby uzyskać więcej na temat podczas instalowania bramy zobacz [Zagadnienia dotyczące korzystania z bramy zarządzania danymi](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) w artykule [Przenoszenie danych między lokalnych źródeł i chmura z bramy zarządzania danymi](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Krok 2: Odczytywanie danych z lokalnego źródła danych za pomocą bramy

Po skonfigurowaniu bramy można dodać moduł **Importowanie danych** do doświadczenia, który wejść danych z bazy danych programu SQL Server lokalnego.

1.  W komputerze nauki Studio wybierz kartę **DOŚWIADCZEŃ** , kliknij pozycję **+ Nowe** w lewym dolnym rogu i wybierz **Pusty doświadczenia** (lub wybierz jedną z kilku doświadczeń przykładowe dostępne).

2.  Znajdź, a następnie przeciągnij moduł **Importowanie danych** do obszaru roboczego doświadczenia.

3.  Poniżej kanwy, kliknij polecenie **Zapisz jako** . Wprowadź "Azure komputera nauki lokalnego programu SQL Server samouczek" dla nazwy doświadczenia, zaznacz obszar roboczy, a następnie kliknij **przycisk OK** znacznik wyboru.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\experiment-save-as.png)

4.  Kliknij polecenie module **Importowanie danych** , aby go zaznaczyć, a następnie w okienku **Właściwości** po prawej stronie obszaru roboczego, wybierz pozycję "Lokalnej bazy danych SQL" na liście rozwijanej **źródło danych** .

5.  Zaznacz **dane bramy** zainstalowany i zarejestrowany. Możesz skonfigurować innej bramy, wybierając pozycję "(Dodaj Nowa brama danych...)".

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-select-on-premises-data-source.png)

6.  Wprowadź **nazwę bazy danych**, wraz z SQL **kwerendy w bazie danych** chcesz wykonać i SQL **Nazwa serwera bazy danych** .

7.  Kliknij pozycję **Wprowadź wartości** w polach **Nazwa użytkownika i hasło** , a następnie wprowadź poświadczenia bazy danych. Za pomocą zintegrowanego uwierzytelniania systemu Windows lub uwierzytelnianie programu SQL Server w zależności od sposobu skonfigurowania lokalnego serwera SQL.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\database-credentials.png)
    
    Komunikat "wartości wymagane" zostanie zmieniona na "zestawu wartości" z zielony znacznik wyboru. Wystarczy raz wprowadź poświadczenia, o ile nie zmienia się informacje z bazy danych lub hasło. Azure nauki komputera używa certyfikatu, podanych podczas instalowania bramę do szyfrowania poświadczeń w chmurze. Azure nigdy nie będą przechowywane poświadczenia lokalnego bez szyfrowania.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-properties-entered.png)

8.  Kliknij przycisk **Uruchom** , aby uruchomić doświadczenia.

Po zakończeniu doświadczenia zostanie uruchomiony można zobrazować importowanego z bazy danych, klikając pozycję port wyjściowy modułu **Importowanie danych** i wybierając **Wizualizacja**.

Po zakończeniu opracowywania usługi doświadczenia, możesz wdrożyć i operationalize modelu. Za pomocą usługi wykonanie partię, danych z bazy danych programu SQL Server lokalnego, które zostały skonfigurowane w module **Importowanie danych** będzie więcej i używane do określania wyników. Podczas korzystania z usługi odpowiedzi żądanie, wyników danych lokalnych, firma Microsoft zaleca za pomocą [dodatku programu Excel](machine-learning-excel-add-in-for-web-services.md) . Obecnie pisania do lokalnego programu SQL Server bazy danych za pomocą **Eksportowanie danych** nie jest obsługiwana w swojej doświadczeń lub usług opublikowanych sieci web.

