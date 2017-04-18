<properties
   pageTitle="Używanie odbiornik HTTP i łącznika w logiczny aplikacji | Microsoft Azure aplikacji usługi "
   description="Jak utworzyć i skonfigurować odbiornik HTTP i aplikacji łącznika lub interfejsu API akcji HTTP i używać go w aplikacji dla logiki Azure aplikacji usługi"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/31/2016"
   ms.author="prkumar"/>


# <a name="get-started-with-the-http-listener-and-http-action-and-add-it-to-your-logic-app"></a>Rozpoczynanie pracy z odbiornik HTTP i akcji HTTP i dodać go do aplikacji warunków logicznych

> [AZURE.NOTE]Firma Microsoft się zakończą pomocy technicznej dla tego łącznika, ponieważ działanie jest teraz domyślnie dostępne **ręcznego wyzwalacza** podczas tworzenia nowych aplikacji logika.  Zaleca się uaktualnienie wszystkich aplikacji logika używających tej łącznik.  
> Tą wersją artykułu dotyczy wersji schematu 2014-12-01-podgląd aplikacji logicznych.

Podłącz bezpośrednio do zasoby HTTP, aby odsłuchać żądania HTTP i konfigurowanie żądania HTTP sieci web. Istnieje kilka scenariuszy, w których może być konieczne Praca z bezpośredniego połączenia HTTP, w tym:

1.  Aby opracować logiki aplikacji, która obsługuje sieci web lub interakcyjnych fronton użytkownik urządzenia przenośnego.
2.  Pobieranie i przetwarzanie danych z usługi sieci web, która nie ma w nowym polu łącznika.
3.  Wykonywanie akcji, które są już dostępne jako usługi sieci web, ale nie są dostępne jako aplikacji interfejsu API.

W przypadku następujących scenariuszy dostępne są dwie opcje:

1. **Odbiornik HTTP**: ten łącznik działa jako wyzwalacz i odbiera żądania HTTP na skonfigurowane punktu końcowego. Po odebraniu połączenie dla punktu końcowego skonfigurowanego wyzwalane nowego wystąpienia przepływu i przekazuje otrzymanych w wezwaniu przepływu dla przetwarzania danych. Może być również skonfigurowane do automatycznego odpowiadania na przychodzące żądanie podczas przepływu pracy lub pozwalają utworzyć odpowiedź oparte na wykonanie przepływu i wysłać odpowiedź do wywołującego.
2. **Akcja HTTP**: to umożliwia konfigurowanie żądania sieci web do dowolnego punktu końcowego dostępne w Internecie, ponownie otrzymuje odpowiedź i udostępnia dodatkowe akcje w procesie korzystać z.

Aplikacje logika może spowodować na podstawie różnych źródeł danych i łączników oferty gromadzenia i przetwarzania danych w ramach przepływu. Możesz dodać łącznik HTTP do przepływu pracy i proces danych biznesowych w ramach tego przepływu pracy w aplikacji logika. 

## <a name="creating-an-http-listener-for-your-logic-app"></a>Tworzenie odbiornik HTTP dla aplikacji logika
Łącznik można utworzyć w aplikacji logicznych lub być tworzone bezpośrednio z usługi Azure Marketplace. Aby utworzyć łącznik z witryny Marketplace:  

1. W startboard Azure wybierz pozycję **Marketplace**.
2. Wyszukiwanie "HTTP", zaznacz odbiornik HTTP, a następnie wybierz pozycję **Utwórz**.
3.  Konfigurowanie odbiornika HTTP w następujący sposób:  
![][1]

4.  Podczas konfigurowania ustawień pakietu, zobaczysz następującą opcję na czy odbiornika należy odpowiedzieć automatycznie lub wymagają, aby wysyłał odpowiedzi jawne. Ustaw wartość to **FAŁSZ,** Aby wysłać swojej odpowiedzi:  
![][2]

5.  Kliknij **przycisk OK** , aby utworzyć.
6.  Po utworzeniu wystąpienia aplikacji interfejsu API Otwórz ustawienia, aby skonfigurować zabezpieczenia. Odbiornik HTTP obsługuje obecnie uwierzytelniania podstawowego. Można to skonfigurować przy użyciu opcji zabezpieczeń podczas otwierania odbiornika protokołu HTTP:  
![][3]
  
    **Znany problem** Ustawienia zabezpieczeń *Pokaż "Brak", jako wartość domyślną, jednak jest nieokreślone. Podstawowy i z powrotem na brak przed zapisaniem upewnij się, że odbiornika protokołu HTTP jest skonfigurowane poprawnie, musisz zmienić ustawienie.*  

7. Ponadto ustawienia zabezpieczeń aplikacji interfejsu API do publicznego (anonimowy), aby umożliwić klientom zewnętrznym uzyskać dostęp do punktu końcowego. To ustawienie jest dostępne w obszarze "wszystkie ustawienia > Ustawienia aplikacji" aplikacji interfejsu API odbiornika HTTP:![][10]

Po wykonaniu tej operacji teraz można utworzyć aplikację logika, aby korzystać z odbiornika protokołu HTTP.

## <a name="using-the-http-listener-in-your-logic-app"></a>Za pomocą odbiornika protokołu HTTP w aplikacji warunków logicznych
Po utworzeniu aplikacji interfejsu API teraz umożliwiają odbiornika protokołu HTTP jako wyzwalacz aplikacji logicznych. Aby to zrobić, musisz:

4.  Tworzenie nowej aplikacji logicznych.
5.  Otwórz "Wyzwalacze i akcje" Otwórz projektanta logiki aplikacji i konfigurowanie usługi przepływu. Odbiornik HTTP znajduje się w galerii. Wybierz go.
6.  Teraz można ustawić metodę HTTP i względny adres URL, na którym jest wymagane odbiornika wyzwalać przepływu:  
![][4]  
![][5]

7.  Aby uzyskać pełną identyfikatora URI, kliknij dwukrotnie odbiornik HTTP, aby wyświetlić jej ustawienia konfiguracji i skopiuj adres URL aplikacji interfejsu API "hosta":  
![][6]
8.  Teraz można dane uzyskane w żądania HTTP w innych akcji w przepływie w następujący sposób:  
![][7]  
![][8]
9.  Na koniec aby wysłać odpowiedź, Dodaj innego odbiornik HTTP i wybierz akcję, wysłać odpowiedź HTTP. Ustaw identyfikator żądania IdentyfikatorŻądania uzyskanego z odbiornika protokołu HTTP i wypełnić treść odpowiedzi i stan HTTP, który chcesz przywrócić ponownie:  
![][9]

## <a name="using-the-http-action"></a>Przy użyciu akcji HTTP
Akcja HTTP obsługiwane przez aplikacje logiki i nie wymaga aplikacji interfejsu API do utworzenia pierwszego, aby można było jej używać. Można wstawiać akcji HTTP w dowolnym momencie w logiczny aplikacji i wybierz pozycję identyfikator URI, nagłówki i treści na połączenie.
Akcja HTTP obsługuje wiele opcji zabezpieczeń po stronie klienta. Zobacz [Opcje zabezpieczeń po stronie klienta](../scheduler/scheduler-outbound-authentication.md).

Wynik działania protokołu HTTP jest nagłówki i treści, które mogą być używane dodatkowo rzeki w procesie podobnie jak zużyte wynik innych działań i łączników.

## <a name="do-more-with-your-connector"></a>Więcej możliwości korzystania z tego łącznika
Teraz, gdy łącznik jest tworzony, możesz dodać go do przepływu pracy firm przy użyciu aplikacji logicznych. Zobacz [Co to są aplikacje logiczny?](app-service-logic-what-are-logic-apps.md).

Wyświetlanie odwołania Swagger interfejsu API usługi REST [łączników](http://go.microsoft.com/fwlink/p/?LinkId=529766)i interfejsu API aplikacje odwołania.

Można także przeglądać wydajności statystyki i kontroli zabezpieczeń do łącznika. Zobacz [Monitorowanie wbudowanych łączników i aplikacjami interfejsu API i zarządzanie nimi](app-service-logic-monitor-your-connectors.md).

> [AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure logiki aplikacji przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj logiczny aplikacji](https://tryappservice.azure.com/?appservice=logic). Możesz od razu utworzyć aplikacji logika krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png
