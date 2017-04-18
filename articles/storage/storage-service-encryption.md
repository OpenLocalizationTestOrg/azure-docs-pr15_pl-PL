<properties
    pageTitle="Szyfrowanie usługi Azure miejsca do magazynowania danych w pozostałych | Microsoft Azure"
    description="Szyfrowanie magazyn obiektów Blob platformy Azure na stronie usługi przechowywania danych za pomocą funkcji szyfrowanie usługi Azure miejsca do magazynowania i odszyfrować podczas pobierania danych."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="robinsh"/>

# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Szyfrowanie usługi Azure miejsca do magazynowania danych w pozostałych

Azure przestrzeni dyskowej usługi szyfrowania (SSE) danych w pozostałych ułatwia ochronę i ochrony danych w celu spełniają organizacji bezpieczeństwa i zgodności zobowiązań. Za pomocą tej funkcji magazyn Azure są szyfrowane danych przed utrzymuje do magazynu i automatycznie odszyfrowuje przed pobierania. Szyfrowanie, odszyfrowywanie i zarządzania kluczami są całkowicie niewidoczne dla użytkowników.

W poniższych sekcjach przedstawiono, że wystąpi szczegółowe wytyczne dotyczące sposobu używania funkcji szyfrowanie usługi miejsca do magazynowania, jak również obsługiwane scenariusze i użytkownika.

## <a name="overview"></a>Omówienie

Magazyn Azure oferuje pełnego zestawu funkcji zabezpieczeń, umożliwiające programistom na tworzenie bezpiecznych aplikacji. Dane mogą być chronione podczas przesyłania między aplikacją i Azure za pomocą [Szyfrowania po stronie klienta](storage-client-side-encryption.md), HTTPs lub SMB 3.0. Szyfrowanie usługi Magazyn zawiera szyfrowania spoczynku, obsługa szyfrowania, odszyfrowywania i zarządzania kluczami w sposób całkowicie przezroczysty. Wszystkie dane są szyfrowane za pomocą 256-bitowego [szyfrowania AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)jednego najwyższy blok, szyfrowania dostępne.

SSE polega na szyfrowaniu danych, gdy jest zapisywany magazyn Azure i może być używany do obiektów blob blok, blob strony i dołączanie obiektów blob. Sprawdza się następujące rzeczy:

-   Uniwersalny miejsca do magazynowania kont i magazyn obiektów Blob
-   Magazyn standardowy i Magazyn Premium 
-   Nadmiarowości wszystkie poziomy (LRS, ZRS, GRS, GRS pomoc Zdalna)
-   Azure magazynowania Menedżera zasobów konta (ale nie klasyczny) 
-   Wszystkie regiony

Aby włączyć lub wyłączyć szyfrowanie usługi miejsca do magazynowania dla konta miejsca do magazynowania, zaloguj się do [portalu Azure](https://azure.portal.com) i wybierz konto miejsca do magazynowania. Na karta Ustawienia poszukaj sekcji obiektów Blob usługi, jak pokazano na tym zrzucie ekranu, a następnie kliknij pozycję szyfrowania.

![Portal zrzut ekranu przedstawiający opcję szyfrowania](./media/storage-service-encryption/image1.png)

Po kliknięciu przycisku Ustawienia szyfrowania można włączyć lub wyłączyć szyfrowanie usługi miejsca do magazynowania.

![Właściwości szyfrowania portalu zrzut ekranu przedstawiający](./media/storage-service-encryption/image2.png)

##<a name="encryption-scenarios"></a>Scenariusze szyfrowania

Szyfrowanie usługi przechowywania może być włączona na poziomie konta miejsca do magazynowania. Obsługuje następujących scenariuszy:

-   Szyfrowanie blob blok dołączyć obiektów blob i obiektów blob strony.

-   Szyfrowanie zarchiwizowane wirtualnych dysków twardych i szablony pozyskano Azure ze źródeł lokalnych.

-   Szyfrowanie odpowiednie dyski systemu operacyjnego i danych IaaS maszyny wirtualne utworzony przy użyciu usługi wirtualnych dysków twardych.

SSE występują następujące ograniczenia:

-   Szyfrowanie klasyczny magazynu kont nie jest obsługiwane.

-   Szyfrowanie klasyczny magazynu, który konta zmigrowane do Menedżera zasobów magazynowania kont nie jest obsługiwane.

-   Istniejące dane - SSE tylko dane są szyfrowane nowo utworzonych po włączeniu szyfrowania. Jeśli na przykład można utworzyć nowe konto miejsca do magazynowania Menedżera zasobów, ale nie włączysz szyfrowania, a następnie przekazywanie obiektów blob lub zarchiwizowane wirtualnych dysków twardych do tego konta miejsca do magazynowania, a następnie włącz SSE, tych obiektów blob nie będą szyfrowane, chyba że ich napisać od nowa lub skopiowania.

-   Obsługa Marketplace — Włącz szyfrowanie maszyny wirtualne utworzone na podstawie Marketplace przy użyciu [Azure portal](https://portal.azure.com), programu PowerShell i polecenie Azure. Obraz podstawowy wirtualnego dysku twardego pozostanie niezaszyfrowanym; będą jednak szyfrowane wszystkie zapisy wykonać po maszyn wirtualnych występują surowa w górę.

-   Tabela, kolejki, a pliki danych nie będą szyfrowane.

##<a name="getting-started"></a>Wprowadzenie

###<a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Krok 1: [Utwórz nowe konto miejsca do magazynowania](storage-create-storage-account.md).

###<a name="step-2-enable-encryption"></a>Krok 2: Włączanie szyfrowania.

Możesz włączyć szyfrowania za pomocą [Azure portal](https://portal.azure.com).

> [AZURE.NOTE] Jeśli chcesz programowo włączyć lub wyłączyć szyfrowanie usługi miejsca do magazynowania na koncie miejsca do magazynowania, można użyć [Interfejsu API usługi REST dostawcy Azure miejsca do magazynowania zasobów](https://msdn.microsoft.com/library/azure/mt163683.aspx), [Biblioteka klienta dostawcy zasobów miejsca do magazynowania dla środowiska .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure programu PowerShell](../powershell-install-configure.md)lub [Interfejsu wiersza polecenia Azure](storage-azure-cli.md).

###<a name="step-3-copy-data-to-storage-account"></a>Krok 3: Kopiowanie danych do konta miejsca do magazynowania

Jeśli Włącz SSE na koncie miejsca do magazynowania, a następnie wpisz obiektów blob do tego konta magazynu obiektów blob będą szyfrowane. Wszelkie blob już znajdują się w tym kontem miejsca do magazynowania nie będą szyfrowane, dopóki nie zostaną napisać od nowa. Można skopiować dane z jednego miejsca do magazynowania konta do jednego z SSE zaszyfrowane, lub nawet włączyć SSE i kopiowanie obiektów blob z jeden kontener do innego do upewnić się, że poprzednie dane są szyfrowane. Za pomocą dowolnej z następujących narzędzi programu w tym celu.

#### <a name="using-azcopy"></a>Przy użyciu AzCopy

AzCopy jest przeznaczona do kopiowania danych z magazynu obiektów Blob platformy Azure firmy Microsoft, plików i tabeli za pomocą prostych poleceń z optymalnej wydajności narzędzie wiersza polecenia systemu Windows. To umożliwia kopiowanie z obiektami blob z jednego miejsca do magazynowania konta do innego, w którym ma SSE włączone. 

Aby uzyskać więcej informacji, odwiedź stronę [Transfer danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md).

#### <a name="using-the-storage-client-libraries"></a>Za pomocą bibliotek klienta miejsca do magazynowania

Możesz skopiować obiektów blob danych do i z magazynem obiektów blob lub między kontami miejsca do magazynowania przy użyciu naszych bogatego zestawu funkcji bibliotek klienta miejsca do magazynowania w tym .NET, C++, Java, Android, Node.js, PHP, Python i dopiskiem.

Aby dowiedzieć się więcej, odwiedź stronę naszych [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Za pomocą Eksploratora magazynu

Eksplorator magazynu umożliwia tworzenie kont miejsca do magazynowania, przekazywanie i pobieranie danych, wyświetlania zawartości obiektów blob i poruszanie się po katalogów. Aby przekazać obiektów blob do swojego konta miejsca do magazynowania z szyfrowanie włączone, możesz używać jedną z następujących. Przy niektórych eksploratorów miejsca do magazynowania możesz również skopiować dane z istniejącego magazyn obiektów blob z innym kontenerem konta miejsca do magazynowania lub nowego konta miejsca do magazynowania, które ma SSE włączone.

Aby dowiedzieć się więcej, odwiedź stronę [Azure eksploratorów miejsca do magazynowania](storage-explorers.md).

###<a name="step-4-query-the-status-of-the-encrypted-data"></a>Krok 4: Kwerendy stanu zaszyfrowane dane

Wdrożeniu zaktualizowanej wersji bibliotek klienta miejsca do magazynowania, umożliwiające zbadać stanu obiektu, aby określić, jeśli jest zaszyfrowany lub nie. Przykłady zostaną dodane do tego dokumentu w najbliższej przyszłości.

W międzyczasie możesz zadzwonić do [Uzyskaj właściwości konta](https://msdn.microsoft.com/library/azure/mt163553.aspx) , aby sprawdzić, czy konto miejsca do magazynowania szyfrowanie włączone lub wyświetlanie właściwości konta miejsca do magazynowania w portalu Azure.

##<a name="encryption-and-decryption-workflow"></a>Szyfrowanie i odszyfrowywanie przepływu pracy

Oto krótki opis Szyfrowanie/odszyfrowywanie przepływu pracy:

-   Klient umożliwia szyfrowania na koncie miejsca do magazynowania.

-   Gdy klient zapisuje nowe dane (umieszczanie obiektów Blob, umieścić blok, umieść strony itp.) magazyn obiektów Blob; co zapisu jest zaszyfrowany przy użyciu 256-bitowego [szyfrowania AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)jednego najwyższy blok, szyfrowania dostępne.

-   Nabywca musi uzyskać dostęp do danych (Uzyskiwanie obiektów Blob itp.), dane są automatycznie odszyfrowane cmd. użytkownika.

-   Jeśli szyfrowanie jest wyłączone, nowe zapisy już nie są szyfrowane i zaszyfrowane dane pozostaną zaszyfrowane aż ulegną przez użytkownika. Po włączeniu szyfrowania, zapisy magazynem obiektów Blob będą szyfrowane. Stan danych nie zmienia się z danym użytkownikiem przełączanie Włączanie i wyłączanie szyfrowania dla konta miejsca do magazynowania.

-   Wszystkie klucze szyfrowania są przechowywane, szyfrowane i zarządzane przez firmę Microsoft.

##<a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Często zadawane pytania dotyczące szyfrowanie usługi przechowywania danych w pozostałych

**P: masz istniejącego konta klasyczny miejsca do magazynowania. Nad nim można włączyć SSE?**

O: nie SSE jest obsługiwana tylko na kontach miejsca do magazynowania Menedżera zasobów.

**P: jak szyfrowanie danych na moim koncie klasyczny miejsca do magazynowania**

O: można utworzyć nowe konto miejsca do magazynowania Menedżera zasobów i kopiowanie danych przy użyciu [AzCopy](storage-use-azcopy.md) z istniejącego konta klasyczny miejsca do magazynowania do nowo utworzonego konta magazynu Menedżera zasobów. 

Innym rozwiązaniem jest migracji konta klasyczny miejsca do magazynowania do zasobu Zarządzaj kontem miejsca do magazynowania. Aby uzyskać więcej informacji zobacz [Platformy obsługiwane migracji z IaaS zasoby z klasycznym do Menedżera zasobów](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/).

**P: masz istniejącego konta magazynu Menedżera zasobów. Nad nim można włączyć SSE?**

O: tak, ale tylko nowo napisane obiektów blob będą szyfrowane. Nie Przejdź wstecz i szyfrowania danych, który jest już. 

**P: Chcę szyfrowanie bieżące dane znajdujące się istniejącego konta magazynu Menedżera zasobów?**

O: można włączyć SSE w dowolnym momencie na koncie miejsca do magazynowania Menedżera zasobów. Jednak obiektów blob, które były już zainstalowane nie będą szyfrowane. Szyfrowanie tych obiektów blob, można je skopiować do innego kontenera lub inną nazwę, a następnie usuń niezaszyfrowanym wersji.

**P: korzystam z magazynu Premium; Czy mogę używać SSE?**

O: tak, SSE jest obsługiwane na standardowy miejsca do magazynowania i Premium miejsca do magazynowania.

**P: czy można utworzyć nowe konto miejsca do magazynowania i Włącz SSE, a następnie tworzenie nowych maszyn wirtualnych przy użyciu tego konta miejsca do magazynowania, czy to oznacza, że Moja maszyn wirtualnych są szyfrowane?**

O: tak. Wszystkie dyski utworzone, które korzystają z nowego konta miejsca do magazynowania będą zaszyfrowane, jak są tworzone po włączeniu SSE. W przypadku maszyn wirtualnych został utworzony przy użyciu miejsce rynku Azure, podstawowego obrazu wirtualnego dysku twardego pozostanie niezaszyfrowanym; będą jednak szyfrowane wszystkie zapisy wykonać po maszyn wirtualnych występują surowa w górę.

**P: czy można utworzyć nowe konta miejsca do magazynowania z SSE włączone przy użyciu programu PowerShell Azure i polecenie Azure?**

O: tak.

**P: jak znacznie więcej miejsca do magazynowania Azure koszt po włączeniu SSE?**

O: nie jest bez dodatkowych opłat.

**P: kto zarządza kluczy szyfrowania?**

O: klawiszy są zarządzane przez firmę Microsoft.

**P: czy mogę używać klawisze szyfrowania**

O: Pracujemy obecnie nad zapewnia możliwości dla klientów wyświetlić kluczy szyfrowania.

**P: czy mogę odwołać dostęp do kluczy szyfrowania?**

O: nie w tej chwili; klucze są w pełni zarządzane przez firmę Microsoft.

**P: czy SSE domyślnie podczas tworzenia nowego konta miejsca do magazynowania?**

O: SSE nie jest włączona domyślnie; Azure portal umożliwia ją włączyć. Można również programowo włączyć tę funkcję, za pomocą interfejsu API usługi REST miejsca do magazynowania zasobu dostawcy.

**P: jak różni się od szyfrowania dysków Azure?**

O: Ta funkcja służy do szyfrowania danych w magazynie obiektów Blob platformy Azure. Szyfrowanie dysku Azure jest używane do szyfrowania dysków systemu operacyjnego i danych w IaaS maszyny wirtualne. Aby uzyskać więcej informacji odwiedź stronę naszego [Przewodnika zabezpieczeń miejsca do magazynowania](storage-security-guide.md).

**P: co się stanie, jeśli I Włącz SSE, a następnie przejdź w i szyfrowania dysku Azure na dyskach?**

O: to działa bezproblemowo. Dane będą szyfrowane za pomocą obu metod.

**P: Moje konto miejsca do magazynowania ustawiono można replikować geo nadmiarowości. Jeśli włączeniu SSE będą moją kopię zbędne również szyfrowane?**

O: tak, są szyfrowane wszystkie kopie konta miejsca do magazynowania, a wszystkie opcje nadmiarowości — lokalnie zbędne przestrzeni dyskowej (LRS), nadmiarowe strefy przestrzeni dyskowej (ZRS) nadmiarowe Geo przestrzeni dyskowej (GRS) i Odczyt nadmiarowe Geo miejsca do magazynowania (Pomoc Zdalna GRS) — są obsługiwane.

**P: nie można włączyć szyfrowania z mojego konta miejsca do magazynowania.**

O: jest konto Menedżera zasobów miejsca do magazynowania? Klasyczny magazynowania konta nie są obsługiwane. 

**P: czy SSE dozwolone tylko w określonych regionach?**

O: SSE jest dostępna we wszystkich regionach. 

**P: jak mogę zasięgnąć czy jakichkolwiek problemów lub chcesz przesłać opinię?**

O: Skontaktuj się [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) na temat wszelkich problemów związanych z magazynu usługi szyfrowania.

##<a name="next-steps"></a>Następne kroki

Magazyn Azure oferuje pełnego zestawu funkcji zabezpieczeń, umożliwiające programistom na tworzenie bezpiecznych aplikacji. Aby uzyskać więcej informacji odwiedź stronę [Przewodnik zabezpieczeń miejsca do magazynowania](storage-security-guide.md).
