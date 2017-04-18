<properties
    pageTitle="Korzystanie z aplikacji App-V z Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak korzystać z aplikacji App-V w Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="using-app-v-apps-in-azure-remoteapp"></a>Korzystanie z aplikacji App-V w Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Za pomocą aplikacji App-V w zbiorze hybrydowych Azure RemoteApp wymaga Dołączanie domeny.

Przed rozpoczęciem, upewnij się zainstalować klienta App-V 5.1 z najnowszymi aktualizacjami. Należy utworzyć [obraz niestandardowy](remoteapp-create-custom-image.md) , który zawiera klienta App-V.  

Jest łatwe do istniejącej infrastruktury App-V za pomocą Azure RemoteApp. Zbiór hybrydowego jest używany do VNET Azure, która ma dostęp do kontrolera domeny i maszyny wirtualne są domeny sprzężone, mogą korzystać z istniejącej aplikacji v infrastruktury i wdrażania metody aplikacji App-V hosta easyily w Azure RemoteApp. Oto kilka zagadnień, które należy pamiętać o zależy od typu wdrożenia App-V, którą obecnie masz:

| Opcje konfiguracji |                       | Dodatnia                                                               | Ujemne                                                                                              |
|-----------------------|-----------------------|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Metoda dostarczania       | Przesyłanie strumieniowe (na żądanie) | Aplikacja jest zawsze najnowsza i nowe                                     | Pierwszy opóźnienie czasu                                                                                    |
|                       | Zainstalowany               | Najszybszy; aplikacja znajduje się już na maszyn wirtualnych                              | Zwiększanie rozmiaru - ma miejsce obrazu (limit 127 GB)                                                           |
| Aplikacja lokalizacji przechowywania  | Zawartości udostępnionej        | Aplikacja działa w pamięci wystąpienia Azure RemoteApp                         | Eats pamięci i dobre połączenie streaming server (plik), w którym znajduje się aplikacja                      |
|                       | Dysku (buforowane)         | Szybkie wykonanie. Aplikacja nie jest zależna od dostępności źródła zawartości | Zwiększanie rozmiaru - ma miejsce obrazu (limit 127 GB)                                                           |
| Określanie wartości docelowej             | Użytkownika                  | Wymaga pełnego autonomicznego infrastruktury App-V                          |                                                                                                       |
|                       | Szablon globalny (komputer)      |  Wstępnie publikowanie lub docelowego, za pomocą publikowania serwera                         |  Wymagana jest aktualizacja Azure obrazu, jeśli chcesz zaktualizować aplikację (duży). Zajmie trochę miejsca na obraz. |

 Po utworzeniu obraz niestandardowy i kolekcji hybrydowych publikowanie aplikacji, przypisywanie użytkowników i korzystać z istniejących aplikacji App-V obsługiwany w RemoteApp Azure dostarczane do dowolnego urządzenia w dowolnym miejscu.
