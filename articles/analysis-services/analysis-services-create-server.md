<properties
   pageTitle="Tworzenie serwer usług Analysis Services w Azure | Microsoft Azure"
   description="Dowiedz się, jak tworzyć wystąpienia serwera usług Analysis Services w Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="create-an-analysis-services-server"></a>Tworzenie serwer usług Analysis Services
W tym artykule opisano tworzenie nowego zasobu serwer usług Analysis Services w ramach subskrypcji Azure.

## <a name="before-you-begin"></a>Przed rozpoczęciem
Aby rozpocząć pracę, należy następująco:

- **Azure subskrypcji**: odwiedź [Bezpłatną wersję próbną Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) utworzyć konto.
- **Grupa zasobów**: Korzystanie z Grupa zasobów, masz już lub [Utwórz nowy](../azure-resource-manager/resource-group-overview.md).

> [AZURE.NOTE] Tworzenie serwer usług Analysis Services może spowodować nową usługę rozliczaniu. Aby uzyskać więcej informacji, zobacz ceny usług Analysis Services.

## <a name="create-an-analysis-services-server"></a>Tworzenie serwer usług Analysis Services

1. Zaloguj się do [portalu Azure](https://portal.azure.com).

2. Kliknij pozycję **+ Nowe** > **analizy + analizy** > **Usług Analysis Services**.

3. W karta **Usług Analysis Services** Wypełnij wymagane pola, a następnie naciśnij klawisz **Utwórz**.

    ![Tworzenie serwera](./media/analysis-services-create-server/aas-create-server-blade.png)

    - **Nazwa serwera**: wpisz unikatową nazwę używany do serwera.

    - **Subskrypcja**: Wybierz subskrypcję, ten serwer rachunków do.

    - **Grupa zasobów**: są kontenery ułatwia zarządzanie zbiorem Azure zasobów. Aby uzyskać więcej informacji, zobacz [grupy zasobów](../resource-group-overview.md).

    - **Lokalizacja**: serwer obsługuje Azure tego centrum danych lokalizacji. Wybierz lokalizację najbliższej największa bazy użytkowników.

    - **Poziom ceny**: Wybierz poziom cennik. Modele tabelaryczne do 100 GB są obsługiwane. Możesz zmienić z poziomu cen później.

4. Kliknij przycisk **Utwórz**.

Tworzenie zwykle trwa w obszarze chwilę. często zaledwie kilka sekund. Jeśli wybrano opcję **Dodaj do portalu**, przejdź do portalu sieci, aby wyświetlić nowy serwer. Lub przejdź do **innych usług** > **Usług Analysis Services** , aby sprawdzić, czy serwer jest gotowy. Jeśli nie widać odświeżenie listy.

 ![Pulpit nawigacyjny](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Następne kroki
Po utworzeniu serwera możesz [wdrożenia modelu](analysis-services-deploy.md) do niej przy użyciu SSDT lub z SSMS.

Jeśli model, który wdrażanie serwera łączy do lokalnych źródeł danych, musisz zainstalować na komputerze w sieci [lokalnej danych bramy](analysis-services-gateway.md) .
