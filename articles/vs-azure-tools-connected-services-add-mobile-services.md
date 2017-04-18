<properties 
   pageTitle="Dodawanie usług Mobile za pomocą połączenia usług w programie Visual Studio | Microsoft Azure"
   description="Dodawanie usług Mobile przy użyciu okna dialogowego Visual Studio Dodaj połączenie usługi"
   services="visual-studio-online"
   documentationCenter="na"
   authors="mlhoop"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="12/16/2015"
   ms.author="mlearned" />

# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Dodawanie usług Mobile za pomocą programu Visual Studio połączenia usług

Przy użyciu programu Visual Studio 2015 r. można nawiązać usługi Mobile Azure za pomocą okna dialogowego **Dodawanie usługi połączone** . Można połączyć z dowolnej aplikacji klienckich C#, dowolnej aplikacji JavaScript lub aplikacji Cordova między platformami. Po nawiązaniu połączenia można tworzyć i uzyskać dostęp do danych, tworzyć niestandardowe interfejsy API i zaplanowanych zadań lub dodać Obsługa powiadomień wypychanych.  Operacja usługi połączone dodaje wszystkie odpowiednie odwołania i kod połączenia. Możesz także korzystać z wbudowanych funkcji uwierzytelniania za pomocą różnych tożsamości popularnych programów, takich jak Azure AD Facebook, Twitter i Accounts firmy Microsoft.

## <a name="supported-project-types"></a>Typy obsługiwanego projektów

>[AZURE.NOTE] W Visual Studio 2015 r. Dodawanie usługi Azure Mobile do projektów uniwersalny systemu Windows (Windows 10) za pomocą okna dialogowego Dodawanie usługami nie jest obsługiwane. Możesz dodać usługi Azure Mobile instalując odpowiednie pakiety za pomocą Menedżera pakietów NuGet projektu.

Okno dialogowe połączenie usługi umożliwia nawiązywanie połączenia z usługi Azure Mobile w następujących typów projektu.

- Projekty ze Sklepu Windows 8.1 .NET, telefon i uniwersalny aplikacji

- Projekty ze Sklepu Windows 8.1 JavaScript, telefon i uniwersalny aplikacji

- Projekty utworzone przy użyciu programu Visual Studio Tools dla Apache Cordova


## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Łączenie się z usługami Mobile Azure za pomocą okna dialogowego Dodawanie usługi połączone

1. Upewnij się, że masz konto usługi Azure. Jeśli nie masz konta usługi Azure, można Załóż [bezpłatnej wersji próbnej](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Otwórz okno dialogowe **Dodawanie usługi połączone** .
 - W przypadku aplikacji .NET Otwórz projekt w programie Visual Studio, otwórz menu kontekstowego dla węzła **odwołania** w Eksploratorze rozwiązań, a następnie wybierz **Dodawanie usługi połączone**
 
        ![Connecting to Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)

 - W przypadku projektów aplikacji Apache Cordova Otwórz projekt w programie Visual Studio, otwórz menu kontekstowe węzeł projektu w Eksploratorze rozwiązań, a następnie wybierz **Dodawanie usługi połączone**.

1. W oknie dialogowym **Dodawanie usługi połączone** wybierz **Usługi Azure Mobile**, a następnie kliknij przycisk **Konfiguruj** . Może zostać wyświetlony monit o zalogowanie się do Azure, jeśli jeszcze tego nie zrobiono.

    ![Dodawanie Azure usługi mobilnej](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)

1. W oknie dialogowym **Usługi Azure Mobile** wybierz istniejące usługi mobilnej, jeśli istnieje. Jeśli potrzebujesz utworzyć nową usługę Azure urządzeń przenośnych, należy wykonać procedurę poniżej, aby to zrobić. W przeciwnym razie przejdź do następnego kroku.

    Aby utworzyć nowe konto usługi mobilnej:
    1. Wybierz łącze **Utwórz usługę **, w dolnej części okna dialogowego.
        ![Dodawanie nowych urządzeń przenośnych usługi połączone](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)




    2. W oknie dialogowym **Tworzenie usługi mobilnej** możesz wybrać z listy rozwijanej **obsługi** języka JavaScript usług mobilnych wewnętrznej bazy danych lub Usługa mobilna wewnętrznej bazy danych programu .NET. 
  
        ![Tworzenie usług mobilnych](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)

        Usługa wewnętrznej bazy danych języka JavaScript jest prostych i zaawansowanych. Jeśli tworzysz JavaScript usług mobilnych wewnętrznej bazy danych, kod JavaScript po stronie serwera są przechowywane w chmurze, ale można edytować skrypty server za pomocą Eksploratora serwera lub portalu zarządzania Azure. 

        Usługa mobilna wewnętrznej bazy danych programu .NET zapewnia możliwości i elastyczność struktury obiektu i interfejsu API sieci Web. Jeśli tworzysz Usługa mobilna wewnętrznej bazy danych programu .NET projektu jest tworzony i dodane do rozwiązania. 

    1. Wybierz **Region** , w którym chcesz usług mobilnych, a następnie wprowadź nazwę użytkownika i hasło dla serwera.
 
    1. Po wprowadzeniu wszystkich wymaganych informacji, wybierz przycisk **Utwórz** , aby utworzyć z usługi mobilnej.
    2. Nowe Usługa mobilna powinien być wyświetlany na liście usługi w oknie dialogowym **Usługi Azure Mobile** . Wybierz nową usługę urządzeń przenośnych na liście, a następnie wybierz przycisk **Dodaj** , aby dodać usługę do projektu.
    

1. Przejrzyj stronie początkowej wyświetlonym i Dowiedz się, jak modyfikacji projektu. Wprowadzenie do strony pojawi się w przeglądarce po dodaniu usługi połączone. Możesz przejrzeć Sugerowane następne kroki i przykłady kodu lub przejść na stronę co się stało z odwołaniami, które zostały dodane do projektu i jak pliki kodu i konfiguracji zostały zmodyfikowane.

1. Za pomocą przykłady kodu jako wskazówek, Rozpocznij pisanie kodu w celu uzyskania dostępu do urządzeń przenośnych usługi!

## <a name="how-your-project-is-modified"></a>Jak zmienić projektu

Jak Visual Studio modyfikuje projektu zależy od typu projektu. W przypadku C# aplikacji klienta zobacz [co się stało z — C# projektów](http://go.microsoft.com/fwlink/p/?LinkId=513119). W przypadku aplikacji klienckich JavaScript zobacz [co się stało z — projekty języka JavaScript](http://go.microsoft.com/fwlink/p/?LinkId=513120). W przypadku aplikacji Cordova Zobacz, [co się stało z — projekty Cordova](http://go.microsoft.com/fwlink/p/?LinkId=513116).


##<a name="next-steps"></a>Następne kroki

Zadawanie pytań i uzyskiwanie pomocy: 

 - [Forum w witrynie MSDN: Azure usług mobilnych](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)

 - [Azure usług mobilnych w blogu zespołu Microsoft Azure](https://azure.microsoft.com/blog/topics/mobile/)

 - [Azure usług mobilnych na azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)

 - [Dokumentację dotyczącą usług mobilnych Azure azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)



