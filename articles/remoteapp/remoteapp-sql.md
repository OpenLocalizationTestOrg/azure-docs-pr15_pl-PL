<properties
   pageTitle="Platformy SQL Azure z Azure RemoteApp | Microsoft Azure"
   description="Dowiedz się, jak platformy SQL Azure za pomocą Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="ericorman"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="sql-azure-with-azure-remoteapp"></a>Platformy SQL Azure z Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Często, gdy klienci wybierz obsługi ich aplikacji systemu Windows w chmurze z Azure RemoteApp chcą przeprowadzić migrację swoich danych, takich jak serwery SQL do chmury wdrożenia całą chmurą. Dzięki temu całą chmurą hostowanej rozwiązanie, które są dostępne w dowolnym momencie dla dowolnego urządzenia z dowolnego miejsca za pomocą Azure RemoteApp. Poniżej znajdują się łącza i odsyłacze wraz z wytyczne pomagające ten proces.  

## <a name="migrate-your-sql-data"></a>Migrowanie danych SQL

Rozpoczyna się [Migrowanie bazy danych programu SQL Server do bazy danych SQL Azure](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Konfigurowanie funkcji RemoteApp Azure
Udostępniać aplikacji systemu Windows Azure RemoteApp. Poniżej przedstawiono bardzo wysoki poziom krok po kroku:

1.     Utwórz [szablon Azure RemoteApp maszyn wirtualnych](remoteapp-imageoptions.md). 
2.     Zainstaluj wymagana aplikacja na maszyn wirtualnych.
3.     Konfigurowanie aplikacji, aby go łączy do bazy danych SQL i upewnij się, że działa.
4.     Narzędzie Sysprep i zamknięcia maszyn wirtualnych. Przechwyć to jako obraz do użytku z Azure. **Uwaga:** Potrzebujesz upewnić się, że aplikacja jest w stanie przechowywania informacji łączności bazy danych za pomocą procesu sysprep. Jeśli aplikacja jest nie można zachować informacje o połączeniu bazy danych, może być nawiązanie z dostawcą aplikacji, aby sprawdzić, jak można określić parametry połączenia.
5.     Importowanie obraz niestandardowy do biblioteki Azure RemoteApp, wybierając odpowiednie położenie geograficzne, znajdujący się z wdrożeniem platformy SQL Azure. 
6.     Wdrażanie zbioru RemoteApp w tym samym centrum danych jako wdrożenia platformy SQL Azure za pomocą tego szablonu powyżej i opublikować aplikację. Wdrażanie RemoteApp Azure w tym samym centrum danych jako swojej platformy SQL Azure wdrożenia pomaga zapewnić najszybszy szybkość połączenia i zmniejszyć opóźnienie. 

## <a name="app-and-sql-configuration-considerations"></a>Aplikacja i SQL zagadnienia dotyczące konfiguracji:
Istnieje kilka kwestie do rozważenia podczas korzystania z trybu RemoteApp Azure SQL:

Dowiedz się, [jak skonfigurować zaporę bazy danych Azure SQL](../sql-database/sql-database-firewall-configure.md). Fragment Państw artykuł "początkowo dostęp do serwera bazy danych SQL Azure jest zablokowany przez zaporę. Aby rozpocząć korzystanie z serwera bazy danych SQL Azure, możesz przejdź do portalu klasyczny i określ jedną lub więcej reguł zaporę na poziomie serwera, które umożliwiają dostęp do serwera bazy danych SQL Azure. Umożliwia reguły zapory Określ, które zakresy adresów IP z Internetu są dozwolone i czy Azure aplikacji może próbować połączyć się z serwerem bazy danych SQL Azure."

Ponadto, gdy komputer próba nawiązania połączenia z serwerem bazy danych z Internetu, zapory sprawdza adres IP źródłowym żądanie pełny zestaw poziomie serwera i (w razie potrzeby) poziomie bazy danych reguły zapory. "W przypadku żądania adresu IP w jednej z zakresu określonego reguły zapory na poziomie serwera, połączenie otrzymuje się z serwerem bazy danych SQL Azure." W związku z tym udzielamy można użyć z zakresów adresów IP, a nie tylko adresów IP pojedynczego źródła.

Postępuj zgodnie z instrukcjami krok po kroku [jak: Konfigurowanie ustawień zapory na SQL Database za pomocą Azure Portal](../sql-database/sql-database-configure-firewall-settings.md) Aby określić zakres adresów IP. Konfigurując reguły zapory SQL, podaj zakres adresów IP podsieci, którą określono w zbiorze Azure RemoteApp. Powinno to umożliwić serwery ARA nawiązać bazy danych SQL, mimo że ta osoba będzie dynamicznie przypisano adresów IP.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
Jeśli hostowanej możliwości korzystania z aplikacji klienckiej Azure RemoteApp, który łączy SQL bazy danych w przypadku, gdy hostowanej Azure lub lokalnego jest powolne może istnieć kilka przyczyn dlaczego.  

- Czas oczekiwania sieci na urządzeniu z systemem Azure jest wysoki. Przechodzenie do najlepszym i najszybszym połączenie, które można wykonywać następujące czynności w celu uzyskania najlepszej wydajności. Aby sprawdzić czas opóźnienia urządzeń z centrum danych Azure za pomocą [azurespeed.com](http://azurespeed.com/) jako narzędzie ogólnego.  
- Jest obsługiwany w Azure RemoteApp aplikacji klienckiej przy dużym obciążeniu. Wybieranie innego planu rozliczeń takich jak rozliczeń Premium zwiększa wydajność. Inny jest monitorowanie aplikacji jest przez inne zasoby: podczas sesji wykonywanie sekwencja klawiszy ctrl-alt-end, uruchamianie ekranu skojarzeń zabezpieczeń, wybierz pozycję Menedżer zadań i obserwować wykorzystanie zasobów dla aplikacji sieci.
- Program SQL server jest przy dużym obciążeniu lub nie zostały zoptymalizowane. Wykonaj SQL wskazówki dotyczące rozwiązywania problemów. 

