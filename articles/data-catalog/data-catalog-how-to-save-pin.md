<properties
   pageTitle="Jak zapisać wyszukiwanie i przypiąć danych składniki majątku | Microsoft Azure"
   description="Artykule wyróżnianie możliwości w wykazie danych Azure zapisywania źródła danych i zasoby danych do późniejszego użycia."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/10/2016"
   ms.author="maroche"/>

# <a name="how-to-save-searches-and-pin-data-assets"></a>Jak zapisać wyszukiwanie i przypiąć danych składników majątku

## <a name="introduction"></a>Wprowadzenie

Wykaz danych Microsoft Azure zapewnia możliwości dla odnajdowania źródła danych. Użytkownicy mogą szybkie wyszukiwanie i filtrowanie katalogu do odnajdywania źródeł danych i zrozumienia ich przeznaczeniem, co ułatwia znajdowanie właściwych danych wykonywanego zadania.

Ale co zrobić, jeśli użytkownicy muszą regularnie pracować z tymi samymi danymi? Co z otwieraniem po użytkowników regularnie Współtworzenie wiedzę do samej źródeł danych w wykazie? W takiej sytuacji konieczności wielokrotnie problemu z tym samym wyszukiwań może być nieefektywne — jest tam, gdzie zapisać wyszukiwanie i przypięte dane, które może pomóc w elementy zawartości.

## <a name="saved-searches"></a>Zapisane wyszukiwania

Zapisane wyszukiwanie w wykazie danych Azure to przeznaczone do wielokrotnego użytku, definicja wyszukiwania dla poszczególnych użytkowników. Gdy użytkownik ma zdefiniowane wyszukiwania — w tym wyszukiwane terminy, znaczniki i inne filtry — on można zapisać do późniejszego użycia. Definicja zapisanego wyszukiwania można następnie ponownie uruchom w późniejszym terminie, aby zwrócić jakichkolwiek trwałych danych, odpowiadające jej kryteria wyszukiwania.

### <a name="creating-a-saved-search"></a>Tworzenie zapisanego wyszukiwania

Aby utworzyć zapisanego wyszukiwania, należy najpierw wprowadzić kryteria wyszukiwania, aby można użyć ponownie. Następnie kliknij łącze "Zapisz" w polu "Bieżące wyszukiwanie" portal Azure wykazu danych.

 ![Wybierz pozycję "Zapisz", aby zapisać bieżące ustawienia wyszukiwania](./media/data-catalog-how-to-save-pin/01-save-option.png)

Po wyświetleniu monitu wprowadź nazwę zapisanego wyszukiwania. Wybierz nazwę, która jest, opisową aktywów danych, które zostaną zwrócone przez wyszukiwanie.

 ![Podaj nazwę zapisanego wyszukiwania](./media/data-catalog-how-to-save-pin/02-name.png)

### <a name="managing-saved-searches"></a>Zarządzanie zapisane wyszukiwania

Gdy użytkownik zapisał jednego lub więcej wyszukiwań, opcja "Zapisane wyszukiwania" pojawią się w portalu wykazu danych Azure w polu "Bieżącego wyszukiwania". Po rozwinięciu pełną listę zapisuje wyniki wyszukiwania będą wyświetlane.

 ![Lista Ulubione wyszukiwania](./media/data-catalog-how-to-save-pin/03-list.png)

Wybranie zapisanego wyszukiwania z listy spowoduje, że wyszukiwania do wykonania.

Wybranie menu rozwijane zapewnia zestaw opcji zarządzania:

 ![Opcje służące do zarządzania zapisane wyszukiwania](./media/data-catalog-how-to-save-pin/04-managing.png)

Wybieranie "Zmień nazwę" wyświetli monit do wprowadź nową nazwę dla zapisanego wyszukiwania. Definicja wyszukiwania nie zostanie zmieniony.

Wybieranie "Usunąć" wyświetli monit o potwierdzenie, a następnie będzie Usuń zapisanego wyszukiwania z listy użytkownika.

Wybieranie "Zapisz jako wartość domyślna" oznaczy wybrane zapisać wyszukiwanie jako domyślnego wyszukiwanie użytkownika. Jeśli użytkownik wykona "puste" wyszukiwania na stronie głównej Azure wykazu danych, zostanie wykonana przez użytkownika domyślnego wyszukiwania. Ponadto wyszukiwania oznaczone jako domyślnego pojawi się u góry listy zapisanego wyszukiwania.

### <a name="organizational-saved-searches"></a>Organizacji zapisane wyszukiwania

Każdy użytkownik może zapisać wyszukiwanie na własny użytek. Administratorzy wykaz danych można również zapisać wyszukiwanie wszystkich użytkowników w organizacji. Podczas zapisywania wyszukiwania, Administratorzy są prezentowane opcję Udostępnianie zapisane wyszukiwanie w firmie. Jeśli ta opcja jest zaznaczona, zapisanego wyszukiwania będą uwzględniane na liście wyszukiwań dostępne dla wszystkich użytkowników.

 ![Organizacji zapisane wyszukiwania](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)


## <a name="pinned-data-assets"></a>Przypięty danych składników majątku

Zapisane wyszukiwania zezwolić użytkownikom na zapisywanie i ponowne używanie wyszukiwanie definicji; składniki majątku danych zwróconych przez wyszukiwanie może się zmienić w czasie, gdy zawartość zmiana wykazu. Przypinanie aktywów danych umożliwia użytkownikom jednoznacznie zidentyfikować określonych danych składniki majątku ułatwi ich do programu access bez konieczności za pomocą funkcji wyszukiwania.

Przypinanie zbiór danych jest proste — użytkowników po prostu kliknąć ikonę "numer pin" trwałego danych je dodać do swojej listy przypiętych. Ta ikona jest wyświetlana w rogu kafelka zawartości w widoku kafelków, w lewej kolumny w widoku listy w portal Azure wykazu danych.

![Przypinanie zbiór danych](./media/data-catalog-how-to-save-pin/05-pinning.png)

Unpinning środka trwałego jest tak proste — użytkowników po prostu kliknij ikonę "numer pin" ponownie, aby zmienić ustawienie dla zaznaczonego elementu.

![Unpinning zbiór danych](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="my-assets"></a>"Środki trwałe?"
Stronie głównej portalu Azure wykazu danych zawiera sekcję "Moje zasoby", w której są wyświetlane składniki majątku potrzebnych do bieżącego użytkownika. Ta sekcja zawiera zarówno przypięty składniki majątku i zapisane wyszukiwania.

!["Moje aktywa" na stronie głównej](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Podsumowanie
Azure wykazu danych zawiera funkcje, które ułatwiają użytkownikom odnajdowanie źródeł danych, które są potrzebne, mogą one poświęcać mniej czasu wyszukiwania danych i więcej czasu pracy nad nim. Zapisane wyszukiwania i przypięte dane, które składniki majątku tworzenie na te funkcje podstawowe, aby użytkownicy mogą łatwo zidentyfikować źródeł danych, które będą działać wielokrotnie.
