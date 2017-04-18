<properties
   pageTitle="Porównanie magazynu Lake Azure danych z obiektów Blob miejsca do magazynowania Azure | Microsoft Azure"
   description="Porównanie magazynu Lake Azure danych z obiektów Blob miejsca do magazynowania Azure"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Porównanie magazynu Lake danych Azure i magazyn obiektów Blob platformy Azure

Tabela w tym artykule podsumowano różnice między magazynu Lake danych Azure i magazyn obiektów Blob platformy Azure wzdłuż kilka kluczowych aspektów duży przetwarzania danych. Magazyn obiektów Blob platformy Azure to uniwersalny magazynu skalowalna obiekt, który jest przeznaczony dla wielu różnych scenariuszach miejsca do magazynowania. Magazyn Lake danych Azure to repozytorium wyraźny przeznaczonego do obciążenia analizy danych duży.

|    | Magazyn Lake danych Azure  | Magazyn obiektów Blob platformy Azure  |
|----|-----------------------|--------------------|
| Cel  | Zoptymalizowane miejsca do magazynowania dla obciążenia analizy danych duży                                                                          | Obiekt uniwersalny przechowywanie wielu różnych scenariuszach miejsca do magazynowania                                                                                |
| Przypadków użycia                                   | Seria, interakcyjnych i przesyłania strumieniowego analizy danych nauki komputera takich jak pliki, IoT danych dziennika, kliknij pozycję strumieni, dużych zestawów danych | Dowolny typ danych tekst lub binarny, takich jak aplikacji kopii zakończenia, danych kopii zapasowej, media miejsca do magazynowania dla danych przeznaczenie przesyłanie strumieniowe i ogólne |
| Podstawowe pojęcia                                | Konto magazynu Lake danych zawiera foldery, które z kolei zawiera dane przechowywane jako pliki | Konto miejsca do magazynowania ma kontenery, która z kolei zawiera dane w postaci obiektów blob |
| Struktura | Hierarchiczne plików systemu Windows                                                                                                    | Magazyn obiektów z prostym nazw  |
| INTERFEJS API   | Interfejsu API usługi REST za pomocą protokołu HTTPS | Interfejsu API usługi REST za pomocą protokołu HTTP/HTTPS                                                                                                                            |
| Interfejs API po stronie serwera                             | [Interfejsu API usługi REST zgodnego z programem WebHDFS](https://msdn.microsoft.com/library/azure/mt693424.aspx)                                                                                                 | [Magazyn obiektów Blob platformy Azure interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dd135733.aspx)                                                                                                                         |
| Plik usługi Hadoop systemu klienta                   | Tak                                                                                                                         | Tak                                                                                                                                                 |
| Operacje na danych — uwierzytelniania            | Oparte na [tożsamości usługi Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md) | Oparte na wspólne hasła — [Udostępnionych klawiszy podpisu dostępu](../storage/storage-dotnet-shared-access-signature-part-1.md)i [Klawisze dostępu do konta](../storage/storage-create-storage-account.md#manage-your-storage-account) .                                                                       |
| Operacje na danych — protokół uwierzytelniania     | OAuth 2.0. Połączenia musi zawierać prawidłową (JSON Web Token) JWT wydany przez usługi Azure Active Directory                     | Kod uwierzytelniania wiadomości oparty na procesie mieszania (HMAC). Połączenia musi zawierać mieszania SHA-256 zakodowany Base64 na części żądania HTTP. |
| Operacje na danych — autoryzacji               | Listy kontroli dostępu POSIX (ACL).  ACL według Azure Active Directory tożsamości można ustawić poziom plików i folderów. | Dla poziomu kont autoryzacji — użyj [Klawiszy dostępu konta](../storage/storage-create-storage-account.md#manage-your-storage-account)<br>Konta, kontenerów i obiektów blob autoryzacji - użyj [Klawiszy podpisu dostępu udostępnione](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| Operacje na danych — inspekcji                    | Dostępne. Zobacz [tutaj](data-lake-store-diagnostic-logs.md) informacji.                                                                                                                   | Dostępne                                                                                                                                           |
| Szyfrowanie danych w stanie spoczynku                     | Przezroczysty, po stronie serwera (już wkrótce)<ul><li>Za pomocą klawiszy zarządzane usługi</li><li>Za pomocą klawiszy zarządzane przez klienta w Azure KeyVault</li></ul>| <ul><li>Przezroczysty, po stronie serwera</li> <ul><li>Za pomocą klawiszy zarządzane usługi</li><li>Za pomocą klawiszy zarządzane przez klienta w KeyVault Azure (już wkrótce)</li></ul><li>Szyfrowania po stronie klienta</li></ul> |
| Operacje zarządzania (np. Tworzenie konta) | [Kontrola dostępu oparta na rolach](../active-directory/role-based-access-control-what-is.md) (RBAC) przewidziane przez Azure Zarządzanie kontami                                                                       | [Kontrola dostępu oparta na rolach](../active-directory/role-based-access-control-what-is.md) (RBAC) przewidziane przez Azure Zarządzanie kontami                                                                                               |
| SDK Deweloper                              | Node.js .NET Java                                                                                                         | .NET, Java, Python, Node.js, C++, dopiskiem                                                                                                              |
| Analiza wydajności obciążenie pracą              | Wydajność zoptymalizowane dla równoległych analizy obciążeń pracą. Wysokiej wydajności i operacji i/o na SEKUNDĘ.                                           | Nie są zoptymalizowane dla analizy obciążeń pracą                                                                                                               |
| Limity rozmiarów                                 | Nie ograniczenia dotyczące konta rozmiary, rozmiar pliku lub liczba plików                                                                   | Ograniczenia określonych udokumentowanych [tutaj](../azure-subscription-service-limits.md#storage-limits)                                                                                                                     |
| Geo nadmiarowości                              | Lokalnie zbędne (wielu kopii danych w jednym regionie Azure)                                                             | Lokalnie zbędne (LRS), globalnie zbędne (GRS), dostęp do odczytu globalnie zbędne (Pomoc Zdalna-GRS). Zobacz [tutaj](../storage/storage-redundancy.md) uzyskać więcej informacji |
| Stan usługi                               | Podgląd publicznej                                                                                                              | Ogólnodostępne                                                                                                                                 |
| Dostępność regionalnych  | Zobacz [tutaj](https://azure.microsoft.com/regions/#services)| Zobacz [tutaj](https://azure.microsoft.com/regions/#services) |
| Cena                                       |     Zobacz [ceny](https://azure.microsoft.com/pricing/details/data-lake-store/)| Zobacz [ceny](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Następne kroki

- [Omówienie magazynu Lake danych Azure](data-lake-store-overview.md)
- [Rozpoczynanie pracy z magazynu Lake danych](data-lake-store-get-started-portal.md)



