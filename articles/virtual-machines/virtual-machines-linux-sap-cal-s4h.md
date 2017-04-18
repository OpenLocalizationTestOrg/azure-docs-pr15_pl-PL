<properties 
pageTitle="Wdrażanie HANA HANA S-4 lub PC-4 na Azure maszyn wirtualnych | Microsoft Azure" 
description="Wdrażanie HANA S-4 lub HANA PC/4 na Azure maszyn wirtualnych" 
services="virtual-machines-linux" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
  keywords=""/> 
<tags 
  ms.service="virtual-machines-linux" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.tgt_pltfrm="vm-linux" 
  ms.workload="infrastructure-services" 
  ms.date="09/15/2016" 
  ms.author="hermannd"/> 


# <a name="deploying-s4-hana-or-bw4-hana-on-microsoft-azure"></a>Wdrażanie HANA S-4 lub HANA PC/4 na platformy Microsoft Azure 

Ten artykuł zawiera opis sposobu wdrażania HANA S/4 Microsoft Azure za pośrednictwem SAP w chmurze urządzenia biblioteki 3.0.
Zrzutów ekranu Pokaż proces krok po kroku. Wdrażanie innych rozwiązań opartych na SAP HANA, takie jak HANA PC/4 działa tak samo z perspektywy proces. Tylko jedna ma wybrać inne rozwiązanie.

Zaczynać się SAP w chmurze urządzenia biblioteki (SAP CAL) Przejdź [tutaj](https://cal.sap.com/). Istnieje blogu z SAP o nowych [SAP w chmurze urządzenia biblioteki 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 


Następujące zrzuty ekranu przedstawiają krok po kroku metody wdrażanie HANA S/4 Microsoft Azure. Proces działa tak samo dla innych rozwiązań likeBW/4 HANA.


![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-1b.jpg)

Pierwszy obraz zawiera wszystkie rozwiązań opartych na SAP CAL HANA, które są dostępne w programie Microsoft Azure.
Exemplarily "SAP S/4 HANA lokalnego wersji" (rozwiązanie u dołu zrzut ekranu) został wybrany do przechodzą przez ten proces.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-2.jpg)

Najpierw na nowe konto SAP CAL musi być utworzony. Obecnie są dwie możliwości Azure - standardowe Azure i Azure w Chinach Chin, który jest obsługiwany przez firmę 21Vianet partnera.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic3b.jpg)

Następnie będzie miał wprowadź identyfikator subskrypcji Azure, który znajduje się na portal Azure — Zobacz także dodatkowo w dół jak ją uzyskać. Następnie certyfikat Azure zarządzania muszą być pobrane.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic6b.jpg)

Nowe Azure jeden portalu znajduje elementu "Subskrypcji" po lewej stronie. Kliknij, aby wyświetlić wszystkie aktywne subskrypcje dla użytkownika.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic7b.jpg)

Wybierając jedną z subskrypcji, a następnie wybierając że "Certyfikaty zarządzania" wyjaśniono który jest nowa koncepcja za pomocą "podmioty usługi" dla nowego modelu Azure Menedżera zasobów.
SAP CAL nie jest jeszcze dostosowane do tego nowego modelu i nadal wymaga "klasycznego" modelu i byłego portal Azure do pracy z certyfikaty zarządzania.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic4b.jpg)

W tym miejscu jedną Zobacz byłego portal Azure. Przekazywanie certyfikatu zarządzania daje SAP CAL uprawnień do tworzenia maszyn wirtualnych subskrypcji klienta. W obszarze "Subskrypcje" karta jedną można znaleźć identyfikator subskrypcji, które należy podać w portalu SAP CAL.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic5.jpg)

Na karcie drugiego następnie jest możliwe przekazywanie certyfikatu zarządzania pobraną przed z SAP CAL.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic8.jpg)

Niewielkie okno dialogowe pojawia się, aby wybrać plik pobranego certyfikatu.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic9.jpg)

Po przesłaniu został certyfikat połączenie między SAP CAL i obsługi klientów Azure subskrypcji można zbadać w SAP CAl. Mała wiadomości należy wyskakujące informuje, czy połączenie jest prawidłowy.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic10.jpg)

Po zakończeniu instalacji konto będzie miał wybierz rozwiązanie, które powinny zostać wdrożone i utworzyć wystąpienia.
W trybie "podstawowe" jest naprawdę uproszczony. Wprowadź nazwę obiektu, wybierz pozycję Azure region i zdefiniuj hasło główne rozwiązania.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic11.jpg)

Po pewnym czasie, w zależności od rozmiaru i złożoności rozwiązanie (oszacowanie wynika SAP CAL) jest wyświetlany jako "aktywnej" i gotowy do użycia. Jest bardzo proste.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic12.jpg)

Spojrzenie na niektóre informacje rozwiązanie jedną można zobaczyć, jakiego rodzaju maszyny wirtualne wdrożono. W tym przypadku trzy Azure maszyny wirtualne o różnych rozmiarach i przeznaczenie zostały utworzone.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic13.jpg)

W portalu Azure maszyn wirtualnych znajdują się począwszy od tej samej nazwie wystąpienia podano w SAP CAL.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic14b.jpg)

Teraz jest możliwe nawiązywanie połączenia z rozwiązanie przy użyciu przycisku Połącz w portalu SAP CAL. Okno dialogowe nieco zawiera łącze do Podręcznik użytkownika opisujący poświadczeń domyślnych do pracy z rozwiązanie.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic15.jpg)

Innym rozwiązaniem jest, aby zalogować się do klienta maszyn wirtualnych systemu Windows i rozpocząć na przykład wstępnie skonfigurowane Graficznym SAP.







