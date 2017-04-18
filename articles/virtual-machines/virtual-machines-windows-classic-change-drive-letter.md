<properties
    pageTitle="Tworzenie dysku D: maszyny dysku danych | Microsoft Azure"
    description="W tym artykule opisano, jak zmienić litery dysków dla maszyn wirtualnych systemu Windows, dzięki czemu można używać dysku D: jako dysk z danymi."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Dysków D: jako dysk z danymi na maszyn wirtualnych systemu Windows 

Jeśli aplikacja musi służy do przechowywania danych na dysku D, wykonaj te instrukcje używać różnych literę tymczasowe dysku. Nigdy nie używaj dysku tymczasowym do przechowywania danych, które chcesz zachować.

Jeśli możesz zmienić rozmiar lub **Zatrzymaj (Deallocate)** maszyny wirtualnej, to może pojawić się położenie maszyny wirtualnej do nowego monitor maszyny wirtualnej. W przypadku zdarzeń konserwacji planowana lub niezaplanowane może być również przyczyną tego położenie. W tym scenariuszu dysku tymczasowym zostanie przydzielona do pierwszej litery dysku. Jeśli masz aplikację, która wymaga określonego dysku D:, musisz wykonaj poniższe czynności, aby tymczasowo przenieść pagefile.sys, dołączyć nowego dysku danych i przypisać go litera D i następnie z powrotem przenieść pagefile.sys tymczasowe dysk. Po zakończeniu Azure nie zajmie ponownie D: Jeśli maszyn wirtualnych przesunie się do różnych monitor maszyny wirtualnej.

Aby uzyskać więcej informacji o używaniu Azure na dysku tymczasowym zobacz [Opis tymczasowe dysk na maszyn wirtualnych Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="attach-the-data-disk"></a>Dołącz dysk danych

Najpierw musisz dołączyć dysku danych maszyn wirtualnych. 

- Aby korzystać z portalu, zobacz [jak dołączyć dysku danych w portalu Azure](virtual-machines-windows-attach-disk-portal.md)
- Aby korzystać z portalu klasyczny, zobacz [sposoby dołączania danych dysku maszyn wirtualnych systemu Windows](virtual-machines-windows-classic-attach-disk.md). 


## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Tymczasowo przenieść pagefile.sys na dysk C

1. Połącz maszyn wirtualnych. 

2. Kliknij prawym przyciskiem myszy **Start** menu i wybierz pozycję **System**.

3. W menu po lewej stronie wybierz pozycję **Zaawansowane ustawienia systemu**.

4. W sekcji **Wydajność** wybierz pozycję **Ustawienia**.

5. Wybierz kartę **Zaawansowane** .

5. W sekcji **pamięć wirtualna** wybierz pozycję **Zmień**.

6. Wybierz dysk **C** , a następnie kliknij pozycję **System zarządzany rozmiar** , a następnie kliknij przycisk **Ustaw**.

7. Zaznacz na dysku **D** , a następnie kliknij **plik stronicowania nie** i kliknij przycisk **Ustaw**.

8. Kliknij przycisk Zastosuj. Zostanie wyświetlony ostrzeżenie, że komputer musi należy ponownie uruchomić, aby zmiany zostały wprowadzone.

9. Uruchom ponownie komputer wirtualnych.




## <a name="change-the-drive-letters"></a>Zmiana liter dysku 

1. Po uruchomieniu maszyn wirtualnych Zaloguj maszyn wirtualnych.

2. Kliknij **Start** menu i wpisz **diskmgmt.msc** i naciśnij klawisz Enter. Zarządzanie dysku zostanie uruchomiony.

3. Kliknij prawym przyciskiem myszy **D**, dysk tymczasowego przechowywania i wybierz pozycję **Zmień literę dysku i ścieżki**.

4. W obszarze literę wybierz dysk **G** , a następnie kliknij **przycisk OK**. 

5. Kliknij prawym przyciskiem myszy na dysku danych, a następnie wybierz pozycję **Zmień literę dysku i ścieżki**.

6. W obszarze literę zaznacz dysku **D** , a następnie kliknij **przycisk OK**. 

7. Kliknij prawym przyciskiem myszy **G**, dysk tymczasowego przechowywania i wybierz pozycję **Zmień literę dysku i ścieżki**.

8. W obszarze literę wybierz dysk **E** , a następnie kliknij **przycisk OK**. 

> [AZURE.NOTE] Jeśli do maszyn wirtualnych ma innych dysków lub dyski, należy używać tej samej metody, aby ponownie przydzielić litery dysków i stacji dysków. Chcesz, aby konfiguracja dysku, należy:  
>- C: dysk OS  
>- D: dysk danych  
>- E: tymczasowe dysku



## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Odsunąć pagefile.sys na dysku tymczasowego przechowywania 

1. Kliknij prawym przyciskiem myszy **Start** menu i wybierz pozycję **System**

2. W menu po lewej stronie wybierz pozycję **Zaawansowane ustawienia systemu**.

3. W sekcji **Wydajność** wybierz pozycję **Ustawienia**.

4. Wybierz kartę **Zaawansowane** .

5. W sekcji **pamięć wirtualna** wybierz pozycję **Zmień**.

6. Wybierz dysk OS **C** i kliknij **plik stronicowania nie** , a następnie kliknij przycisk **Ustaw**.

7. Wybierz dysk tymczasowego przechowywania **E** i kliknij pozycję **System zarządzany rozmiar** , a następnie kliknij przycisk **Ustaw**.

8. Kliknij przycisk **Zastosuj**. Zostanie wyświetlony ostrzeżenie, że komputer musi należy ponownie uruchomić, aby zmiany zostały wprowadzone.

9. Uruchom ponownie komputer wirtualnych.




## <a name="next-steps"></a>Następne kroki
- Na komputerze wirtualnych można zwiększyć miejsca do magazynowania, dołączając [dysku dodatkowe dane](virtual-machines-windows-attach-disk-portal.md).



