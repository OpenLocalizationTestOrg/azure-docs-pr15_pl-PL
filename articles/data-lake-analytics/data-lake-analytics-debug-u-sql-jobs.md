<properties 
   pageTitle="Debugowanie U SQL zadania | Microsoft Azure" 
   description="Dowiedz się, jak debugowanie U SQL wierzchołka nie powiodło się przy użyciu programu Visual Studio. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>



#<a name="debug-c-code-in-u-sql-for-data-lake-analytics-jobs"></a>Debugowanie kodu C# w języku SQL U dla zadań analizy Lake danych 

Dowiedz się, jak za pomocą narzędzia Visual Studio Lake danych Azure debugowanie zadania U SQL zakończone niepowodzeniem z powodu usterek wewnątrz kodu użytkownika. 

Narzędzie Visual Studio umożliwia pobieranie skompilowany kod i dane niezbędne wierzchołek z klaster śledzenia i debugowania zadania nie powiodło się.

Systemy duży danych zazwyczaj zawierają rozszerzania modelu za pośrednictwem języków, takich jak Java, C#, Python itp. Te systemy zapewniają ograniczone środowisko uruchomieniowe debugowanie informacji, które utrudnia debugowanie błędy wykonania w kodzie niestandardowym. Najnowsze narzędzia Visual Studio zawiera funkcję o nazwie "Nie powiodło się debugowanie wierzchołek". Korzystając z tej funkcji, możesz pobrać dane środowisko uruchomieniowe z platformy Azure do lokalnych roboczej tak, aby umożliwić debugowanie nie powiodło się C# kodu niestandardowego za pomocą samego obsługi i dokładnie danych wejściowych z chmury.  Po poprawieniu problemy, można uruchomić ponownie zmieniony kod platformy Azure na karcie Narzędzia.

Dla prezentacji wideo tej funkcji zobacz [Debugowanie niestandardowy kod do analizy Lake danych Azure](https://mix.office.com/watch/1bt17ibztohcb).

>[AZURE.NOTE] Visual Studio może się zawieszać lub ulec awarii, jeśli nie masz następujące uaktualnienia dwa okna: [Microsoft Visual C++ 2015 pakiet redystrybucyjny aktualizacji 2](https://www.microsoft.com/download/details.aspx?id=51682), [Universal C Runtime dla systemu Windows](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0).


##<a name="prerequisites"></a>Wymagania wstępne
-   Masz przeszli artykuł [rozpocząć pracę](data-lake-analytics-data-lake-tools-get-started.md) .

## <a name="create-and-configure-debug-projects"></a>Tworzenie i konfigurowanie debugowania projektów

Po otwarciu zadania zakończonego niepowodzeniem w narzędziu programu Visual Studio Lake danych zostanie wyświetlony alert. Informacje o błędach szczegółowe będą wyświetlane na karcie błędu i alertów żółtym pasku u góry okna. 

![Azure analizy Lake danych U-SQL debugowania programu visual studio pobierania wierzchołka](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**Aby pobrać wierzchołek i utworzyć rozwiązanie debugowania**

1.  Otwórz zadanie U SQL nie powiodło się w programie Visual Studio.
2.  Kliknij przycisk **Pobierz** do pobrania wszystkich wymaganych zasobów i wejściowych strumieni. W przypadku pobierania danych, kliknij przycisk **Ponów próbę** .
3.  Po zakończeniu pobierania, aby utworzyć projekt lokalne debugowania, kliknij przycisk **Otwórz** . Rozwiązanie Visual Studio o nazwie **VertexDebug** pusty projekt o nazwie **LocalVertexHost** zostanie utworzona.

Operatory zdefiniowane przez użytkownika są używane w U SQL kodu źródłowego (Script.usql.cs), należy utworzyć projekt klasy biblioteki C# z kodem operatory zdefiniowane przez użytkownika i dołączyć projektu rozwiązania VertexDebug.

Jeśli zarejestrowano zestawów dll do analizy Lake danych bazy danych, należy dodać kod źródłowy zestawów rozwiązanie VertexDebug.
 
Jeśli utworzono oddzielnym C# Biblioteka klas kod U SQL i zestawy zarejestrowanych dll do bazy danych Lake analizy danych, musisz dodać C# projekcie źródłowym zestawów rozwiązanie VertexDebug.

W niektórych przypadkach rzadkich w U SQL kodu źródłowego pliku (Script.usql.cs) w oryginalnym rozwiązanie używać operatorów zdefiniowanych przez użytkownika. Jeśli chcesz nadać mu pracy, należy utworzyć bibliotekę C# zawierającego kod źródłowy i Zmień nazwę zestawu do tego, które zostało zarejestrowane w klastrze. Możesz uzyskać nazwy zestawu zarejestrowane w grupie, zaznaczając pole wyboru skrypt, który został uruchomiony w klastrze. Można to zrobić, otwierając zadania U SQL, a następnie kliknij przycisk "skrypt" w panelu zadania. 

**Aby skonfigurować rozwiązanie**

1.  Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy nowo utworzonej projektu C#, a następnie kliknij **Właściwości**.
2.  Ustawianie ścieżki wyjściowej jako projektu LocalVertexHost pracy ścieżkę katalogu. Możesz uzyskać ścieżkę katalogu pracy projektu LocalVertexHost za pośrednictwem właściwości LocalVertexHost.
3.  Tworzenie projektu C#, aby umieścić plik .pdb do programu project LocalVertexHost katalog roboczy lub możesz ręcznie skopiuj plik .pdb do tego folderu.
4.  W przypadku **Ustawienia wyjątków**Sprawdź typowe wyjątki środowisko uruchomieniowe język:

![Ustawienie programu visual studio debugowania w usłudze Azure analizy Lake danych U-SQL](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##<a name="debug-the-job"></a>Debugowanie zadania

Po utworzono rozwiązanie debugowania, pobierając wierzchołek i skonfigurować środowisko programu, możesz rozpocząć debugowania kodu U SQL.

1.  Korzystając z Eksploratora rozwiązanie kliknij prawym przyciskiem myszy nowo utworzonej projektu **LocalVertexHost** , wskaż **Debugowanie**, a następnie kliknij **Uruchom nowe wystąpienie**. LocalVertexHost należy ustawić jako projekt startowy. Może zostać wyświetlony komunikat następujących po raz pierwszy, który można zignorować. Może potrwać do jednej minuty, aby przejść do ekranu debugowania.
 
    ![Ostrzeżenie programu visual studio debugowania w usłudze Azure analizy Lake danych U-SQL](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.  Używanie programu Visual Studio podstawie debugowania środowiska (czujki, zmienne itp.) do rozwiązywania problemów. 
5.  Po zidentyfikowaniu problemu naprawić kod, a następnie ponownie projekt C# przed sprawdzeniem jej ponownie, dopóki nie są rozwiązane wszystkie problemy. Po debugowania została pomyślnie ukończona, okno dane wyjściowe przedstawiający komunikat 

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##<a name="resubmit-the-job"></a>Prześlij ponownie to zadanie

Po zakończeniu debugowania kodu U SQL można przesłać ponownie to zadanie nie powiodło się.

1. Rejestrowanie nowego zestawów dll do bazy danych programu ADLA.

    1.  Eksploratora Eksploratora-chmury serwera w narzędzia Visual Studio Lake danych rozwiń węzeł **bazy danych** 
    2.  Kliknij prawym przyciskiem myszy zestawów zestawów rejestru. 
    3.  Zarejestruj się w nowych zestawów dll ADLA bazy danych.
 
2.  Lub skopiuj kod C# do script.usql.cs - C# kodu źródłowego pliku.
3.  Prześlij ponownie zadania.

##<a name="next-steps"></a>Następne kroki

- [Samouczek: Wprowadzenie do języka Azure danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md)
- [Samouczek: projektowania skryptów U SQL przy użyciu narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Można opracowywać operatory zdefiniowane przez użytkownika U SQL Azure danych Lake analizy zadań](data-lake-analytics-u-sql-develop-user-defined-operators.md)

