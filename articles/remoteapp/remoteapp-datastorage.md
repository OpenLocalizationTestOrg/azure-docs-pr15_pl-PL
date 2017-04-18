
<properties
    pageTitle="Nigdy nie są przechowywane dane poufne na niestandardowych obrazów dla Azure RemoteApp | Microsoft Azure"
    description="Więcej informacji na temat wytyczne dotyczące przechowywania danych w niestandardowych obrazów w Azure RemoteApp"
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


# <a name="never-store-sensitive-data-on-custom-images"></a>Nigdy nie są przechowywane dane poufne na niestandardowych obrazów

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Jeśli aplikacja w Azure RemoteApp, pierwszym krokiem jest utworzenie obraz niestandardowy. Aby utworzyć wystąpienia maszyn wirtualnych, które służą do użytkowników aplikacji firma Microsoft korzysta z danego niestandardowego obrazu. Obraz niestandardowy powinny zawierać tylko aplikacji i nigdy nie poufne dane, które mogą zostać utracone, takich jak bazy danych programu SQL, plików danych personelu lub pliki danych specjalne, takie jak QuickBooks firmowych plików. Wszystkie dane poufne powinien znajdować się zewnętrznych Azure RemoteApp na serwerze plików innym Głosowa Azure, lub platformy SQL Azure. Obraz tylko powinien obsługiwać aplikacji, która nawiązuje połączenie ze źródłem danych i dane. Aby uzyskać więcej informacji, sprawdź [wymagania dotyczące Azure RemoteApp obrazy](remoteapp-imagereqs.md) . 

Aby dowiedzieć się, dlaczego nie należy przechowywać poufne dane, należy zrozumieć, jak działa funkcja RemoteApp platformy Azure. Podczas tworzenia lub aktualizowania w tle, wielu klonów lub kopii obrazu zbioru są tworzone. Wszystkie wystąpienia maszyn wirtualnych są dokładnie replikami niestandardowego obrazu; gdy użytkownicy uruchamiania aplikacji są one połączone z jedną z tych wystąpień maszyn wirtualnych. Ale tego samego wystąpienia nie jest gwarantowana i czy nie ma znaczenia, ponieważ są one nietrwałe. Wystąpienia maszyn wirtualnych hostingu aplikacje są nietrwałe i mogą być zniszczone lub usunięte, opartych na przykład podczas aktualizacji zbioru. 

Zainicjowano obsługę administracyjną kolekcji i użytkownicy rozpoczną łączenie się maszyny wirtualne, dane użytkownika po trwałych chronione, ponieważ zostanie on zapisany w osobnym ilość miejsca do magazynowania w ramach wirtualnego dysku twardego czy nazywamy [dysku profilu użytkownika (UDP)](remoteapp-upd.md), czyli profilu użytkownika w c:\users\<userprofile >. Po uruchomieniu aplikacji UDP jest zainstalowany i traktowane podobnie jak lokalny profil użytkownika przez system operacyjny. Przeczytaj więcej o sposobie [Azure RemoteApp umożliwia zapisanie danych i ustawień użytkownika](remoteapp-upd.md).

Przykładowe dane, które nie powinny znajdować się w obrazu:

- Danych dla użytkowników uzyskać dostęp do udostępnionych
- Bazy danych SQL lub DB programu QuickBooks
- Wszystkie dane w D:\

Przykładowe dane, które mogą znajdować się w profilu domyślnego, można skopiować do UDP wszystkich użytkowników:

- Pliki konfiguracji dla poszczególnych użytkowników
- Skryptów, czy potrzebne użytkownikom zachowywane w ich UDP

Najważniejsze zagadnienia:

- Nigdy nie sklepu poufne dane, które mogą zostać utracone na obrazie podczas tworzenia niestandardowego obrazu.
- Poufne dane należy zawsze znajdują się na serwerze osobny plik, oddzielnych Głosowa Azure, w chmurze i na zewnątrz zawsze wystąpienia maszyn wirtualnych hostingu aplikacji w Azure RemoteApp. 
- Dane użytkownika są zapisywane i będzie nadal występował na dysku profilu użytkownika (UDP)


