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
    ms.date="10/12/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-security-and-identity"></a>Zabezpieczenia Azure dla instytucji rządowych i tożsamości

##  <a name="key-vault"></a>Kluczowe magazynu
Aby uzyskać szczegółowe informacje na tej usługi i jak używać tej funkcji, zobacz <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure klucza magazynu dokumentacji publicznej.</a>

### <a name="data-considerations"></a>Zagadnienia dotyczące danych
Poniższe informacje identyfikuje granicę dla instytucji rządowych Azure dla Azure klucza magazynu:

| Dane regulacji kontrolowane dozwolone | Dane regulacji kontrolowane niedozwolone |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wszystkie dane są szyfrowane za pomocą klucza Azure klucza magazynu może zawierać dane Regulated kontrolowane. | Azure klucza magazynu metadanych nie jest dozwolona zawierać kontrolowane Eksportuj dane. Metadane to między innymi wszystkie dane konfiguracji wprowadzone podczas tworzenia i przechowywania do magazynu klucza.  Nie wprowadzaj dane Regulated kontrolowane w następujących polach: nazwy grupy zasobów i klucza magazynu, nazwy subskrypcji. |

Klucz magazynu jest zazwyczaj dostępny w Azure dla instytucji rządowych. Jak publicznej istnieje bez rozszerzenia, więc klucz magazynu jest dostępne za pośrednictwem programu PowerShell i polecenie tylko.

## <a name="next-steps"></a>Następne kroki

Informacje dodatkowe i aktualizacji subskrybowanie <a href="https://blogs.msdn.microsoft.com/azuregov/">Blog dotyczący programu Microsoft Azure dla instytucji rządowych.</a>
