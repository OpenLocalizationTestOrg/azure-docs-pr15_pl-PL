
1. W karta **hybrydowych połączenia** kliknij połączenie hybrydowych właśnie utworzony, a następnie kliknij przycisk **Ustawienia odbiornika**.
    
    ![Kliknij pozycję Ustawienia odbiornika](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
    
4. Zostanie wyświetlona karta **hybrydowych właściwości połączenia** . W obszarze **Lokalnego hybrydowych Connection Manager**wybierz pozycję **Pobierz i skonfiguruj ręcznie**zapisać pobrany pakiet HybridConnectionManager.msi i skopiuj parametry połączenia bramy.
    
    ![Kliknij tutaj, aby zainstalować](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
    
5. W wierszu polecenia administratora wpisz następujące polecenie, aby uruchomić Instalatora pakietu:

        start HybridConnectionManager.msi
 
7. Po uruchomieniu Instalatora pakietu **nie teraz**, kliknij pozycję, a następnie przejdź do folderu %ProgramFiles%\Microsoft\HybridConnectionManager, uruchom HCMConfigWizard.exe i kliknij przycisk **Tak** w oknie dialogowym **Kontrola konta użytkownika** .
        
7. Wklej hybrydowych parametry połączenia, który został skopiowany wcześniej i kliknij **przycisk OK**. 
    
    ![Instalowanie](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
    
8. Po zakończeniu instalacji kliknij przycisk **Zamknij**.
    
    ![Kliknij przycisk Zamknij](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
    
    Na karta **połączenia hybrydowych** w kolumnie **Stan** zawiera **połączony**. 
    
    ![Stan połączenia](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)