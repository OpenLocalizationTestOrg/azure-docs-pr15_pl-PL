<properties
    pageTitle="Tworzenie zestawu dostępność maszyn wirtualnych | Microsoft Azure"
    description="Dowiedz się, jak utworzyć dostępność ustawieniem maszyn wirtualnych za pomocą Azure portal lub przy użyciu Menedżera zasobów modelu wdrożenia programu PowerShell."
    keywords="Konfigurowanie dostępności"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>


# <a name="create-an-availability-set"></a>Tworzenie zestawu dostępności 

Po ustawieniu za pomocą portalu, jeśli chcesz, aby usługi maszyn wirtualnych jako część dostępność potrzebne do utworzenia dostępność zestawu.

Aby uzyskać więcej informacji o tworzeniu i używaniu zestawy dostępności zobacz [Zarządzanie dostępność maszyn wirtualnych](virtual-machines-windows-manage-availability.md).


## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>Dostępność ustawić przed utworzeniem pośrednictwem usługi SMS za pomocą portalu

1. W menu Centrum kliknij przycisk **Przeglądaj** i wybierz pozycję **Ustawia dostępność**.

2. Na **dostępność ustawia karta**kliknij przycisk **Dodaj**.

    ![Zrzut ekranu przedstawiający przycisk Dodaj do tworzenia dostępność nowych Ustaw.](./media/virtual-machines-windows-create-availability-set/add-availability-set.png)

3. Karta **Tworzenie dostępność Ustaw** Wypełnij informacje dotyczące zestawu.

    ![Zrzut ekranu przedstawiający informacje należy wprowadzić Tworzenie zestawu dostępności.](./media/virtual-machines-windows-create-availability-set/create-availability-set.png)

    - **Nazwa** - nazwa powinna być 1 – 80 znaków składa się z liczb, liter, okresów, podkreślenia i kresek. Pierwszy znak musi być literą lub cyfrą. Ostatni znak musi być literę, liczbę lub znak podkreślenia.
    - **Błąd domen** - domen błędów Definiuj grupy maszyn wirtualnych, które współużytkują typowe przełącznika źródłowej i sieci power. Domyślnie maszyny wirtualne są oddzielone domen maksymalnie trzy błędów i może zostać zmieniony między 1 a 3.
    - **Aktualizowanie domen** - pięć aktualizacji domeny są przypisywane domyślnie i to można ustawić przedziału od 1 do 20. Aktualizacja domen wskazują grupy maszyn wirtualnych i źródłowych sprzętu, który należy ponownie uruchomić, w tym samym czasie. Na przykład jeśli firma Microsoft określony pięć aktualizacja domen, gdy więcej niż pięciu maszyn wirtualnych są skonfigurowane w obrębie zestawu pojedynczy dostępność, szóstym maszyny wirtualnej zostaną umieszczone w tej samej domeny aktualizacji jako pierwsza maszyna wirtualna siódmym w tym samym UD jako drugi maszyny wirtualnej i tak dalej. Kolejność ponownego uruchamiania może nie być sekwencyjne, ale tylko jedna aktualizacja domeny zostanie uruchomiony ponownie naraz.
    - **Subskrypcja** — Wybierz subskrypcję, jeśli masz więcej niż jeden.
    - **Grupa zasobów** — wybierz istniejącej grupy zasobów, klikając strzałkę i wybierając grupa zasobów z listy w dół. Można także tworzyć nowej grupy zasobów, wpisując nazwę. Nazwa może zawierać żadnego z następujących znaków: liter, cyfr, kropek, kresek, podkreślenia i otwieranie lub nawias zamykający. Nazwa nie może kończyć się kropką. Wszystkie maszyny wirtualne w grupie dostępność muszą zostać utworzone w tej samej grupy zasobów określonych dostępności.
    - **Lokalizacja** — wybierz lokalizację z listy rozwijanej.

4. Po zakończeniu wprowadzania informacji, kliknij przycisk **Utwórz**. Po utworzeniu grupy dostępności można to sprawdzić na liście po odświeżeniu portalu.

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>Tworzenie maszyny wirtualnej i dostępności, ustaw w tym samym czasie za pomocą portalu

Jeśli tworzysz nowy maszyn wirtualnych za pomocą portalu można także tworzyć dostępność nowych ustawieniem maszyn wirtualnych podczas tworzenia pierwszego maszyn wirtualnych w zestawie.

![Zrzut ekranu przedstawiający proces tworzenia dostępność nowych ustawianie podczas tworzenia maszyn wirtualnych.](./media/virtual-machines-windows-create-availability-set/new-vm-avail-set.png)


## <a name="add-a-new-vm-to-an-existing-availability-set"></a>Dodawanie nowych maszyn wirtualnych do istniejącego zestawu dostępności

Dla każdej dodatkowej Głosowa utworzony której powinna należeć w zestawie upewnij się, czy go utworzyć w tej samej **grupy zasobów** , a następnie wybierz istniejący dostępność w kroku 3. 

![Zrzut ekranu przedstawiający sposób zaznaczyć istniejący dostępność skonfigurowane do korzystania z usługi maszyn wirtualnych.](./media/virtual-machines-windows-create-availability-set/add-vm-to-set.png)



## <a name="use-powershell-to-create-an-availability-set"></a>Używanie programu PowerShell Tworzenie zestawu dostępności

W tym przykładzie tworzy dostępność Ustawianie w grupie zasobów **RMResGroup** w lokalizacji, w **Stanach Zjednoczonych zachód** . To musi zostać wykonane przed utworzeniem pierwszej maszyn wirtualnych, która będzie znajdować się w zestawie.

    New-AzureRmAvailabilitySet -ResourceGroupName "RMResGroup" -Name "AvailabilitySet03" -Location "West US"
    
Aby uzyskać więcej informacji zobacz [AzureRmAvailabilitySet nowy](https://msdn.microsoft.com/library/mt619453.aspx).


## <a name="troubleshooting"></a>Rozwiązywanie problemów

- Po utworzeniu Głosowa, jeśli zestaw dostępności, które mają nie ma na liście rozwijanej w portalu może utworzono go w różnych grupach zasobów. Jeśli nie wiesz, grupa zasobów dla swojej dostępności Ustaw, przejdź do menu Centrum i kliknij przycisk Przeglądaj > Dostępność ustawia Aby wyświetlić listę zestawów dostępność i które należą do grupy zasobów.


## <a name="next-steps"></a>Następne kroki

Dodawanie dodatkowego miejsca do magazynowania do swojego maszyn wirtualnych, dodając dodatkowego [dysku danych](virtual-machines-windows-attach-disk-portal.md).
