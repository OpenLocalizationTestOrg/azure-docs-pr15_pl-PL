
Domyślnie interfejsy API w aplikacji Mobile wewnętrznej bazie danych mogą być wywoływane anonimowo. Następnie należy ograniczyć dostęp do tylko uwierzytelnionym klientom.  

+ **Node.js wewnętrznej bazy danych (za pośrednictwem portalu)** :  
    
    W aplikacji Mobile **ustawień**kliknij **Łatwe tabel** i wybierz tabelę. Kliknij przycisk **Zmień uprawnienia**, wybierz pozycję **Dostęp uwierzytelniony tylko** dla wszystkich uprawnień, a następnie kliknij przycisk **Zapisz**. 

+ **.NET wewnętrznej bazy danych (C#)**:  

    W programie project server, przejdź do **kontrolerów** > **TodoItemController.cs**. Dodawanie `[Authorize]` atrybutów do klasy **TodoItemController** w następujący sposób. Aby ograniczyć dostęp tylko do określonych metod, można również zastosować ten atrybut tylko do jednej z tych metod zamiast klasy. Ponowne publikowanie na serwerze project Server.


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **Node.js wewnętrznej bazy danych (przez kod Node.js)** :  
    
    Aby wymagać uwierzytelniania dla tabeli programu access, należy dodać następujący wiersz do skryptu serwera Node.js:


        table.access = 'authenticated';

    Aby uzyskać więcej szczegółowych informacji, zobacz [jak: wymaga uwierzytelniania, aby uzyskać dostęp do tabel](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Aby dowiedzieć się, jak pobrać z witryny projektu kodu Szybki Start, zobacz [jak: pobieranie przy użyciu cyfra projekt kodu Node.js wewnętrznej bazy danych Szybki Start](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).

