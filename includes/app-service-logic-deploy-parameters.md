Przy użyciu Menedżera zasobów Azure Definiowanie parametrów wartości, które chcesz określić po wdrożeniu szablonu. Szablon zawiera sekcję o nazwie parametry, która zawiera wszystkie wartości parametru.
Należy zdefiniować parametr te wartości, które różnią się na podstawie projektu, który instalujesz lub na podstawie środowiska, który instalujesz do. Nie Definiowanie parametrów do wartości, które zawsze pozostanie takie same. Każda wartość parametru jest używana w szablonie Aby zdefiniować zasoby, które są wdrażanie. 

Podczas definiowania parametrów, należy użyć pola **allowedValues** , aby określić wartości, które umożliwiają użytkownika podczas wdrażania. Aby przypisać wartość parametru należy użyć pola **wartość domyślna** , jeśli wartość nie jest dostępna podczas wdrażania.

Firma Microsoft opisując każdego parametru w szablonie.

### <a name="logicappname"></a>logicAppName

Nazwa aplikacji logiki do utworzenia.

    "logicAppName": {
        "type": "string"
    }