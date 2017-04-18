## <a name="create-an-event-hub"></a>Tworzenie Centrum zdarzenia

1. Zaloguj się do [portalu Azure][], a następnie kliknij przycisk **Nowy** w górnej części ekranu w lewo.

2. Kliknij pozycję **dane + analizy**, a następnie kliknij **Zdarzenie koncentratory**.

    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)

3. W karta **Tworzenie nazw** wprowadź nazwę nazw. System natychmiast sprawdza, czy nazwa jest dostępna.

    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)

4. Po upewnić się, że nazwa obszaru nazw jest dostępna, wybierz pozycję warstwie cennik (Basic lub standardowe). Ponadto wybierz Azure subskrypcji, grupa zasobów i lokalizacji, w której chcesz utworzyć zasób. 

2. Kliknij przycisk **Utwórz** , aby utworzyć obszar nazw.

6. Na liście nazw koncentratory zdarzenia kliknij nazw nowo utworzone.      

    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)

7. Karta nazw kliknij **Koncentratory zdarzenia**.

    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)

8. U góry karta kliknij pozycję **Dodaj Centrum zdarzenia**.

    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)

3. Wpisz nazwę swojego Centrum zdarzenia, a następnie kliknij przycisk **Utwórz**.

    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)

4. Na liście koncentratory zdarzenia kliknij nazwę nowo utworzonego Centrum zdarzenia. 

    ![](./media/event-hubs-create-event-hub/create-event-hub6.png)

5. Po powrocie do nazw karta (nie określonych karta Centrum zdarzenia) kliknij pozycję **udostępnione zasady dostępu**, a następnie kliknij **RootManageSharedAccessKey**.

    ![](./media/event-hubs-create-event-hub/create-event-hub7.png)

5. Kliknij przycisk Kopiuj, aby skopiować parametry połączenia **RootManageSharedAccessKey** do Schowka. Zapisz ten ciąg połączenia, aby użyć w dalszej części samouczka.

    ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

Twoim Centrum zdarzenie jest tworzony, a masz parametry połączenia, które są potrzebne do wysyłania i odbierania zdarzeń.

[Azure portal]: https://portal.azure.com/