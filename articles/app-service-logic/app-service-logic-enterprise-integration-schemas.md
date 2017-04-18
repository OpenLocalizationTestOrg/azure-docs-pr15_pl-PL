<properties
    pageTitle="Omówienie schematów i pakiet integracji przedsiębiorstwa | Microsoft Azure"
    description="Dowiedz się, jak schematy za pomocą aplikacji Enterprise Integracja z dodatkiem Service Pack i układy logiczne"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>Więcej informacji na temat schematów i pakiet integracji przedsiębiorstwa  

## <a name="why-use-a-schema"></a>Dlaczego warto używać schematu?
Upewnij się, że są prawidłowe, oczekiwanych danych według wstępnie zdefiniowanego formatu dokumentów XML, które otrzymujesz za pomocą schematów. Schematy są używane do sprawdzania poprawności komunikatów wymienianych w scenariuszu B2B.

## <a name="add-a-schema"></a>Dodawanie schematu
Z poziomu portalu Azure:  

1. Wybierz pozycję **więcej usług**.  
![Zrzut ekranu przedstawiający Azure portalu usługami"więcej" wyróżnione](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. W polu wyszukiwania filtru wprowadź **integracji**, a następnie wybierz pozycję **Konta integracji** z listy wyników.     
![Zrzut ekranu przedstawiający pole wyszukiwania filtru](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Wybierz **konto integracji** , do którego można dodać schematu.    
![Zrzut ekranu przedstawiający listę kont integracji](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. Wybierz Kafelek **schematów** .  
![Zrzut ekranu: IEP integracji konta, z wyróżnionym "schematy"](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>Dodawanie pliku schematu mniej niż 2 MB  

1. W kartę **Schematy** , która zostanie otwarta (z powyższych kroków) wybierz pozycję **Dodaj**.  
![Zrzut ekranu przedstawiający schematy karta, z wyróżnionym przyciskiem "Dodaj"](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. Wprowadź nazwę dla schematu. Następnie aby przekazać plik schematu, wybierz ikonę folderu obok pola tekstowego **schematu** . Po zakończeniu procesu przekazywania, kliknij przycisk **OK**.    
![Zrzut ekranu: "Dodaj schemat", "Małe plik" wyróżnione](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>Dodawanie pliku schematu większych niż 2 MB (maksymalnie 8 MB)  

Proces to zależy od poziomu dostępu kontenera obiektów blob: **publiczna** lub **nie dostęp anonimowy**. Aby określić ten poziom dostępu w **Eksplorator magazynu Azure**, w obszarze **Kontenerów obiektów Blob**, wybierz kontener obiektów blob, który ma. Wybierz pozycję **Zabezpieczenia**, a następnie wybierz kartę **Poziom dostępu** .

1. W przypadku **publicznych**poziom obiektów blob zabezpieczeń programu access, wykonaj następujące kroki.  
  ![Zrzut ekranu przedstawiający Azure Eksplorator magazynu z "Obiektów Blob kontenerów", "Zabezpieczenia" i "Publicznej" wyróżnione](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    . Przekaż schematu do miejsca do magazynowania, a następnie skopiuj identyfikator URI.  
    ![Zrzut ekranu przedstawiający konta miejsca do magazynowania, z wyróżnionym identyfikator URI](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    b. **Dodaj schemat**wybierz **duży plik**, a następnie przekazać go w polu tekstowym **Identyfikator URI zawartości** .  
    ![Zrzut ekranu przedstawiający schematy przy użyciu przycisku "Dodaj" i "Duży plik" wyróżnione](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. Jeśli poziom dostępu zabezpieczeń obiektów blob jest **nie dostęp anonimowy**, wykonaj następujące kroki.  
  ![Zrzut ekranu przedstawiający Azure Eksplorator magazynu z "Blob kontenerów", "Zabezpieczenia" i "Nie dostęp anonimowy" wyróżnione](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    . Przekaż schematu do miejsca do magazynowania.  
    ![Zrzut ekranu przedstawiający konta miejsca do magazynowania](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    b. Generowanie podpisu udostępniania dla schematu.  
    ![Zrzut ekranu przedstawiający sccount miejsca do magazynowania, z wyróżnioną kartą podpisów udostępniania](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c. **Dodaj schemat**wybierz **duży plik**i podaj podpisu udostępnienia URI w polu tekstowym **Identyfikator URI zawartości** .  
    ![Zrzut ekranu przedstawiający schematy przy użyciu przycisku "Dodaj" i "Duży plik" wyróżnione](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. W karta **Schematy** konta integracji element EIP powinien zostać wyświetlony nowo dodany schematu.  
![Zrzut ekranu: element EIP integracji konto z "Schematów" i nowy schemat wyróżnione](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>Edytowanie schematów
1. Wybierz Kafelek **schematów** .  
2. Wybierz schemat, który chcesz edytować kartę **Schematy** , która zostanie otwarta.
3. Na karta **Schematy** kliknij przycisk **Edytuj**.  
![Zrzut ekranu przedstawiający schematy karta](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Wybierz plik schematu, który chcesz edytować, przy użyciu okna dialogowego Wybór pliku, który zostanie otwarty.
5. Wybierz polecenie **Otwórz** w selektorze pliku.  
![Zrzut ekranu przedstawiający selektor plików](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. Otrzymasz powiadomienie, które oznacza, że przekazywanie zakończyło się pomyślnie.  

## <a name="delete-schemas"></a>Usuwanie schematów
1. Wybierz Kafelek **schematów** .  
2. Wybierz schemat, który chcesz usunąć z kartę **Schematy** , która zostanie otwarta.  
3. Na karta **schematów** wybierz pozycję **Usuń**.
![Zrzut ekranu przedstawiający schematy karta](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. Aby potwierdzić wybór, wybierz opcję **Tak**.  
![Zrzut ekranu przedstawiający komunikat potwierdzający "Usuń schematu"](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. Warto zauważyć, że na liście schematów w karta **Schematy** odświeża i schematu usuniętego nie jest już wyświetlana.  
![Zrzut ekranu przedstawiający element EIP integracji konto z "Schematy" wyróżnione](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Następne kroki

- [Dowiedz się więcej o pakiecie integracji przedsiębiorstwa] (./app-service-logic-enterprise-integration-overview.md "Więcej informacji na temat pakietu integracji przedsiębiorstwa").  
