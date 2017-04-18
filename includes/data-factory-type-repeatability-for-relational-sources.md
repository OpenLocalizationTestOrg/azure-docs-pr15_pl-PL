## <a name="repeatability-during-copy"></a>Powtarzalność podczas kopiowania

Podczas kopiowania danych od i do relacyjnych sklepy, należy pamiętać o powtarzalności Aby uniknąć niezamierzonych wyników. 

Wycinek można automatycznie ponownie w Azure Factory danych zgodnie z zasadami ponawiania określonymi. Firma Microsoft zaleca ustawienia zasady ponów próbę Aby zabezpieczyć się przed przejściowych błędy. W związku z tym powtarzalność jest ważnym aspektem obsługę podczas przenoszenia danych. 

**Jako źródła:**

> [AZURE.NOTE] Następujące próbki dla Azure SQL, ale mają zastosowanie do dowolnego magazynu danych obsługującego prostokątnym zestawy danych. Może być konieczne dopasowanie **Typ** źródła i właściwości **kwerendy** (na przykład: kwerendy zamiast sqlReaderQuery) dla danych przechowywania.   

Zazwyczaj podczas czytania sklepach relacyjnych, czy chcesz odczytać tylko dane odpowiadające ten wycinek. Jak to zrobić będzie przy użyciu zmiennych WindowStart i WindowEnd dostępne w Azure danych Factory. Przeczytaj o zmiennych i funkcji w Azure danych tutaj Factory w artykule [Planowanie i wykonanie](../articles/data-factory/data-factory-scheduling-and-execution.md) . Przykład: 
    
      "source": {
        "type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
      },

Ta kwerenda odczytuje dane z Moja_tabela, która znajduje się w zakresie czasu trwania wycinek. Uruchom ten wycinek będzie zawsze upewnij się, to zachowanie. 

W innych przypadkach możesz przeczytać całą tabelę (Załóżmy dla jeden raz tylko przenoszenie) i może zdefiniować sqlReaderQuery w następujący sposób:

    
    "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "select * from MyTable"
              },
    
