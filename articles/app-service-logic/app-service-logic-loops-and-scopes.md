<properties
   pageTitle="Logika aplikacje pętli, w zakresach i Debatching | Microsoft Azure"
   description="Logika aplikacji pętli, zakres i debatching pojęcia"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logika aplikacje pętli, w zakresach i Debatching
  
>[AZURE.NOTE] Tej wersji artykuł dotyczy schemat logiczny aplikacji 2016-04-01-Podgląd i później.  Pojęcia są podobne dla starszych schematów, ale zakresy tylko są dostępne dla tego schematu lub nowszy.
  
## <a name="foreach-loop-and-arrays"></a>Pętli ForEach i tablice
  
Logika aplikacje umożliwia pętli zestawu danych i wykonywanie akcji dla każdego elementu.  Można to zrobić za pomocą `foreach` akcji.  W Projektancie można określić, aby dodać dla każdej pętli.  Po zaznaczeniu tabeli, którą chcesz przejść przez, można rozpocząć dodawać akcje.  Obecnie są ograniczone tylko jedną akcję na pętli foreach, ale to ograniczenie zostanie zostać cofnięte w ciągu najbliższych tygodni.  Raz w pętli można rozpocząć Określ, co ma nastąpić w każdej wartości w tablicy.

Jeśli przy użyciu widoku kodu, można określić dla każdej pętli, takich jak poniżej.  Przykład dla każdej pętli, który wysyła wiadomość e-mail dla każdego adresu e-mail, który zawiera "microsoft.com":

```
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` akcji można przejść przez tablice wierszy do 5000.  Iteracji można wykonywać równolegle, więc może być konieczne jest dodanie wiadomości do kolejki, jeśli sterowanie przepływem jest potrzebny.
  
## <a name="until-loop"></a>Poczekaj, aż w pętli
  
  Można wykonywać akcji lub serii akcji do momentu spełnienia warunku.  Najbardziej typowy scenariusz dla tego dzwoni punktu końcowego, aż przejdziesz odpowiedź, którą chcesz odnaleźć.  W Projektancie można określić, aby dodać do pętli.  Po dodaniu Akcje wewnątrz pętli, można ustawić warunku zakończenia, a także pętli ograniczenia.  Istnieje 1 minucie między cykli pętli.
  
  Jeśli przy użyciu widoku kodu, można określić aż pętli, takich jak poniżej.  To jest przykład wywoływania punktu końcowego HTTP, aż treść odpowiedzi ma wartość "Ukończony".  Zakończy, kiedy albo 
  
  * Odpowiedź HTTP ma stan "Ukończone"
  * Prób 1 godziny
  * Zawiera zwracane 100 razy
  
  ```
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed'),
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn i Debatching

Czasami może się pojawić wyzwalacza tablicy elementów, które mają być debatch i uruchomić przepływ pracy dla elementu.  Można to osiągnąć przy użyciu `spliton` polecenia.  Domyślnie, jeśli do swagger wyzwalacza określa ładunku, która jest tablicą `spliton` , zostaną dodane i rozpocząć Uruchom na element.  SplitOn można dodawać tylko do wyzwalacza.  Można to ręcznie skonfigurowane lub zastąpiony w widoku kodu definicji.  Obecnie można debatch SplitOn tablice do 5000 elementów.  Nie mogą mieć `spliton` , a także zaimplementowania deseniem odpowiedzi synchronicznego.  Każdy przepływ pracy o nazwie, która ma `response` akcji oprócz `spliton` uruchomisz asyncronously i wysyłanie natychmiastowego `202 Accepted` odpowiedzi.  

SplitOn można określić w widoku Kod, jak w poniższym przykładzie.  Ten recieves tablicy elementów i debatches w każdym wierszu.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequency": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Zakresy

Istnieje możliwość grupowania serii akcji współpraca przy użyciu zakresu.  To jest szczególnie przydatne w przypadku wykonywania obsługi wyjątków.  W Projektancie można dodać nowy zakres i rozpocząć dodawanie akcji wewnątrz go.  Można zdefiniować zakresy w widoku Kod podobnej do następującej:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
