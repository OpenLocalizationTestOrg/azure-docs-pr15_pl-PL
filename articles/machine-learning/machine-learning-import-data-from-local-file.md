<properties
    pageTitle="Importowanie danych do komputera nauki Studio z pliku lokalnego | Microsoft Azure"
    description="Jak zaimportować dane szkolenia Azure maszynowego uczenia Studio z pliku lokalnego."
    keywords="Importowanie danych, format danych, typów danych, źródeł danych, dane szkolenia"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye;bradsev" />


# <a name="import-your-training-data-into-azure-machine-learning-studio-from-a-local-file"></a>Zaimportuj dane szkolenia do Azure maszynowego uczenia Studio z plikiem lokalnym

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]


Aby użyć własnych danych w Studio nauki komputera, możesz przekazać plik danych związaną z wcześniejszym przygotowaniem z na lokalnym dysku twardym, aby utworzyć moduł zestawu danych w obszarze roboczym. 


## <a name="import-data-from-a-local-file"></a>Importowanie danych z pliku lokalnego

Można importować dane z lokalnym dysku twardym, wykonując następujące czynności:

1. Kliknij pozycję **+ Nowe** w dolnej części okna Studio nauki komputera.
2. Wybierz **zestaw danych** i **od pliku lokalnego**.
3. W oknie dialogowym **Przekaż nowy zestaw danych** przejdź do pliku, który chcesz przekazać
4. Wprowadź nazwę, wskazują typ danych i opcjonalnie wpisz opis. Zaleca się opis — umożliwia rejestrowanie wszelkie cechy o dane, które należy pamiętać podczas korzystania z danych w przyszłości.
5. Pole wyboru **to jest nowa wersja istniejącego zestawu danych** umożliwia aktualizowanie istniejącego zestawu danych przy użyciu nowych danych. Po prostu kliknij to pole wyboru, a następnie wprowadź nazwę istniejącego zestawu danych.

Podczas przekazywania zostanie wyświetlony komunikat jest przekazywanego pliku. Przekazywanie czasu zależy od rozmiaru danych i szybkości połączenia z usługą.
Jeśli wiesz, że plik będzie trwać bardzo długo, można wykonać inne czynności wewnątrz maszynowego uczenia Studio, podczas oczekiwania. Jednak zamknięcie przeglądarki spowoduje przekazanie danych nie powiedzie się.

Po przesłaniu dane są przechowywane w module zestawu danych i są dostępne dla każdego doświadczenia w obszarze roboczym.
Podczas edytowania doświadczenia, możesz znaleźć zestawy danych utworzony na liście **Moje zestawy danych** na liście **Zestawów danych zapisanych** w palecie modułu. Można przeciągnij i upuść zestawu danych na kanwę doświadczenia, jeśli chcesz użyć zestawu danych dla dalszej analizy i nauka komputera.



