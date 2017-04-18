 (kopii zapasowej magazynami<properties
   Tytuł strony = "Azure Backup limits table"
   Opis = "w tym artykule opisano limity systemu Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2015"
   ms.author="trinadhk";"jimpark"; "aashishr" />


Następujące ograniczenie dotyczy Azure kopii zapasowej.

| Identyfikator limitu | Domyślny Limit |
|---|---|
|Liczba serwerów i komputerów zarejestrowane przed każdym magazynu|50 dla systemu Windows Server klienta i SCDPM <br/> 200 IaaS maszyny wirtualne|
|Rozmiar źródła danych dla dane przechowywane w magazynie Azure magazynu|Maksymalna liczba 54400 GB<sup>1</sup>|
|Liczba kopii zapasowej magazynów, utworzone w każdej subskrypcji Azure|25 (kopia zapasowa magazynów) <br/> 25 usługi odzyskiwania magazynu w rozbiciu na regiony|
|Liczba kopii zapasowej mogą być planowane na dzień|3 dziennie dla systemu Windows Server/Client <br/> 2 dziennie SCDPM <br/> Raz dziennie IaaS maszyny wirtualne|
|Dyski danych dołączone do Azure maszyn wirtualnych do tworzenia kopii zapasowych|16|

- <sup>1</sup> Wykonywanie kopii zapasowych maszyn wirtualnych IaaS nie dotyczy limit 54400 GB.

