<properties
    pageTitle="Azure dokumentacji dla instytucji rządowych | Microsoft Azure"
    description="Umożliwia porównanie funkcji i wskazówki dotyczące tworzenia aplikacji dla instytucji rządowych Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="09/29/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-compute"></a>Obliczeń Azure dla instytucji rządowych

##  <a name="virtual-machines"></a>Maszyn wirtualnych

Aby uzyskać szczegółowe informacje na tej usługi i jak używać tej funkcji zobacz [Azure maszyn wirtualnych rozmiarów](../virtual-machines/virtual-machines-windows-sizes.md).

### <a name="variations"></a>Odmiany

Następujące wersje maszyn wirtualnych będą ogólnie Dostępnym w Azure dla instytucji rządowych:

JEDNOSTKA SKU MASZYN WIRTUALNYCH|Stany Zjednoczone Gov VA|Stany Zjednoczone Gov IA|Notatki
---|---|---|---
A|GA|GA|Brak
Dv1|GA|-|Brak
DSv1|GA|-|Brak
Dv2|GA|GA|wkrótce 15
F|GA|GA|Brak
G|Planowana|-|Brak

###  <a name="data-considerations"></a>Zagadnienia dotyczące danych

Poniższe informacje określa granicę dla instytucji rządowych Azure maszyn wirtualnych Azure:

| Dane regulacji kontrolowane dozwolone | Dane regulacji kontrolowane niedozwolone |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dane wprowadzone, przechowywane i przetwarzane w maszyny mogą zawierać kontrolowane Eksportuj dane. Pliki binarne działa w obrębie maszyn wirtualnych Azure. Statyczne uwierzytelnienia, takich jak haseł i PIN karty inteligentnej dostępu do składniki platformy Azure. Kluczy prywatnych certyfikatów służące do zarządzania składniki platformy Azure. Parametry połączenia SQL.  Inne zabezpieczeń informacji i hasła, takich jak certyfikaty, klucze szyfrowania kluczy głównych i kluczy miejsca do magazynowania przechowywanych w usługach Azure.  | Metadane nie jest dozwolona zawierać kontrolowane Eksportuj dane. Metadane to między innymi wszystkie dane konfiguracji wprowadzone podczas tworzenia i przechowywania komputera wirtualnych Azure.  Nie wprowadzaj dane Regulated kontrolowane w następujących polach: dzierżawa roli nazwy i grup zasobów, wdrażania, nazwy zasobów, znaczniki zasobów  

## <a name="next-steps"></a>Następne kroki

Informacje dodatkowe i aktualizacji subskrybowanie <a href="https://blogs.msdn.microsoft.com/azuregov/">Blog dotyczący programu Microsoft Azure dla instytucji rządowych.</a>
