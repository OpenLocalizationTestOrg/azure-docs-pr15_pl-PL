## <a name="repeatability-during-copy"></a>Powtarzalność podczas kopiowania

Podczas kopiowania danych do serwera SQL-SQL Azure z innych danych przechowywanych należy pamiętać o powtarzalności Aby uniknąć niezamierzonych wyników. 

Podczas kopiowania danych do bazy danych serwera SQL/SQL Azure, kopiowanie działanie będzie domyślnie Dołącz zestawu danych do tabeli sink domyślnie. Na przykład podczas kopiowania danych ze źródła pliku CSV (dane wartości rozdzielanych przecinkami) zawierający dwa rekordy do bazy danych serwera SQL-SQL Azure, Oto jak wygląda tabeli:
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00

Załóżmy, że znaleziono błędy w pliku źródłowym i aktualizowane ilość rurki w dół od 2 do 4 w pliku źródłowym. Jeśli ponownie uruchomić wycinek danych dla tego okresu, można znaleźć dwa nowe rekordy dołączane do bazy danych serwera SQL-SQL Azure. Poniżej założono, żadna z kolumn w tabeli ograniczenia klucza podstawowego.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   2           2015-05-01 00:00:00
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Aby tego uniknąć, należy określić znaczeń właściwych UPSERT wykorzystując jeden z poniżej 2 mechanizmy podaną poniżej.

> [AZURE.NOTE] Wycinek można ponownie uruchomić automatycznie w Azure Factory danych zgodnie z zasadami ponawiania określonymi.

### <a name="mechanism-1"></a>Mechanizm 1

Można wykorzystać właściwość **sqlWriterCleanupScript** najpierw przeprowadzić Akcja oczyszczania po uruchomieniu wycinek. 

    "sink":  
    { 
      "type": "SqlSink", 
      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
    }

Skrypt oczyszczanie może być wykonywane pierwszego podczas kopiowania dla danego wycinek, który chcesz usunąć dane z tabeli SQL odpowiadające ten wycinek. Działania zostanie następnie wstawiania danych w tabeli SQL. 

Jeśli wycinek jest teraz ponownie uruchom, a następnie możesz znajdzie ilość jest aktualizowana jako żądane.
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    6   Flat Washer 3           2015-05-01 00:00:00
    7   Down Tube   4           2015-05-01 00:00:00

Załóżmy, że rekord prostym spryskiwacza zostanie usunięty z oryginalnego pliku csv. Następnie uruchom ponownie wycinek przyjmie następujący wynik: 
    
    ID  Product     Quantity    ModifiedDate
    ... ...         ...         ...
    7   Down Tube   4           2015-05-01 00:00:00

Żadne nowe miały zostać wykonane. Działanie kopii uruchomił skrypt Oczyszczanie usunąć odpowiednich danych dla ten wycinek. A następnie go więcej danych wejściowych z pliku csv (która następnie zawarte tylko 1 rekord) i wstawić go do tabeli. 

### <a name="mechanism-2"></a>Mechanizm 2
> [AZURE.IMPORTANT] sliceIdentifierColumnName nie jest obsługiwane dla magazynu danych SQL Azure w tej chwili. 

Inny mechanizm uzyskanie powtarzalności jest przez dedykowane kolumny (**sliceIdentifierColumnName**) w tabeli docelowej. W tej kolumnie może być używane przez Factory danych Azure mieć pewność, że źródłowa i docelowa zachować synchronizację. Ta metoda działa po elastyczność podczas zmieniania i schematów SQL tabeli docelowej. 

W tej kolumnie może być używane przez Azure Factory danych na potrzeby powtarzalności i w procesie Factory danych Azure nie spowoduje zmiany schematów do tabeli. Sposób użycia tej metody:

1.  Definiowanie kolumny typu binarnego (32) w polu miejsce docelowe tabela programu SQL. Należy bez ograniczeń w tej kolumnie. Załóżmy nazwa w tej kolumnie jako "ColumnForADFuseOnly" w tym przykładzie.
2.  Należy użyć go w działaniu kopii w następujący sposób:

        "sink":  
        { 
          "type": "SqlSink", 
          "sliceIdentifierColumnName": "ColumnForADFuseOnly"
        }

Azure Factory danych będą wypełniać w tej kolumnie zgodnie potrzeba upewnij się, że źródłowa i docelowa zachować synchronizację. Wartości w tej kolumnie nie będą używane poza tym kontekście przez użytkownika. 

Podobnie jak mechanizm 1, Kopiuj aktywności zostanie automatycznie najpierw wyczyścić dane dla danego wycinek przeznaczenia tabela programu SQL, a następnie uruchom aktywności kopii zwykle, aby wstawić dane ze źródła do miejsca docelowego dla ten wycinek. 
