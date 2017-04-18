<properties 
pageTitle="Wdrażanie SAP IDES EHP7 z dodatkiem SP3 systemu SAP ERP 6.0 na platformy Microsoft Azure | Microsoft Azure" 
description="Wdrażanie SAP IDES EHP7 z dodatkiem SP3 systemu SAP ERP 6.0 na platformy Microsoft Azure" 
services="virtual-machines-windows" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
keywords=""/> 
<tags 
ms.service="virtual-machines-windows" 
ms.devlang="na" 
ms.topic="article" 
ms.tgt_pltfrm="vm-windows" 
ms.workload="infrastructure-services" 
ms.date="09/16/2016" 
ms.author="hermannd"/> 


# <a name="deploying-sap-ides-ehp7-sp3-for-sap-erp-60-on-microsoft-azure"></a>Wdrażanie SAP IDES EHP7 z dodatkiem SP3 systemu SAP ERP 6.0 na platformy Microsoft Azure 

Ten artykuł zawiera opis sposobu wdrażania IDES SAP w celu korzystania z programu SQL Server i systemu operacyjnego Microsoft Azure za pośrednictwem SAP w chmurze urządzenia biblioteki 3.0. Zrzutów ekranu Pokaż proces krok po kroku. Wdrażanie innych rozwiązań na liście działa tak samo z perspektywy proces. Tylko jedna ma wybrać inne rozwiązanie.

Zaczynać się SAP w chmurze urządzenia biblioteki (SAP CAL) Przejdź [tutaj](https://cal.sap.com/). Istnieje blogu z SAP o nowych [SAP w chmurze urządzenia biblioteki 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 


Następujące zrzuty ekranu przedstawiają krok po kroku metody wdrażanie IDES SAP w programie Microsoft Azure. Proces działa tak samo dla innych rozwiązań.


![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

Pierwszy obraz zawiera wszystkie rozwiązania, które są dostępne w programie Microsoft Azure. Wyróżnione systemu SAP IDES rozwiązanie, które jest dostępna tylko na Azure został wybrany do przechodzą przez ten proces.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic2.jpg)

Najpierw na nowe konto SAP CAL musi być utworzony. Obecnie są dwie możliwości Azure - standardowe Azure i Azure w Chinach Chin, który jest obsługiwany przez firmę 21Vianet partnera.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic3.jpg)

Następnie będzie miał wprowadź identyfikator subskrypcji Azure, który znajduje się na portal Azure — Zobacz także dodatkowo w dół jak ją uzyskać. Następnie certyfikat Azure zarządzania muszą być pobrane.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic6.jpg)

Nowe Azure jeden portalu znajduje elementu "Subskrypcji" po lewej stronie. Kliknij, aby wyświetlić wszystkie aktywne subskrypcje dla użytkownika.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic7.jpg)

Wybierając jedną z subskrypcji, a następnie wybierając że "Certyfikaty zarządzania" wyjaśniono który jest nowa koncepcja za pomocą "podmioty usługi" dla nowego modelu Azure Menedżera zasobów.
SAP CAL nie jest jeszcze dostosowane do tego nowego modelu i nadal wymaga "klasycznego" modelu i byłego portal Azure do pracy z certyfikaty zarządzania.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic4.jpg)

W tym miejscu jedną Zobacz byłego portal Azure. Przekazywanie certyfikatu zarządzania daje SAP CAL uprawnień do tworzenia maszyn wirtualnych subskrypcji klienta. W obszarze "Subskrypcje" karta jedną można znaleźć identyfikator subskrypcji, które należy podać w portalu SAP CAL.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic5.jpg)

Na karcie drugiego następnie jest możliwe przekazywanie certyfikatu zarządzania pobraną przed z SAP CAL.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic8.jpg)

Niewielkie okno dialogowe pojawia się, aby wybrać plik pobranego certyfikatu.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic9.jpg)

Po przesłaniu został certyfikat połączenie między SAP CAL i obsługi klientów Azure subskrypcji można zbadać w SAP CAl. Mała wiadomości należy wyskakujące informuje, czy połączenie jest prawidłowy.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic10.jpg)

Po zakończeniu instalacji konto będzie miał wybierz rozwiązanie, które powinny zostać wdrożone i utworzyć wystąpienia.
W trybie "podstawowe" jest naprawdę uproszczony. Wprowadź nazwę obiektu, wybierz pozycję Azure region i zdefiniuj hasło główne rozwiązania.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic11.jpg)

Po pewnym czasie, w zależności od rozmiaru i złożoności rozwiązanie (oszacowanie wynika SAP CAL) jest wyświetlany jako "aktywnej" i gotowy do użycia. Jest bardzo proste.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic12.jpg)

Spojrzenie na niektóre informacje rozwiązanie jedną można zobaczyć, jakiego rodzaju maszyny wirtualne wdrożono. W tym przypadku jest jeden pojedynczy maszyna Azure wirtualna wielkości D12 utworzone przez SAP CAL.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic13.jpg)

W portalu Azure maszyny wirtualnej znajdują się począwszy od tej samej nazwie wystąpienia podano w SAP CAL.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic14.jpg)

Teraz jest możliwe nawiązywanie połączenia z rozwiązanie przy użyciu przycisku Połącz w portalu SAP CAL. Okno dialogowe nieco zawiera łącze do Podręcznik użytkownika opisujący poświadczeń domyślnych do pracy z rozwiązanie.
[W tym miejscu](https://caldocs.hana.ondemand.com/caldocs/help/Getting_Started_Guide_IDES607MSSQL.pdf) jest łącze do przewodnika dotyczącego rozwiązanie IDES.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Innym rozwiązaniem jest, aby zalogować się do maszyn wirtualnych systemu Windows i rozpocząć na przykład wstępnie skonfigurowane Graficznym SAP.





