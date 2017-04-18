<properties 
   pageTitle="Można opracowywać operatory zdefiniowane przez użytkownika U SQL Azure danych Lake analizy zadań | Azure" 
   description="Dowiedz się, jak można opracowywać operatory zdefiniowane przez użytkownika używane i ponownie wykorzystywać analizy Lake danych zadań. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="develop-u-sql-user-defined-operators-for-azure-data-lake-analytics-jobs"></a>Można opracowywać operatory zdefiniowane przez użytkownika U SQL Azure danych Lake analizy zadań

Dowiedz się, jak można opracowywać operatory zdefiniowane przez użytkownika używane i ponownie wykorzystywać analizy Lake danych zadań. Będzie można opracowywać niestandardowe operator przekonwertować nazwy kraju.

##<a name="prerequisites"></a>Wymagania wstępne

- Visual Studio 2015, Visual Studio 2013 aktualizowanie 4 lub program Visual Studio 2012 ze zainstalowany program Visual C++ 
- Microsoft Azure SDK dla .NET wersji 2.5 lub nowszej.  Zainstaluj go za pomocą Instalatora platformy sieci Web.
- Konto analizy Lake danych.  Zobacz [Wprowadzenie do analiz Lake danych Azure za pomocą Azure Portal](data-lake-analytics-get-started-portal.md).
- Przeglądanie samouczek [wprowadzenie Azure danych Lake analizy U SQL Studio](data-lake-analytics-u-sql-get-started.md) .
- Nawiązywanie połączenia z Azure, zobacz [Rozpoczynanie pracy z Azure danych Lake analizy U SQL Studio](data-lake-analytics-u-sql-get-started.md#connect-to-azure). 
- Przekazywanie danych źródłowych, zobacz artykuł [Wprowadzenie do Azure danych Lake analizy U SQL Studio](data-lake-analytics-u-sql-get-started.md#upload-source-data-files). 

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Definiowanie i używanie ich w języku SQL U operatora zdefiniowane przez użytkownika

**Aby utworzyć i przesłać zadanie U SQL** 

1. W menu **plik** kliknij polecenie **Nowy**, a następnie kliknij **Projekt**.
2. Wybierz typ **Projektu U-SQL** .

    ![Nowy projekt Visual Studio U SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Kliknij **przycisk OK**. Program Visual studio powoduje rozwiązanie z pliku Script.usql.
4. W **Eksploratorze rozwiązań**rozwiń Script.usql, a następnie kliknij dwukrotnie pozycję **Script.usql.cs**.
5. Wklej następujący kod do pliku:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;
        
        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };
        
                public override IRow Process(IRow input, IUpdatableRow output)
                {
        
                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");
        
                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);
        
                    return output.AsReadOnly();
                }
            }
        }

5. Otwórz Script.usql i wklej następujący skrypt U SQL:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);
        
        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    
        
        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);

6. Korzystając z **Eksploratora rozwiązań**kliknij prawym przyciskiem myszy **Script.usql**, a następnie kliknij **Tworzenia skryptów**.
6. Korzystając z **Eksploratora rozwiązań**kliknij prawym przyciskiem myszy **Script.usql**, a następnie kliknij **Przesyłanie skryptu**.
7. Jeśli nie możesz połączyć się subskrypcji Azure, będzie monitu o podanie poświadczeń konta Azure.
7. Kliknij przycisk **Prześlij**. Wyniki przesyłania i łącze do zadania są dostępne w oknie wyniki po zakończeniu przekazywania.
8. Należy kliknąć przycisk Odśwież, aby wyświetlić najnowsze stan zadania i odświeżanie ekranu.

**Aby wyświetlić wynik zadania**

1. Korzystając z **Eksploratora serwera**rozwiń **Azure**, rozwiń węzeł **Analizy Lake danych**, rozwiń konta analizy Lake danych, rozwiń **Kont miejsca do magazynowania**, kliknij prawym przyciskiem myszy magazyn domyślny, a następnie kliknij **Eksploratora**. 
2. Rozwiń próbki, rozwiń wyjściowe, a następnie kliknij dwukrotnie **Drivers.csv**.


##<a name="see-also"></a>Zobacz też

- [Wprowadzenie do analizy Lake danych przy użyciu programu PowerShell](data-lake-analytics-get-started-powershell.md)
- [Wprowadzenie do analizy Lake danych za pomocą portalu Azure](data-lake-analytics-get-started-portal.md)
- [Korzystanie z narzędzi Lake danych dla programu Visual Studio do tworzenia aplikacji U SQL](data-lake-analytics-data-lake-tools-get-started.md)
