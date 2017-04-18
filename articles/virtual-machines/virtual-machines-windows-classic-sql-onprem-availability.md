<properties
    pageTitle="Rozszerzanie lokalnego zawsze na grupy dostępności Azure | Microsoft Azure"
    description="Ten samouczek używa zasobów utworzone za pomocą modelu Klasyczny wdrożenia i opisano, jak za pomocą kreatora Dodawanie replice w SQL Server Management Studio (SSMS) dodawanie replice zawsze na grupy dostępności w Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>Rozszerzanie lokalnego zawsze na grupy dostępności Azure

Zawsze na grupy dostępności zapewniają wysoką dostępność dla grup bazy danych przez dodanie repliki pomocniczą. Zezwalanie na te repliki awarii między bazami danych w przypadku awarii. Ponadto można można używać odciążania obciążenia odczytu lub zadania kopii zapasowej.

Grupy dostępności lokalnego można rozszerzyć Microsoft Azure inicjowania obsługi administracyjnej jeden lub więcej maszyny wirtualne Azure z programem SQL Server, a następnie dodając ich jako repliki grupom dostępność lokalnego.

Tego samouczka przyjęto założenie, że masz następujące czynności:

- Aktywną subskrypcję Azure. Możesz [utworzyć konto w bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

- Istniejącej zawsze na dostępność grupy lokalnej. Aby uzyskać więcej informacji na grupy dostępności zobacz [Zawsze na grup dostępności](https://msdn.microsoft.com/library/hh510230.aspx).

- Łączność między sieci lokalnej i Azure wirtualnej sieci. Aby uzyskać więcej informacji o tworzeniu ten wirtualną sieć Zobacz [Konfigurowanie sieci VPN witryny do witryny, w portalu klasyczny Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="add-azure-replica-wizard"></a>Dodawanie kreatora replice Azure

W tej sekcji pokazano, jak za pomocą **Kreatora dodawania replice Azure** rozszerzenie rozwiązania zawsze na grupy dostępności do uwzględnienia repliki Azure.

1. Z poziomu programu SQL Server Management Studio rozwiń **Zawsze na szybkiej** > **Grupy dostępności** > **[Nazwa grupy dostępność]**.

1. Kliknij prawym przyciskiem myszy **Repliki dostępność**, a następnie kliknij pozycję **Dodaj replice**.

1. Domyślnie zostanie wyświetlona **Dodawanie replice do kreatora grupy dostępności** . Kliknij przycisk **Dalej**.  Zaznaczenie **Nie pokazuj tego ponownie strony** opcji u dołu strony podczas poprzedniego uruchomienia tego kreatora na tym ekranie nie będą wyświetlane.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)

1. Trzeba będzie nawiązywania połączenia z wszystkich istniejących replik pomocniczą. Możesz kliknąć **Nawiązywanie połączenia...** obok każdej replice lub kliknąć przycisk **Połącz wszystko...** u dołu ekranu. Po uwierzytelnieniu kliknij przycisk **Dalej** , aby przejść do następnego ekranu.

1. Na stronie **Określanie repliki** wiele kart znajdują się u góry: **repliki**, **punkty końcowe** **Preferencje kopii zapasowej**i **odbiornika**. Na karcie **repliki** kliknij pozycję **Dodaj Azure replice...** Aby uruchomić Kreatora dodawania replice Azure.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)

1. Wybierz istniejący certyfikat zarządzania Azure z lokalnego magazynu certyfikatów systemu Windows, jeśli zainstalowano przed. Wybierz lub wprowadź identyfikator subskrypcji usługi Azure, jeśli użyto przed. Można kliknąć przycisk Pobierz, aby pobrać i zainstalować certyfikat zarządzania Azure i pobrać listę subskrypcji przy użyciu konta usługi Azure.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)

1. Będą wypełniać każdego pola, na stronie z wartościami, które będzie używane do tworzenia Azure maszyn wirtualnych (maszyn wirtualnych) którym będzie przechowywana replice.

  	|Ustawienie|Opis|
|---|---|
|**Obraz**|Wybierz żądaną kombinację systemu operacyjnego i SQL Server|
|**Rozmiar pamięci Wirtualnej**|Wybierz odpowiedni rozmiar maszyn wirtualnych, który najlepiej pasuje do potrzeb biznesowych|
|**Nazwa maszyn wirtualnych**|Określ unikatową nazwę dla nowego maszyn wirtualnych. Nazwa musi zawierać od 3 do 15 znaków, można zawierają tylko litery, cyfry i łączniki, musi rozpoczynać się literą i kończą się literą lub cyfrą.|
|**Nazwa_użytkownika maszyn wirtualnych**|Określ nazwę użytkownika, który ma stać się do konta administratora na maszyn wirtualnych|
|**Hasło administratora maszyn wirtualnych**|Określ hasło dla nowego konta|
|**Potwierdź hasło**|Potwierdź hasło nowego konta|
|**Wirtualna sieć**|Określ Azure wirtualnej sieci, który ma być używany w nowych maszyn wirtualnych. Aby uzyskać więcej informacji na wirtualnych sieci zobacz [Omówienie wirtualnych sieci](../virtual-network/virtual-networks-overview.md).|
|**Wirtualna podsieci**|Określić podsieć wirtualnej sieci, który ma być używany w nowych maszyn wirtualnych|
|**Domeny**|Upewnij się, że wartość wstępnie wypełnione dla domeny jest poprawny|
|**Nazwa użytkownika domeny**|Określanie konta, które znajduje się w lokalnej grupy administratorów na lokalnym węzłach|
|**Hasło**|Określ hasło dla nazwy użytkownika domeny|

1. Kliknij **przycisk OK** , aby sprawdzić poprawność ustawień rozmieszczania.

1. Warunki prawne są wyświetlane obok. Czytanie i kliknij **przycisk OK** , jeśli użytkownik zgadza się tych terminów.

1. Ponownie zostanie wyświetlona strona **Określ repliki** . Sprawdź ustawienia nowego replice Azure na kartach **repliki**, **punkty końcowe**i **Preferencje kopii zapasowej** . Modyfikowanie ustawień zgodnie z potrzebami firm.  Aby uzyskać więcej informacji o parametrach znajdujących się na następujących kartach zobacz [Określ strony repliki (dostępność Kreatora nowej grupy kreatora i Dodaj replice)](https://msdn.microsoft.com/library/hh213088.aspx). Należy zauważyć, że detektory nie można utworzyć za pomocą karty odbiornika dla grupy dostępności, które zawierają repliki Azure. Ponadto jeśli utworzono już detektor przed uruchomieniem kreatora, otrzymasz komunikat z informacją, że nie jest obsługiwana w Azure. Firma Microsoft będzie wyglądać w sposób tworzenia detektory w sekcji **Tworzenie detektor grupy dostępności** .

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)

1. Kliknij przycisk **Dalej**.

1. Wybierz metodę synchronizacji danych, których chcesz użyć na stronie **Wybierz pozycję początkowej synchronizacji danych** , a następnie kliknij przycisk **Dalej**. W przypadku większości scenariuszy zaznacz **Pełnej synchronizacji danych**. Więcej informacji na temat metod synchronizacji danych zobacz [Wybieranie danych synchronizacji strony początkowej (zawsze na dostępność grupy kreatorzy)](https://msdn.microsoft.com/library/hh231021.aspx).

1. Przejrzyj wyniki na stronie **Sprawdzanie poprawności** . Poprawianie zagadnienia i ponownie uruchom sprawdzanie poprawności, w razie potrzeby. Kliknij przycisk **Dalej**.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)

1. Przejrzyj ustawienia na stronie **Podsumowanie** , a następnie kliknij przycisk **Zakończ**.

1. Rozpocznie się proces inicjowania obsługi administracyjnej. Gdy Kreator zakończy się pomyślnie, kliknij przycisk **Zamknij** zamkniesz Kreatora.

>[AZURE.NOTE] Kreatora dodawania replice Azure tworzy plik dziennika, w Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Ten plik dziennika może służyć do Rozwiązywanie problemów z wdrożeń replice Azure nie powiodło się. Jeśli Kreator nie działa, wykonywanie wszelkich działań, wszystkie operacje poprzedniego są przywracane, łącznie z usuwanie ustanawianie maszyn wirtualnych.

## <a name="create-an-availability-group-listener"></a>Tworzenie detektor grupy dostępności

Po utworzeniu grupy dostępności, należy utworzyć detektor dla klientów nawiązać połączenie repliki. Detektory bezpośrednie połączeń przychodzących do podstawowej lub pomocniczej replice tylko do odczytu. Aby uzyskać więcej informacji na detektory zobacz [Konfigurowanie detektor ILB zawsze na dostępność grup w Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

## <a name="next-steps"></a>Następne kroki

Oprócz korzystania z **Kreatora dodawania replice Azure** rozszerzenie zawsze na dostępność grupy Azure, to może również przenieść niektóre obciążenia programu SQL Server całkowicie Azure. Aby rozpocząć pracę, zobacz [inicjowania obsługi administracyjnej maszyny wirtualnej serwera SQL Azure](virtual-machines-windows-portal-sql-server-provision.md).

Aby uzyskać innych tematów związanych z programem SQL Server w maszyny wirtualne Azure zobacz [Programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).
