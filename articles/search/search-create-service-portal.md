<properties
    pageTitle="Tworzenie usługi Azure wyszukiwania przy użyciu Azure Portal | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Dowiedz się, jak inicjować obsługę usługi Azure wyszukiwania przy użyciu Azure Portal."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Tworzenie usługi Azure wyszukiwania przy użyciu Azure Portal

Ten przewodnik przeprowadzi Cię przez proces tworzenia (lub inicjowania obsługi administracyjnej) usługa wyszukiwania Azure za pomocą [Azure Portal](https://portal.azure.com/).

Ten przewodnik założono, że już masz subskrypcji usługi Azure i zalogować się do portalu Azure.

## <a name="find-azure-search-in-the-azure-portal"></a>Znajdowanie Azure wyszukiwania w portalu Azure
1. Przejdź do [Azure Portal](https://portal.azure.com/) i zaloguj się.
1. Kliknij znak plus ("+") w lewym górnym rogu.
2. Zaznacz **dane + miejsca do magazynowania**.
3. Wybierz pozycję **Azure wyszukiwania**.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Wybierz nazwę usługi i adres URL punktu końcowego usługi
1. Nazwę usługi będzie część adresu URL punktu końcowego usługi Azure wyszukiwania z którą wprowadzisz połączenia interfejsu API i zarządzanie nimi za pomocą usługi wyszukiwania.
2. Wpisz nazwę usługi w polu **adres URL** . Nazwa usługi:
  * musi zawierać tylko małe litery, cyfry lub kreski ("-")
  * Nie można użyć łącznika ("-") jako pierwsze 2 znaków lub ostatniego pojedynczy znak
  * nie mogą zawierać następujących po sobie kresek ("-")
  * ograniczono między 2 i 60 znaków


## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Wybierz subskrypcję, gdzie powoduje, że usługi
Jeśli masz więcej niż jedną subskrypcję, możesz wybrać, które z nich będzie zawierać tej usługi Azure wyszukiwania.

## <a name="select-a-resource-group-for-your-service"></a>Wybierz grupę zasobów dla tej usługi
Tworzenie nowej grupy zasobów, lub wybierz istniejący. Grupa zasobów to zbiór usług Azure i zasoby, które są używane razem. Na przykład jeśli korzystasz z indeksowania bazy danych SQL Azure wyszukiwania, następnie obie te usługi należy część tej samej grupy zasobów.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Wybierz lokalizację, w której zostanie umieszczona usługi
Jako usługa Azure Azure wyszukiwania jest dostępna dla obsługiwać w centrach danych na całym świecie. Uwaga tej [ceny mogą się różnić](https://azure.microsoft.com/pricing/details/search/) się geograficznych.

## <a name="select-your-pricing-tier"></a>Wybierz pozycję z poziomu cen
[Azure wyszukiwania jest dostępne w wielu poziomów cennik](https://azure.microsoft.com/pricing/details/search/): bezpłatne, podstawowe lub standardowy. Każdą ma własny [Wydajność i ograniczenia](search-limits-quotas-capacity.md). Zobacz [Wybierz cennik warstwa lub SKU](search-sku-tier.md) orientacji.

W tym przypadku możemy wybranego warstwie standardowy z naszych usług.

## <a name="select-the-create-button-to-provision-your-service"></a>Wybierz przycisk "Utwórz", aby zapewnić usługi

![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>Skalowanie usługi

Po tej usługi jest obsługi administracyjnej, można skalować je stosownie do potrzeb. Jeśli wybrano standardowy warstwa tej usługi Azure wyszukiwania, można skalować usługi dwuwymiarowej: repliki i partycje. Jeśli wybrano warstwie podstawowe, można dodać tylko repliki.

*__Partycje__* umożliwiają usługi przechowywania i przeszukiwania więcej dokumentów.

*__Repliki__* Zezwalaj usługi obsługi wyższą obciążenia kwerend wyszukiwania — [usługa wymaga repliki 2, aby osiągnąć SLA tylko do odczytu, a 3 repliki uzyskanie SLA odczytu/zapisu](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Przejdź do karta zarządzania usługą Azure wyszukiwania w Azure Portal.
2. W karta **Ustawienia** wybierz **skalę**.
3. Dodając repliki lub partycje można skalować usługi.
  * Nie można skalować usługi poza jednostki 36 wyszukiwania. Z całkowitej liczby jednostek wyszukiwania jest produktem partycje i repliki (repliki * partycje = Suma jednostek wyszukiwania).
  * Jeśli wybrano warstwie podstawowe tylko można skalować repliki 3. Podstawowe usługi są powiązane z jedną partycją.

![](./media/search-create-service-portal/scale-service.png)

## <a name="next"></a>Następny
Po zainicjowaniu obsługi administracyjnej usługi wyszukiwania Azure, użytkownik będzie gotowy do [definiowania indeksu wyszukiwania Azure](search-what-is-an-index.md) , możesz przekazać i wyszukiwania danych.

Krótki samouczek, zobacz [Wprowadzenie do wyszukiwania Azure w portalu](search-get-started-portal.md) .
