<properties 
   pageTitle="Konfigurowanie i używanie emulatora miejsca do magazynowania z programem Visual Studio | Microsoft Azure"
   description="Konfigurowanie i używanie emulatora miejsca do magazynowania z programem Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Konfigurowanie i używanie emulatora miejsca do magazynowania z programem Visual Studio

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Omówienie
Środowisko projektowania Azure SDK zawiera emulatora miejsca do magazynowania, narzędzie symuluje obiektów Blob, kolejki i tabeli miejsca do magazynowania dostępne usługi platformy Azure na komputerze lokalnym rozwoju. W przypadku tworzenia usługi w chmurze, która używa usług Azure magazynu lub pisania zewnętrzną aplikację, która wymaga usług miejsca do magazynowania, można przetestować kod lokalnie przed emulatora miejsca do magazynowania. Narzędzia Azure dla programu Microsoft Visual Studio zintegrować Visual Studio zarządzania emulatora miejsca do magazynowania. Narzędzia Azure zainicjować bazy danych magazynu emulatora przy pierwszym użyciu, uruchomienie usługi emulatora miejsca do magazynowania, podczas uruchamiania lub debugowania kodu z programu Visual Studio i zapewnia dostęp tylko do odczytu do miejsca do magazynowania danych emulatora za pomocą Eksploratora magazynu Azure.

Szczegółowe informacje na temat emulatora miejsca do magazynowania, w tym wymagania systemowe i instrukcje dotyczące konfigurowania niestandardowych zobacz [Używanie Azure emulatora miejsca do magazynowania dla rozwoju i testowanie](./storage/storage-use-emulator.md).

>[AZURE.NOTE] Istnieją pewne różnice w funkcjach między symulacji emulatora miejsca do magazynowania i usługi Azure miejsca do magazynowania. Zobacz [różnice między emulatora miejsca do magazynowania i usługi Azure miejsca do magazynowania](./storage/storage-use-emulator.md) w dokumentacji Azure SDK informacje o różnicach określone.

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Konfigurowanie parametrów połączenia dla emulatora miejsca do magazynowania

Do nich dostęp emulatora miejsca do magazynowania z kodami roli, należy skonfigurować parametry połączenia, który wskazuje emulatora miejsca do magazynowania i można to zmienić później, aby wskazywały konto Azure miejsca do magazynowania. Parametry połączenia jest ustawienie konfiguracji, który może zostać odczytany Twoja rola w czasie rzeczywistym w celu nawiązania połączenia z kontem miejsca do magazynowania. Aby uzyskać więcej informacji na temat tworzenia ciągów połączeń zobacz [Konfigurowanie aplikacji Azure](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

>[AZURE.NOTE] Właściwość **DevelopmentStorageAccount** można przywrócić odwołanie do konta emulatora miejsca do magazynowania w kodzie. Ta metoda działa poprawnie, jeśli chcesz uzyskać dostęp emulatora miejsca do magazynowania w kodzie, ale jeśli chcesz opublikować aplikacji Azure, należy utworzyć parametry połączenia do uzyskiwania dostępu do konta magazynu platformy Azure i modyfikowania swój kod, aby użyć tego ciągu połączenia przed opublikowaniem. Aby przełączyć się między kontem emulatora miejsca do magazynowania i konto Azure magazynowania często, parametry połączenia będzie uprościć ten proces.

## <a name="initializing-and-running-the-storage-emulator"></a>Inicjowanie i uruchamianie emulatora miejsca do magazynowania

Można określić, że podczas uruchamiania lub debugowania usługi w programie Visual Studio, Visual Studio powoduje automatyczne uruchomienie emulatora miejsca do magazynowania. W oknie Eksplorator rozwiązań Otwórz menu skrótów dla projektu **Azure** i wybierz polecenie **Właściwości**. Na karcie **rozwoju** na liście **Rozpoczynanie emulatora miejsca do magazynowania Azure** wybierz **wartość PRAWDA** (Jeśli nie został już ustawiony do tej wartości).

Podczas pierwszego uruchamiania lub debugowania usługi z programu Visual Studio emulatora miejsca do magazynowania uruchamia proces inicjowania. Ten proces zastrzega sobie portów lokalnych dla emulatora miejsca do magazynowania i jest tworzona baza danych emulatora miejsca do magazynowania. Po zakończeniu tego procesu nie musi ponownie uruchomić, chyba że bazy danych magazynu emulatora zostanie usunięty.

>[AZURE.NOTE] Począwszy od wersji czerwca 2012 narzędzia Azure, emulatora miejsca do magazynowania działa domyślnie w SQL Express LocalDB. W starszych wersjach narzędzia Azure, emulatora miejsca do magazynowania działa w odniesieniu do domyślne wystąpienie programu SQL Express 2005 lub 2008, które należy zainstalować przed zainstalować Azure SDK. Możesz również uruchomić emulatora miejsca do magazynowania przed nazwane wystąpienie programu SQL Express lub nazwanego lub wystąpienia domyślnego programu Microsoft SQL Server. Jeśli musisz skonfigurować emulatora miejsca do magazynowania w celu uruchomienia wystąpienia innych niż domyślne wystąpienie, zobacz [Używanie Azure emulatora miejsca do magazynowania dla rozwoju i testowanie](./storage/storage-use-emulator.md).

Emulator miejsca do magazynowania znajdują się w interfejsie użytkownika, aby wyświetlić stan usług magazynu lokalnego i aby uruchomić, zatrzymać, a ich ponownie. Po rozpoczęciu usługa emulatora magazynu można wyświetlać interfejs użytkownika lub uruchamianie lub zatrzymywanie usługi klikając prawym przyciskiem myszy ikona w obszarze powiadomień dla emulatora Microsoft Azure na pasku zadań systemu Windows.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Wyświetlanie danych emulatora miejsca do magazynowania w Eksploratorze serwera

Węzeł magazyn Azure w Eksploratorze serwera umożliwia wyświetlanie danych i zmienić ustawienia dla obiektów blob i tabeli danych kont miejsca do magazynowania, łącznie z emulatora miejsca do magazynowania. Aby uzyskać więcej informacji, zobacz [Przeglądanie i zarządzanie zasobami magazynowania w Eksploratorze serwera](https://msdn.microsoft.com/library/azure/ff683677.aspx) .
