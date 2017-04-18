<properties 
   pageTitle="Monitorowanie użycia i statystyki w usłudze Azure wyszukiwania | Microsoft Azure | Usługa wyszukiwania hostowanej chmury" 
   description="Śledzenie zasobów zużycie i indeks rozmiar Azure wyszukiwania, usługi wyszukiwania w chmurze obsługiwane w programie Microsoft Azure." 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="jhubbard" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="05/17/2016"
   ms.author="heidist"/>

# <a name="monitor-usage-and-statistics-in-an-azure-search-service"></a>Monitorowanie użycia oraz statystyki w usłudze Azure wyszukiwania

Śledzenie indeksy i rozmiar dokumentu może ułatwić wyprzedzeniem dostosować wydajność przed naciśnięcie górną granicę, które zostaną utworzone dla tej usługi. 

Monitorowanie użycia zasobów, liczniki i statystyki dotyczące usługi łatwo są wyświetlane w [Azure Portal](https://portal.azure.com), ale można również uzyskać informacje programowy Jeśli tworzysz narzędzia administracyjnego usługa niestandardowa. W tym artykule opisano, jak instrukcje dotyczące obu metod.

Za pomocą nowej funkcji analizy ruchu wyszukiwanie dla wniosków do działania na poziomie indeksu. Odwiedź stronę [Analizy ruchu wyszukiwanie dla wyszukiwania Azure](search-traffic-analytics.md) rozpocząć pracę.

##<a name="view-counts-and-metrics-in-the-portal"></a>Wyświetlanie liczniki i wskaźniki w portalu 

1. Zaloguj się do [portalu Azure](https://portal.azure.com). 

2. Otwórz pulpit nawigacyjny usługi usługi Azure wyszukiwania. Kafelki w usłudze można znaleźć na stronie głównej, lub można przeglądać z usługą z Przeglądaj w JumpBar. Aby uzyskać instrukcje krok po kroku, zobacz temat [Tworzenie usługi](search-create-service-portal.md) .

Sekcja zastosowania zawiera licznik, informujący o tym, jaka część dostępne zasoby są obecnie używane.

  ![][1]

Odwoływanie, że usług udostępnionych zawiera więcej niż jednej replice i partycje każdego. Ponadto w całości lub 50 MB danych obsługuje tylko 10 000 dokumentów, zależnie od jest pierwsze.

##<a name="get-index-statistics-using-the-rest-api"></a>Uzyskiwanie statystyki indeksu przy użyciu interfejsu API usługi REST

Zarówno interfejsu API usługi REST wyszukiwania Azure i .NET SDK umożliwiają programowy dostęp do usług metryki.  Jeśli korzystasz z [indeksatory](https://msdn.microsoft.com/library/azure/dn946891.aspx) załadować indeksu z bazy danych SQL Azure lub DocumentDB, interfejsu API jest dostępna uzyskać numery, które są wymagane. 

  + [Uzyskiwanie statystyki indeksu](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [Liczba dokumentów](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [Pobierz stan indeksowania](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## <a name="next-steps"></a>Następne kroki

Przejrzyj [ograniczenia i pojemnością](search-limits-quotas-capacity.md) ustalenie kombinacja partycje i repliki potrzebne, jeśli istniejący pojemność jest niewystarczająca. 

Aby uzyskać więcej informacji na temat administracji usługi, odwiedź stronę [Zarządzanie usługi wyszukiwania w programie Microsoft Azure](search-manage.md) .

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG




 
