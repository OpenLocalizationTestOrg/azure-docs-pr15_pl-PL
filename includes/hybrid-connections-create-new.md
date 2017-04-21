
1. Na komputerze lokalnym Zaloguj się do [Portalu zarządzania Azure](http://manager.windowsazure.com) (jest to portalu stare).

2. U dołu okienka nawigacji, wybierz pozycję **+ Nowy** > **Aplikacji usług** > **Usługi BizTalk** > **Utworzyć niestandardowe**.

3. Podaj **Nazwę usługi BizTalk** i wybierz pozycję **Edition**. 

    Samouczku **mobile1**. Należy podać unikatową nazwę nowej usługi BizTalk.

4. Po utworzeniu usługę BizTalk wybierz kartę **Hybrydowych połączenia** , a następnie kliknij przycisk **Dodaj**.

    ![Dodawanie połączenia hybrydowego](./media/hybrid-connections-create-new/3.png)

    Spowoduje to utworzenie nowego połączenia hybrydowych.

5. Podaj **nazwę** i **Nazwa hosta** dla połączenia hybrydowych i ustawianie **portu** `1433`. 
  
    ![Konfigurowanie połączenia hybrydowego](./media/hybrid-connections-create-new/4.png)

    Nazwa hosta jest nazwą lokalnego serwera. Pozwala tak skonfigurować połączenie hybrydowego dostępu SQL Server uruchomionym na porcie 1433. Jeśli korzystasz z nazwane wystąpienie programu SQL Server, użyj zamiast tego portu statycznego, który wcześniej zdefiniowano.

6. Po utworzeniu nowego połączenia, stan nowego połączenia pokazuje **lokalnej instalacji niekompletna**.

7. Przejść z powrotem do urządzeń przenośnych usługi, kliknij przycisk **Konfiguruj**, przewiń w dół do **hybrydowego połączenia** kliknij pozycję **Dodaj połączenie hybrydowe**, wybierz połączenie hybrydowe, która została właśnie utworzona i kliknij **przycisk OK**.

    Dzięki temu usługi mobile umożliwia nowe połączenie hybrydowych.

Następnie musisz zainstalować hybrydowych Connection Manager na komputerze lokalnym.