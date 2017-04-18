<properties
   pageTitle="Menedżer zasobów szablonu funkcje | Microsoft Azure"
   description="W tym artykule opisano funkcje za pomocą szablonu Menedżera zasobów Azure pobierania wartości, Praca z ciągami i numeryczne i pobrać informacje na temat wdrażania."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-template-functions"></a>Azure funkcji szablonu Menedżera zasobów

W tym temacie opisano wszystkie funkcje używanego w szablonie Azure Menedżera zasobów.

Funkcje szablonu i ich parametrów wielkość liter. Na przykład Menedżer zasobów rozwiązuje **variables('var1')** i **VARIABLES('VAR1')** jako takie same. Gdy szacowana jest funkcja, o ile funkcja wyraźnie Zmienia litery (na przykład toUpper lub toLower), funkcja zachowuje wielkość liter. Niektóre typy zasobów mogą mieć wielkość liter wymagania niezależnie od tego, jak są obliczane w funkcji.

## <a name="numeric-functions"></a>Funkcje numeryczne

Menedżer zasobów udostępnia następujące funkcje do pracy z liczbami całkowitymi:

- [Dodawanie](#add)
- [copyIndex](#copyindex)
- [Dziel](#div)
- [int](#int)
- [mod](#mod)
- [mul](#mul)
- [Sub](#sub)


<a id="add" />
### <a name="add"></a>Dodawanie

**Dodawanie (operand1, operand2)**

Zwraca sumę dwóch podanych liczb całkowitych.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| operand1                           |   Tak    | Pierwszy liczba całkowita, aby dodać.
| operand2                           |   Tak    | Druga liczba całkowita, aby dodać.

W poniższym przykładzie dodawane dwa parametry.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to add"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to add"
        }
      }
    },
    ...
    "outputs": {
      "addResult": {
        "type": "int",
        "value": "[add(parameters('first'), parameters('second'))]"
      }
    }

<a id="copyindex" />
### <a name="copyindex"></a>copyIndex

**copyIndex(offset)**

Zwraca bieżącą indeks iteracji pętli. 

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| Przesunięcie                           |   Brak    | Kwota, aby dodać do bieżącej wartości iteracji.

Ta funkcja jest zawsze używana z **Kopiuj** obiekt. Pełny opis korzystania **copyIndex**zobacz [Tworzenie wielu wystąpień zasobów w Menedżerze zasobów Azure](resource-group-create-multiple.md).

W poniższym przykładzie pokazano pętla kopii, a wartość indeksu zawarte w nazwie. 

    "resources": [ 
      { 
        "name": "[concat('examplecopy-', copyIndex())]", 
        "type": "Microsoft.Web/sites", 
        "copy": { 
          "name": "websitescopy", 
          "count": "[parameters('count')]" 
        }, 
        ...
      }
    ]


<a id="div" />
### <a name="div"></a>Dziel

**Dziel (operand1, operand2)**

Zwraca wartość liczby całkowitej podziału dwóch podanych liczb całkowitych.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| operand1                           |   Tak    | Liczba całkowita jest rozdzielany.
| operand2                           |   Tak    | Liczba całkowita jest używana do dzielenia. Nie może być równa 0.

Poniższy przykład dzieli jeden parametr przez inny parametr.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "divResult": {
        "type": "int",
        "value": "[div(parameters('first'), parameters('second'))]"
      }
    }

<a id="int" />
### <a name="int"></a>int

**int(valueToConvert)**

Konwertuje wartość określonej liczby całkowitej.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Tak    | Wartość do przekonwertowania do liczby całkowitej. Typ wartości może zawierać tylko ciąg lub liczba całkowita.

Poniższy przykład konwertuje wartość parametru użytkownika do liczby całkowitej.

    "parameters": {
        "appId": { "type": "string" }
    },
    "variables": { 
        "intValue": "[int(parameters('appId'))]"
    }


<a id="mod" />
### <a name="mod"></a>mod

**MOD (operand1, operand2)**

Zwraca resztę z dzielenia liczby całkowitej za pomocą dwóch podanych liczb całkowitych.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| operand1                           |   Tak    | Liczba całkowita jest rozdzielany.
| operand2                           |   Tak    | Liczba całkowita, która jest używana do dzielenia, musi być inne niż 0.

W poniższym przykładzie zwracany reszta z podzielenia jeden parametr przez inny parametr.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer being divided"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer used to divide"
        }
      }
    },
    ...
    "outputs": {
      "modResult": {
        "type": "int",
        "value": "[mod(parameters('first'), parameters('second'))]"
      }
    }

<a id="mul" />
### <a name="mul"></a>mul

**mul (operand1, operand2)**

Zwraca iloczyn dwóch podanych liczb całkowitych.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| operand1                           |   Tak    | Pierwszy liczba całkowita mnożenia.
| operand2                           |   Tak    | Druga liczba całkowita mnożenia.

Poniższy przykład mnoży jeden parametr przez inny parametr.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "First integer to multiply"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Second integer to multiply"
        }
      }
    },
    ...
    "outputs": {
      "mulResult": {
        "type": "int",
        "value": "[mul(parameters('first'), parameters('second'))]"
      }
    }

<a id="sub" />
### <a name="sub"></a>Sub

**Sub (operand1, operand2)**

Zwraca odejmowanie dwóch podanych liczb całkowitych.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| operand1                           |   Tak    | Liczba całkowita jest odjęta od.
| operand2                           |   Tak    | Liczba całkowita jest odjęta.

Poniższy przykład odejmuje jeden parametr z innego parametru.

    "parameters": {
      "first": {
        "type": "int",
        "metadata": {
          "description": "Integer subtracted from"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Integer to subtract"
        }
      }
    },
    ...
    "outputs": {
      "subResult": {
        "type": "int",
        "value": "[sub(parameters('first'), parameters('second'))]"
      }
    }

## <a name="string-functions"></a>Funkcje tekstowe

Menedżer zasobów udostępnia następujące funkcje do pracy z ciągów:

- [Base64](#base64)
- ["concat"](#concat)
- [długość](#lengthstring)
- [padLeft](#padleft)
- [zamienianie](#replace)
- [Pomiń](#skipstring)
- [dzielenie](#split)
- [ciąg](#string)
- [podciąg](#substring)
- [sporządzanie](#takestring)
- [toLower](#tolower)
- [toUpper](#toupper)
- [Przycinanie](#trim)
- [uniqueString](#uniquestring)
- [Identyfikator URI](#uri)


<a id="base64" />
### <a name="base64"></a>Base64

**Base64 (inputString)**

Zwraca wartość odpowiadającą base64 ciągu wejściowego.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| inputString                        |   Tak    | Wartość ciągu, aby zwrócić jako reprezentacja base64.

W poniższym przykładzie pokazano, jak używać funkcji base64.

    "variables": {
      "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
      "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

<a id="concat" />
### <a name="concat---string"></a>"concat" - ciągu

**"concat" (ciąg1, ciąg2, ciąg3,...)**

Łączy kilka wartości ciągów i zwraca połączony ciąg. 

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| ciąg1                        |   Tak    | Wartość ciągu do łączenia.
| dodatkowe ciągów             |   Brak     | Ciąg wartości do łączenia.

Ta funkcja może zająć dowolną liczbę argumentów i zaakceptować ciągów lub tablic parametrów. Przykład łączenia tablic, zobacz ["concat" - tablicy](#concatarray).

W poniższym przykładzie pokazano sposób łączenia wielu wartości parametrów zwracana połączony ciąg.

    "outputs": {
        "siteUri": {
          "type": "string",
          "value": "[concat('http://', reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
        }
    }


<a id="lengthstring" />
### <a name="length---string"></a>długość - ciągu

**length(String)**

Zwraca liczbę znaków w ciągu.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| ciąg                        |   Tak    | Wartość ciągu dla wprowadzenie liczby znaków.

Przykład długość za pomocą tablicy, zobacz [Długość - tablicy](#length).

Poniższy przykład zwraca liczbę znaków w ciągu. 

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "nameLength": "[length(parameters('appName'))]"
    }
        

<a id="padleft" />
### <a name="padleft"></a>padLeft

**padLeft (valueToPad, wartość właściwości totalLength, paddingCharacter)**

Zwraca ciąg wyrównane do prawej, dodając znaków z lewej strony do osiągnięcia sumy określonej długości.
  
| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| valueToPad                         |   Tak    | Ciąg lub int wyrównać do prawej.
| wartość właściwości totalLength                        |   Tak    | Całkowita liczba znaków w zwracany ciąg.
| paddingCharacter                   |   Brak     | Znak służących do lewej dopełnienia aż do osiągnięcia całkowitą długość. Wartość domyślna to miejsce.

W poniższym przykładzie pokazano sposób konsoli wartości parametru użytkownika, dodając znak "zero", aż osiągnie ciąg 10 znaków. Jeśli wartość parametru oryginalny jest dłuższa niż 10 znaków, zostaną dodane żadnych znaków.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "paddedAppName": "[padLeft(parameters('appName'),10,'0')]"
    }

<a id="replace" />
### <a name="replace"></a>zamienianie

**Zamień (originalString, oldCharacter, newCharacter)**

Zwraca nowy ciąg z wszystkich wystąpień o jeden znak w określony ciąg zamienione na inny znak.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| originalString                     |   Tak    | Ciąg, który zawiera wszystkie wystąpienia zamienione na inny znak o jeden znak.
| oldCharacter                       |   Tak    | Znak, który ma zostać usunięty z oryginalnego ciągu.
| newCharacter                       |   Tak    | Znak Dodawanie zamiast znakiem usunięte.

W poniższym przykładzie pokazano, jak usunąć wszystkie kreski z ciągu użytkownika.

    "parameters": {
        "identifier": { "type": "string" }
    },
    "variables": { 
        "newidentifier": "[replace(parameters('identifier'),'-','')]"
    }

<a id="skipstring" />
### <a name="skip---string"></a>Pomiń - ciągu
**Pomiń (originalValue, numberToSkip)**

Zwraca ciąg zawierający wszystkie znaki po określonej liczbie w ciągu.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| originalValue                      |   Tak    | Ciąg, który ma być używany pomijania.
| numberToSkip                       |   Tak    | Liczba znaków, aby pominąć. Jeśli ta wartość jest mniejsze lub równe 0, zwracane są wszystkie znaki w ciągu. Jeśli plik jest większy niż długość ciągu, zwracany jest pusty ciąg. 

Przykład Pomiń za pomocą tablicy zobacz [pominąć - tablicy](#skip).

Poniższy przykład pomija określoną liczbę znaków w ciągu.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for skipping"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[skip(parameters('first'),parameters('second'))]"
      }
    }


<a id="split" />
### <a name="split"></a>dzielenie

**Podziel (inputString, delimiterString)**

**Podziel (inputString, delimiterArray)**

Zwraca tablicę ciągów zawierający podciągi ciąg wejściowy są rozdzielone określonej ograniczniki.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| inputString                        |   Tak    | Ciąg dzielenia.
| ogranicznika                          |   Tak    | Ogranicznik, być może być jeden ciąg znaków lub tablica ciągów.

Poniższy przykład dzieli ciąg wejściowy przecinkami.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "stringPieces": "[split(parameters('inputString'), ',')]"
    }

Następny przykład dzieli ciąg wejściowy przecinkami lub średnikami.

    "variables": {
      "stringToSplit": "test1,test2;test3",
      "delimiters": [ ",", ";" ]
    },
    "resources": [ ],
    "outputs": {
      "exampleOutput": {
        "value": "[split(variables('stringToSplit'), variables('delimiters'))]",
        "type": "array"
      }
    }

<a id="string" />
### <a name="string"></a>ciąg

**String(valueToConvert)**

Konwertuje ciąg określonej wartości.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| valueToConvert                     |   Tak    | Wartość do przekonwertowania na ciąg. Można konwertować dowolny typ wartości, takich jak obiekty i tablic.

Poniższy przykład konwertuje wartości parametrów użytkownika na ciągi znaków.

    "parameters": {
      "jsonObject": {
        "type": "object",
        "defaultValue": {
          "valueA": 10,
          "valueB": "Example Text"
        }
      },
      "jsonArray": {
        "type": "array",
        "defaultValue": [ "a", "b", "c" ]
      },
      "jsonInt": {
        "type": "int",
        "defaultValue": 5
      }
    },
    "variables": { 
      "objectString": "[string(parameters('jsonObject'))]",
      "arrayString": "[string(parameters('jsonArray'))]",
      "intString": "[string(parameters('jsonInt'))]"
    }

<a id="substring" />
### <a name="substring"></a>podciąg

**substring (stringToParse, Wartość startIndex, długość)**

Zwraca ciąg, który zaczyna się od pozycji określonego znaku i zawiera określoną liczbę znaków.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| stringToParse                     |   Tak    | Oryginalny ciąg znaków, z którego wyodrębnieniu podciąg.
| Wartość startIndex                         | Brak      | Od zera początkowe położenie znaku dla podciąg.
| długość                             | Brak      | Liczba znaków podciąg.

Poniższy przykład wyodrębnia znaki pierwsze trzy z parametrem.

    "parameters": {
        "inputString": { "type": "string" }
    },
    "variables": { 
        "prefix": "[substring(parameters('inputString'), 0, 3)]"
    }

<a id="takestring" />
### <a name="take---string"></a>Przejmowanie - ciągu
**Przejmowanie (originalValue, numberToTake)**

Zwraca ciąg zawierający określoną liczbę znaków od początku ciągu.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| originalValue                      |   Tak    | Ciąg znaków z wykonać.
| numberToTake                       |   Tak    | Liczba znaków w celu wykonania. Jeśli ta wartość jest mniejsze lub równe 0, zwracany jest pusty ciąg. Jeśli plik jest większy niż długość ciągu, zwracane są wszystkie znaki w ciągu.

Na przykład używania skorzystaj z tablicy zobacz [wykonać - tablicy](#take).

W poniższym przykładzie pokazano określoną liczbę znaków z ciągu.

    "parameters": {
      "first": {
        "type": "string",
        "metadata": {
          "description": "Value to use for taking"
        }
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of characters to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "string",
        "value": "[take(parameters('first'), parameters('second'))]"
      }
    }

<a id="tolower" />
### <a name="tolower"></a>toLower

**toLower(stringToChange)**

Konwertuje określonego ciągu znaków na małe litery.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Tak    | Ciąg do przekonwertowania na małe litery.

Poniższy przykład konwertuje wartość parametru użytkownika na małe litery.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "lowerCaseAppName": "[toLower(parameters('appName'))]"
    }

<a id="toupper" />
### <a name="toupper"></a>toUpper

**toUpper(stringToChange)**

Konwertuje określonego ciągu na wielkie litery.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| stringToChange                     |   Tak    | Ciąg do przekonwertowania na wielkie litery.

Poniższy przykład konwertuje wartość parametru użytkownika na wielkie litery.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "upperCaseAppName": "[toUpper(parameters('appName'))]"
    }

<a id="trim" />
### <a name="trim"></a>Przycinanie

**przycinanie (stringToTrim)**

Usuwa wszystkie znaki wiodące i końcowe odstępu z określonego ciągu znaków.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| stringToTrim                       |   Tak    | Ciąg przycięcia.

Poniższy przykład przycina spacji w wartości parametru użytkownika.

    "parameters": {
        "appName": { "type": "string" }
    },
    "variables": { 
        "trimAppName": "[trim(parameters('appName'))]"
    }

<a id="uniquestring" />
### <a name="uniquestring"></a>uniqueString

**uniqueString (baseString,...)**

Tworzy ciąg mieszania deterministyczne na podstawie wartości podanych jako parametry. 

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| baseString      |   Tak    | Ciąg używane w funkcji skrótu do tworzenia unikatowy ciąg.
| dodatkowe parametry stosownie do potrzeb    | Brak       | Możesz dodać dowolną liczbę ciągów potrzebne do utworzenia wartość, która określa poziom unikatowość.

Ta funkcja jest przydatne, gdy trzeba utworzyć unikatową nazwę dla zasobu. Podaj wartości parametrów, które ograniczania zakresu unikatowość wyników. Można określić, czy nazwa jest unikatowy w dół do subskrypcji, grupa zasobów lub wdrożenia. 

Zwracana wartość nie jest przypadkowe ciągu, ale zamiast wynik funkcji skrótu. Zwracana wartość jest 13 znaków. Nie jest globalnie unikatowa. Można połączyć wartości z prefiksem z Konwencja nazewnictwa utworzyć nazwę opisową. W poniższym przykładzie pokazano formatu zwrócona wartość. Oczywiście rzeczywista wartość zależą od podanych parametrach.

    tcvhiyu5h2o5o

W poniższych przykładach pokazano, jak za pomocą uniqueString tworzenie unikatowych wartości dla często używanych poziomów.

Unikatowe występujące w subskrypcji

    "[uniqueString(subscription().subscriptionId)]"

Unikatowe ograniczone do grupy zasobów

    "[uniqueString(resourceGroup().id)]"

Unikatowe występujące w wdrożenia dla grupy zasobów

    "[uniqueString(resourceGroup().id, deployment().name)]"
    
W poniższym przykładzie pokazano, jak utworzyć unikatową nazwę konta magazynu na podstawie swojej grupy zasobów (wewnątrz ta grupa zasobów, nazwa nie jest unikatowy Jeżeli tworzona w ten sam sposób).

    "resources": [{ 
        "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]", 
        "type": "Microsoft.Storage/storageAccounts", 
        ...



<a id="uri" />
### <a name="uri"></a>Identyfikator URI

**Identyfikator URI (baseUri, relativeUri)**

Tworzy bezwzględnego identyfikatora URI, łącząc baseUri oraz ciąg relativeUri.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| baseUri                            |   Tak    | Ciąg podstawowy identyfikator uri.
| relativeUri                        |   Tak    | Ciąg względnego identyfikatora uri dodać do ciągu podstawowy identyfikator uri.

Wartość parametru **baseUri** mogą zawierać określonego pliku, ale tylko ścieżki bazowej jest używana podczas tworzenia identyfikatora URI. Na przykład przekazywanie **http://contoso.com/resources/azuredeploy.json** jako baseUri parametru skutkuje podstawowy identyfikator URI **http://contoso.com/resources/**.

W poniższym przykładzie pokazano sposób tworzenia łącza do zagnieżdżonych szablonu na podstawie wartości szablonu nadrzędnej.

    "templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"

## <a name="array-functions"></a>Funkcji tablicowych

Menedżer zasobów udostępnia kilka funkcji do pracy z wartościami w tablicy.

- ["concat"](#concatarray)
- [długość](#length)
- [Pomiń](#skip)
- [sporządzanie](#take)

Aby uzyskać tablicę wartości ciągu rozdzielone wartości, zobacz [Dzielenie](#split).

<a id="concatarray" />
### <a name="concat---array"></a>"concat" - tablicy

**"concat" (tablica1, tablica2; tablica3;...)**

Łączy wielu tablicach i zwraca tablicę połączonych. 

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| Tablica1                        |   Tak    | Tablica do łączenia.
| dodatkowe tablic             |   Brak     | Tablice służy do łączenia.

Ta funkcja może zająć dowolną liczbę argumentów i zaakceptować ciągów lub tablic parametrów. Przykład łączenia wartości ciągu zobacz ["concat" - ciąg](#concat).

W poniższym przykładzie pokazano sposób łączenia dwóch tablic.

    "parameters": {
        "firstarray": {
            type: "array"
        }
        "secondarray": {
            type: "array"
        }
     },
     "variables": {
         "combinedarray": "[concat(parameters('firstarray'), parameters('secondarray'))]
     }
        

<a id="length" />
### <a name="length---array"></a>Długość - tablicy

**length(Array)**

Zwraca liczbę elementów w tablicy.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| Tablica                        |   Tak    | Tablica służących do pobierania liczby elementów.

Ta funkcja z tablicą umożliwia określenie liczby iteracji podczas tworzenia zasobów. W poniższym przykładzie parametr **siteNames** czy dotyczą tablicę nazw do użytku podczas tworzenia witryn sieci web.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

Aby uzyskać więcej informacji na temat używania tej funkcji z tablicy zobacz [Tworzenie wielu wystąpień zasobów w Menedżerze zasobów Azure](resource-group-create-multiple.md). 

Przykład długość za pomocą wartość ciągu zobacz [Długość - ciąg](#lengthstring).

<a id="skip" />
### <a name="skip---array"></a>Pomiń - tablicy
**Pomiń (originalValue, numberToSkip)**

Zwraca tablicę zawierająca wszystkie elementy po określonej liczbie w tablicy.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| originalValue                      |   Tak    | Tablica umożliwia pomijania.
| numberToSkip                       |   Tak    | Liczba elementów, aby pominąć. Jeśli ta wartość jest mniejsze lub równe 0, zwracane są wszystkie elementy w tablicy. Jeśli plik jest większy niż długość tablicy, zwracana jest pusta tablica. 

Przykład użycia Pomiń ciągiem zobacz [Pomiń - ciąg](#skipstring).

Poniższy przykład pomija określoną liczbę elementów w tablicy.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for skipping"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to skip"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[skip(parameters('first'), parameters('second'))]"
      }
    }

<a id="take" />
### <a name="take---array"></a>sporządzanie - tablicy
**Przejmowanie (originalValue, numberToTake)**

Zwraca tablicę z określoną liczbę elementów od początku tablicy.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| originalValue                      |   Tak    | Tablica podjęcie elementów za pomocą.
| numberToTake                       |   Tak    | Liczba elementów podjęcie. Jeśli wartość ta jest mniejsze lub równe 0, zwracana jest pusta tablica. Jeśli jest większa niż długość danej tablicy, są zwracane wszystkich elementów w tablicy.

Przykład użycia skorzystaj z ciągiem zobacz [wykonać - ciąg](#takestring).

W poniższym przykładzie pokazano określoną liczbę elementów w tablicy.

    "parameters": {
      "first": {
        "type": "array",
        "metadata": {
          "description": "Values to use for taking"
        },
        "defaultValue": [ "one", "two", "three" ]
      },
      "second": {
        "type": "int",
        "metadata": {
          "description": "Number of elements to take"
        }
      }
    },
    "resources": [
    ],
    "outputs": {
      "return": {
        "type": "array",
        "value": "[take(parameters('first'),parameters('second'))]"
      }
    }

## <a name="deployment-value-functions"></a>Funkcje wartość wdrażania

Menedżer zasobów udostępnia następujące funkcje uzyskania wartości z sekcji szablonu i wartości dotyczące rozmieszczania:

- [wdrożenie](#deployment)
- [Parametry](#parameters)
- [zmienne](#variables)

Aby uzyskać wartości z zasobów, grup zasobów lub subskrypcji, zobacz [Funkcje zasobów](#resource-functions).

<a id="deployment" />
### <a name="deployment"></a>wdrożenie

**Deployment()**

Zwraca informacje o bieżącej operacji wdrożenia.

Ta funkcja zwraca obiekt, który jest przekazywany podczas wdrażania. Właściwości w zwracany obiekt są różne w zależności od tego, czy obiekt wdrożenia jest przekazywany jako łącze lub jako obiekt w tekście. 

Gdy obiektu wdrożenia przekazywana w tekście, takich jak podczas przy użyciu parametru **- TemplateFile** Azure programu PowerShell wskaż plik lokalny, zwracany obiekt ma następujący format:

    {
        "name": "",
        "properties": {
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [
                ],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

Gdy obiekt jest przekazywany jako łącze, takie jak podczas wskaż polecenie Obiekt zdalnych przy użyciu parametru **- TemplateUri** obiekt jest zwracana w następującym formacie. 

    {
        "name": "",
        "properties": {
            "templateLink": {
                "uri": ""
            },
            "template": {
                "$schema": "",
                "contentVersion": "",
                "parameters": {},
                "variables": {},
                "resources": [],
                "outputs": {}
            },
            "parameters": {},
            "mode": "",
            "provisioningState": ""
        }
    }

W poniższym przykładzie pokazano, jak za pomocą deployment() łącze do innego szablonu opartego na identyfikator URI szablonu nadrzędnej.

    "variables": {  
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
    }  

<a id="parameters" />
### <a name="parameters"></a>Parametry

**Parametry (nazwa parametru)**

Zwraca wartość parametru. W sekcji Parametry szablonu, należy zdefiniować nazwę określonego parametru.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| Nazwa parametru                      |   Tak    | Nazwa parametru zwracana.

W poniższym przykładzie pokazano uproszczone użycie funkcji parametrów.

    "parameters": { 
      "siteName": {
          "type": "string"
      }
    },
    "resources": [
       {
          "apiVersion": "2014-06-01",
          "name": "[parameters('siteName')]",
          "type": "Microsoft.Web/Sites",
          ...
       }
    ]

<a id="variables" />
### <a name="variables"></a>zmienne

**zmienne (variableName)**

Zwraca wartość zmiennej. W sekcji Zmienne szablonu, należy zdefiniować zmiennych o określonej nazwie.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| Nazwa zmiennej                      |   Tak    | Nazwa zmiennej zwracana.

W poniższym przykładzie użyto wartości zmiennej.

    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
      {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
      }
    ],

## <a name="resource-functions"></a>Funkcje zasobów

Menedżer zasobów udostępnia następujące funkcje uzyskania wartości zasobu:

- [listKeys i lista {wartość}](#listkeys)
- [dostawców](#providers)
- [odwołania](#reference)
- [Grupa zasobów](#resourcegroup)
- [resourceId](#resourceid)
- [subskrypcji](#subscription)

Do pobierania wartości z parametrów, zmienne lub bieżący wdrażania, zobacz [Funkcje wartość wdrożenia](#deployment-value-functions).

<a id="listkeys" />
<a id="list" />
### <a name="listkeys-and-listvalue"></a>listKeys i lista {wartość}

**listKeys (resourceName lub resourceIdentifier, apiVersion)**

**Lista {wartość} (resourceName lub resourceIdentifier, apiVersion)**

Zwraca wartości dla dowolnego typu zasobu, który obsługuje operacja listy. Najbardziej typowe zastosowania jest **listKeys**. 
  
| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| resourceName lub resourceIdentifier |   Tak    | Unikatowy identyfikator zasobu.
| apiVersion                         |   Tak    | Wersja interfejsu API stanu środowisko uruchomieniowe zasobu.

Dowolnej operacji, które rozpoczynają się od **listy** można używać funkcji w szablonie. Dostępne operacje zawierać nie tylko **listKeys**, ale również operacji, takich jak **listy**, **listAdminKeys**i **listStatus**. Aby określić, które typy zasobów mają operacji na liście, użyj następującego polecenia programu PowerShell.

    Get-AzureRmProviderOperation -OperationSearchString *  | where {$_.Operation -like "*list*"} | FT Operation

Lub pobrać listę z polecenie Azure. W poniższym przykładzie pobiera wszystkie operacje w **apiapps**a używa narzędzie JSON [jq](http://stedolan.github.io/jq/download/) filtrować tylko operacje listy.

    azure provider operations show --operationSearchString */apiapps/* --json | jq ".[] | select (.operation | contains(\"list\"))"

ResourceId można określić przy użyciu [funkcji resourceId](#resourceid) lub przy użyciu formatu **{providerNamespace}-{typu zasobu}-{resourceName}**.

W poniższym przykładzie pokazano sposób zwracania kluczy podstawowych i pomocniczych z konta miejsca do magazynowania w sekcji wyjściowe.

    "outputs": { 
      "listKeysOutput": { 
        "value": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2016-01-01')]", 
        "type" : "object" 
      } 
    } 

Zwracany obiekt z listKeys ma następujący format:

    {
      "keys": [
        {
          "keyName": "key1",
          "permissions": "Full",
          "value": "{value}"
        },
        {
          "keyName": "key2",
          "permissions": "Full",
          "value": "{value}"
        }
      ]
    }

<a id="providers" />
### <a name="providers"></a>dostawców

**dostawcy (providerNamespace, [typu zasobu])**

Zwraca informacje o dostawcy zasobów i jego typów zasobów obsługiwane. Jeśli typ zasobu nie zostanie określona, funkcja zwraca wszystkie obsługiwane typy dla dostawcy zasobów.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| providerNamespace                  |   Tak    | Namespace dostawcy
| typu zasobu                       |   Brak     | Typ zasobu w określonym obszarze nazw.

Każdego typu obsługiwane jest zwracana w następującym formacie. Kolejność tablicy nie jest gwarantowana.

    {
        "resourceType": "",
        "locations": [ ],
        "apiVersions": [ ]
    }

W poniższym przykładzie pokazano, jak używać funkcji dostawcy:

    "outputs": {
        "exampleOutput": {
            "value": "[providers('Microsoft.Storage', 'storageAccounts')]",
            "type" : "object"
        }
    }

<a id="reference" />
### <a name="reference"></a>odwołania

**Odwołanie (resourceName lub resourceIdentifier, [apiVersion])**

Zwraca obiekt reprezentujący stanu środowisko uruchomieniowe innego zasobu.

| Parametr                          | Wymagane | Opis
| :--------------------------------: | :------: | :----------
| resourceName lub resourceIdentifier |   Tak    | Nazwa lub Unikatowy identyfikator zasobu.
| apiVersion                         |   Brak     | Wersja interfejsu API określonego zasobu. Ten parametr należy uwzględnić, gdy nie zainicjowano zasobu w ramach tego samego szablonu.

Funkcja **Odwołanie** pochodzi jej wartość w Państwie środowisko uruchomieniowe, a nie można używać w sekcji zmiennych. Czy można używać w sekcji wyjściowe szablonu.

Za pomocą funkcji odwołanie, możesz niejawnie deklarować że jeden zasób zależy od innego zasobu, jeśli żądanego zasobu jest obsługi administracyjnej w ramach tego samego szablonu. Nie należy również użyć tej właściwości **dependsOn** . Funkcja nie jest obliczana, dopóki żądanego zasobu zostanie ukończona wdrożenia.

Poniższy przykład odwołuje się do konta miejsca do magazynowania, który został wdrożony w tym samym szablonie.

    "outputs": {
        "NewStorage": {
            "value": "[reference(parameters('storageAccountName'))]",
            "type" : "object"
        }
    }

Poniższy przykład odwołuje się do konta miejsca do magazynowania, które nie jest używany w tym szablonie, ale nie istnieje w tej samej grupy zasobów jako zasoby wdrażania.

    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }

Jak pokazano w poniższym przykładzie, można pobrać określonej wartości z zwracane obiekcie, na przykład punkt końcowy obiektów blob identyfikator URI.

    "outputs": {
        "BlobUri": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Poniższy przykład odwołuje się z kontem przestrzeni dyskowej w różnych grupach zasobów.

    "outputs": {
        "BlobUri": {
            "value": "[reference(resourceId(parameters('relatedGroup'), 'Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
            "type" : "string"
        }
    }

Właściwości obiektu zwrócone przez funkcję **Odwołanie** zależy od typu zasobu. Aby wyświetlić nazwy właściwości i wartości Typ zasobu, Utwórz szablon proste, który zwraca obiekt w sekcji **Wyświetla** . Jeśli masz istniejący zasób typu szablonu po prostu zwraca obiekt bez wdrażania wszelkie nowe zasoby. Jeśli nie masz istniejący zasób tego typu, szablonu wdrożenie tylko na tego typu i zwraca obiekt. Następnie należy dodać te właściwości do innych szablonów, które trzeba dynamicznie pobierania wartości podczas wdrażania. 

<a id="resourcegroup" />
### <a name="resourcegroup"></a>Grupa zasobów

**resourceGroup()**

Zwraca obiekt reprezentujący bieżącej grupy zasobów. 

Zwracany obiekt znajduje się w następującym formacie:

    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
      "name": "{resourceGroupName}",
      "location": "{resourceGroupLocation}",
      "tags": {
      },
      "properties": {
        "provisioningState": "{status}"
      }
    }

W poniższym przykładzie użyto lokalizacji grupy zasobów do przypisywania lokalizacji dla witryny sieci web.

    "resources": [
       {
          "apiVersion": "2014-06-01",
          "type": "Microsoft.Web/sites",
          "name": "[parameters('siteName')]",
          "location": "[resourceGroup().location]",
          ...
       }
    ]

<a id="resourceid" />
### <a name="resourceid"></a>resourceId

**resourceId ([subscriptionId], [resourceGroupName], typu zasobu, resourceName1, [resourceName2]...)**

Zwraca unikatowy identyfikator zasobu. 
      
| Parametr         | Wymagane | Opis
| :---------------: | :------: | :----------
| subscriptionId    |   Brak     | Wartość domyślna to bieżącej subskrypcji. Określ wartość w przypadku należy pobrać zasobu w innej subskrypcji.
| resourceGroupName |   Brak     | Wartość domyślna to bieżąca grupa zasobów. Określ wartość w przypadku należy pobrać zasobu w innej grupie zasobów.
| typu zasobu      |   Tak    | Typ zasobu, łącznie z nazw dostawcy zasobów.
| resourceName1     |   Tak    | Nazwa zasobu.
| resourceName2     |   Brak     | Następny zasobu Nazwa odcinek Jeśli zasobów jest zagnieżdżone.

Jeśli nazwa zasobu jest niejednoznaczne lub nie ustanawianie w ramach tego samego szablonu korzystać z tej funkcji. Identyfikator jest zwracany w następującym formacie:

    /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/{resourceProviderNamespace}/{resourceType}/{resourceName}

W poniższym przykładzie pokazano, jak pobrać identyfikatory zasobów dla witryny sieci web i bazy danych. W grupie zasobów o nazwie **myWebsitesGroup** istnieje w witrynie sieci web i baza danych istnieje w bieżącej grupie zasobów dla tego szablonu.

    [resourceId('myWebsitesGroup', 'Microsoft.Web/sites', parameters('siteName'))]
    [resourceId('Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]
    
Często należy użyć tej funkcji podczas korzystania z konta miejsca do magazynowania lub wirtualną sieć w grupie alternatywny zasobów. Konto miejsca do magazynowania lub wirtualnej sieci mogą być używane w przypadku wielu grup zasobów; w związku z tym czy chcesz je usunąć, usuwając pojedynczej grupy zasobów. W poniższym przykładzie pokazano, jak łatwo można zasobu z grupy zasobów zewnętrznych:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
          "virtualNetworkName": {
              "type": "string"
          },
          "virtualNetworkResourceGroup": {
              "type": "string"
          },
          "subnet1Name": {
              "type": "string"
          },
          "nicName": {
              "type": "string"
          }
      },
      "variables": {
          "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
          "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
      },
      "resources": [
      {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[parameters('nicName')]",
          "location": "[parameters('location')]",
          "properties": {
              "ipConfigurations": [{
                  "name": "ipconfig1",
                  "properties": {
                      "privateIPAllocationMethod": "Dynamic",
                      "subnet": {
                          "id": "[variables('subnet1Ref')]"
                      }
                  }
              }]
           }
      }]
    }

<a id="subscription" />
### <a name="subscription"></a>subskrypcji

**Subscription()**

Zwraca szczegółowe informacje o subskrypcji w następującym formacie.

    {
        "id": "/subscriptions/#####",
        "subscriptionId": "#####",
        "tenantId": "#####"
    }

W poniższym przykładzie pokazano funkcję subskrypcji o nazwie w sekcji wyjściowe. 

    "outputs": { 
      "exampleOutput": { 
          "value": "[subscription()]", 
          "type" : "object" 
      } 
    } 


## <a name="next-steps"></a>Następne kroki
- Opis sekcji w szablonie Menedżera zasobów Azure zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md)
- Aby scalić wiele szablonów, zobacz [Korzystanie z szablonów połączone z Menedżerem zasobów Azure](resource-group-linked-templates.md)
- Aby przejść do określonej liczby godzin podczas tworzenia typ zasobu, zobacz [Tworzenie wielu wystąpień zasobów w Menedżerze zasobów Azure](resource-group-create-multiple.md)
- Aby zobaczyć jak wdrożyć szablonu, który został utworzony, zobacz [Wdrażanie aplikacji za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy.md)

