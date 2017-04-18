<properties
   pageTitle="Jak utworzyć bilet pomocy technicznej dla magazynu danych SQL | Microsoft Azure"
   description="Jak utworzyć bilet pomocy technicznej w magazynie danych SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess"/>

# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Jak utworzyć bilet pomocy technicznej dla magazynu danych SQL
 
Jeśli masz problemy z magazynu danych SQL, zobacz utworzysz pomocy technicznej biletów tak, aby nasz zespół inżynierów może być pomocny.

## <a name="create-a-support-ticket"></a>Tworzenie bilet pomocy technicznej

1. Otwórz [Azure portal][].

2. Na ekranie głównym kliknij **Pomoc + pomocy technicznej** .

    ![Pomoc + pomocy technicznej](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)

3. W menu Pomoc + karta pomocy technicznej, kliknij przycisk **Utwórz żądanie pomocy technicznej**.

    ![Nowe żądanie pomocy technicznej](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
    
    <a name="request-quota-change"></a> 

4. Wybierz **Typ żądania**.

    ![Typ żądania](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
    
    >[AZURE.NOTE]  Domyślnie każdy programu SQL server (np. myserver.database.windows.net) ma **Przydziału DTU** 45,000. Przydział jest po prostu limit bezpieczeństwo. Tworząc bilet pomocy technicznej i wybranie *przydziału* jako typ żądania, można zwiększyć przydziału. Do obliczania usługi DTU potrzebom, należy pomnożyć 7.5 przez razem, gdy potrzebne [DWU][] . Na przykład chcesz udostępniać dwóch DW6000s na jednym programu SQL server, a następnie należy zażądać normę DTU 90,000.  Możesz wyświetlać Twojej bieżącej zużycia DTU z karta SQL server w portalu. Zarówno wstrzymanych i usuwając wstrzymanych baz danych są wliczane do przydziału DTU. 

5. Wybierz **subskrypcję** , obsługującym bazę danych, na którym występuje problem, który chcesz zgłosić.

    ![Subskrypcji](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)

6. Wybierz pozycję **magazynu danych SQL** jako zasobu.

    ![Zasób](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)

7. Wybierz pozycję usługi [Azure obsługuje planu][].

    - Pomoc dotycząca **rozliczeń i zarządzania przydziału i subskrypcji** jest dostępna na wszystkich poziomach pomocy technicznej.
    - **Poprawka podziału** są obsługiwane przez [dewelopera][], [Standardowy][], [Professional bezpośrednie][] lub pomocy technicznej [Premier][] . Podział Rozwiąż problemy są problemy podczas korzystania z platformy Azure przez klientów przypadku rozsądne oczekiwania, że Microsoft spowodowało problem.
    - **Deweloper doradztwo** i **usługi doradcze** są dostępne w poziomie pomocy technicznej [Premier][] i [Professional bezpośrednie][] . 
    
    Jeśli masz Premier obsługuje planu, możesz również zgłosić magazynu danych SQL problemy w [portalu online Microsoft Premier][]związane z.  Zobacz [Plany pomocy technicznej Azure][Azure obsługuje plan] , aby dowiedzieć się więcej na temat poszczególnych planów pomocy technicznej, łącznie z zakresu czasu odpowiedzi, ceny, itp.  Często zadawane pytania dotyczące Azure obsługi, zobacz [Azure obsługuje często zadawane pytania][].  

    ![Plan pomocy technicznej](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)

8. Wybierz **Typ problemu** i **kategorii**.

    ![Kategoria typu problemu](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)

9. Opisz problem, a następnie wybierz żądany poziom biznesowych.

    ![Opis problemu](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)

10. **Informacje kontaktowe** dla tego bilet pomocy technicznej zostaną wstępnie wypełnione. Aktualizowanie, jeśli to konieczne.

    ![Informacje kontaktowe](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)

11. Kliknij przycisk **Utwórz** , aby wysłać żądanie pomocy technicznej.


## <a name="monitor-a-support-ticket"></a>Monitorowanie bilet pomocy technicznej

Po zostało przesłane żądanie pomocy technicznej, z obsługą Azure będzie skontaktować się z Tobą. Aby sprawdzić stan żądania i uzyskać szczegółowe informacje, kliknij pozycję **Zarządzaj pomocy technicznej żądania** na pulpicie nawigacyjnym.

![Sprawdzanie stanu](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Inne zasoby

Ponadto można połączyć społeczności magazynu danych SQL na [Przepełnienie stosu][] lub na [forum MSDN magazynu danych SQL Azure][].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Planowanie obsługi Azure]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Deweloper]: https://azure.microsoft.com/support/plans/developer/  
[Standardowe]: https://azure.microsoft.com/support/plans/standard/  
[Profesjonalny bezpośrednich]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Obsługa Azure często zadawane pytania]: https://azure.microsoft.com/support/faq/
[Microsoft Premier portal usługi Lync online]: https://premier.microsoft.com/
[Przepełnienie stosu]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Forum MSDN magazynu danych SQL Azure]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

