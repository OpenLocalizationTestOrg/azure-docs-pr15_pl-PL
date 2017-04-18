### <a name="prerequisites"></a>Wymagania wstępne
- Konto Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)
- [Magazyn obiektów Blob platformy Azure konta](../articles/storage/storage-create-storage-account.md) w tym nazwę konta magazynu i jego klawisz dostępu. Te informacje znajduje się w oknie dialogowym właściwości konta miejsca do magazynowania w portalu Azure. Dowiedz się więcej o [Magazyn Azure](../articles/storage/storage-introduction.md).

Przed użyciem konta magazyn obiektów Blob platformy Azure w aplikacji logiczny, łączenia się z kontem magazyn obiektów Blob platformy Azure. Możesz to zrobić łatwo w logiczny aplikacji portalu Azure.  

Nawiązywania połączenia z kontem magazyn obiektów Blob platformy Azure, wykonując następujące czynności:  

1. Tworzenie aplikacji logicznych. W Projektancie aplikacji logika dodać wyzwalacz, a następnie dodaj akcję. Wybierz pozycję **Pokaż Microsoft zarządzane interfejsy API** na liście rozwijanej, a następnie wprowadź "obiektów blob" w polu wyszukiwania. Wybierz jedną z czynności:  

    ![Azure kroku tworzenia połączenia magazyn obiektów Blob](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  

2. Jeśli nie utworzono jeszcze wszystkie połączenia z magazynem Azure, zostanie wyświetlony monit o szczegóły połączenia:   

    ![Azure kroku tworzenia połączenia magazyn obiektów Blob](./media/connectors-create-api-azureblobstorage/connection-details.png)  

3. Wprowadź szczegółowe informacje o koncie miejsca do magazynowania. Właściwości gwiazdką są wymagane.

    | Właściwość | Szczegóły |
|---|---|
| Nazwa połączenia * | Wpisz dowolną nazwę połączenia. |
| Nazwę konta magazynu platformy Azure * | Wprowadź nazwę konta magazynu. Nazwę konta magazynu jest wyświetlany w oknie dialogowym właściwości miejsca do magazynowania w portalu Azure. |
| Klawisz dostępu konta magazynu platformy Azure * | Wprowadź klucz konta miejsca do magazynowania. Klawisze dostępu są wyświetlane w oknie dialogowym właściwości miejsca do magazynowania w portalu Azure. |

    Te poświadczenia są używane do zezwolić aplikacji logiki do łączenia i uzyskać dostęp do danych. 

4. Wybierz polecenie **Utwórz**.

5. Zwróć uwagę, że utworzono połączenie. Teraz kontynuować innych czynności w aplikacji logiczny: 

    ![Azure kroku tworzenia połączenia magazyn obiektów Blob](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  
