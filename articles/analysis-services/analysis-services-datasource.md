<properties
   pageTitle="Połączenia źródła danych | Microsoft Azure"
   description="W tym artykule opisano połączenia ze źródłami danych w przypadku modeli danych w usługach Analysis Services Azure."
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
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="datasource-connections"></a>Połączenia źródła danych

Modele danych w usługach Analysis Services Azure może wymagać dostawców danych podczas nawiązywania połączenia ze źródłami danych, niektóre. W niektórych przypadkach modeli tabelarycznych połączenia ze źródłami danych za pomocą natywnego dostawców, takich jak program SQL Server Native Client (SQLNCLI11) może zostać zwrócony błąd.

Na przykład; Jeśli masz w pamięci lub bezpośrednia kwerenda modelu danych nawiązuje połączenie ze źródłem danych chmurze, takich jak bazy danych SQL Azure, użycie natywnych dostawcami usług innych niż SQLOLEDB, może zostać wyświetlony komunikat o błędzie: **"Provider"SQLNCLI11.1"nie jest zarejestrowany"**.

Lub, jeśli masz modelu zapytania bezpośredniego połączenia ze źródłami danych lokalnych, jeśli korzystasz z macierzystego dostawców może zostać wyświetlony komunikat o błędzie: **"błąd podczas tworzenia zestaw wierszy OLE DB. Nieprawidłowa składnia u "Granica" "**.

## <a name="data-source-providers"></a>Dostawców źródeł danych

Następujących dostawców źródła danych są obsługiwane w pamięci lub bezpośrednie kwerendy modeli danych podczas nawiązywania połączenia lokalnego lub źródeł danych w chmurze:

|               | **Źródła danych**                     | **W pamięci**                            |  **Bezpośrednia kwerenda**                                           |
|---------------------------|-------------------------------|---------------------------------------------|---------------------------------------------|
| **Chmury**                     | Magazyn danych Azure SQL      | Dostawca danych programu .NET framework dla programu SQL Server | Dostawca danych programu .NET framework dla programu SQL Server |
|                           | Baza danych SQL Azure            | Dostawca danych programu .NET framework dla programu SQL Server | Dostawca danych programu .NET framework dla programu SQL Server |
| **Lokalne** (za pomocą bramy) | Program SQL Server                    | Program SQL Server Native Client 11.0               | Dostawca danych programu .NET framework dla programu SQL Server |
|                           |  Program SQL Server                             | Dostawca Microsoft OLE DB dla programu SQL Server    |   Dostawca danych programu .NET framework dla programu SQL Server                                          |
|                           |  Program SQL Server                             | Dostawca danych programu .NET framework dla programu SQL Server |  Dostawca danych programu .NET framework dla programu SQL Server                                           |
|                           | Oracle                        | Dostawca Microsoft OLE DB dla programu Oracle        | Dostawca danych programu Oracle dla środowiska .NET               |
|                           |  Oracle                             | Dostawca danych programu Oracle dla środowiska .NET               | Dostawca danych programu Oracle dla środowiska .NET                                            |
|                           | Teradata                      | Dostawca OLE DB dla programu Teradata                | Dostawca danych programu Teradata dla środowiska .NET             |
|                           |  Teradata                             | Dostawca danych programu Teradata dla środowiska .NET             |  Dostawca danych programu Teradata dla środowiska .NET                                            |
|                           | System analiz platformy | Dostawca danych programu .NET framework dla programu SQL Server | Dostawca danych programu .NET framework dla programu SQL Server |


> [AZURE.NOTE] Upewnij się, że zainstalowano dostawców 64-bitowej, podczas korzystania z lokalnego bramy.

Podczas migracji lokalnego modelu tabelarycznego usług Analysis Services programu SQL Server Azure usług Analysis Services, może być konieczne zmienić dostawcę.

**Aby określić dostawcę źródła danych**

1. W SSDT > **Eksplorator modelu tabelarycznego** > **Źródła danych**, kliknij prawym przyciskiem myszy połączenia ze źródłem danych, a następnie kliknij **Edytuj źródło danych**.

2. W obszarze **Edytowanie połączenia**kliknij przycisk **Zaawansowane** , aby otworzyć okno właściwości zaawansowane.

3. W oknie dialogowym **Ustawianie właściwości zaawansowane** > **dostawców**, a następnie wybierz pozycję odpowiedniego dostawcy.

## <a name="impersonation"></a>Personifikacji
W niektórych przypadkach może być konieczne określić konto różnych personifikacji. Konta personifikacji można określić w SSDT lub SSMS.

W przypadku lokalnych źródeł danych:

- Jeśli używane uwierzytelnianie SQL, personifikacji powinny być konta usługi.
- Jeśli przy użyciu funkcji uwierzytelniania systemu Windows, ustaw hasła użytkownika systemu Windows. Dla programu SQL Server uwierzytelniania systemu Windows za pomocą konta personifikacji określonego jest obsługiwana tylko w przypadku modeli danych w pamięci.

Dla źródła danych w chmurze:

- Jeśli używane uwierzytelnianie SQL, personifikacji powinny być konta usługi.


## <a name="next-steps"></a>Następne kroki

Jeśli masz lokalnych źródeł danych, należy zainstalować [bramę lokalnego](analysis-services-gateway.md). Aby dowiedzieć się więcej na temat zarządzania serwerem SSDT lub SSMS, zobacz [Zarządzanie serwerem](analysis-services-manage.md).
