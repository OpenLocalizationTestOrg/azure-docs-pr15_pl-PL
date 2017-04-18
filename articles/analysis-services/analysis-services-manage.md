<properties
   pageTitle="Zarządzanie usługami Azure Analysis | Microsoft Azure"
   description="Dowiedz się, jak zarządzać serwer usług Analysis Services w Azure."
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

# <a name="manage-analysis-services"></a>Zarządzanie usług Analysis Services

Po utworzeniu serwer usług Analysis Services w Azure, może być niektóre zadania administracyjne, trzeba wykonać razu lub kiedyś występowanie. Na przykład uruchomić przetwarzania odświeżanie danych, określanie osób, które można uzyskać dostęp do modeli na serwerze i monitorowanie kondycji swojego serwera. Niektóre zadania związane z zarządzaniem można wykonać tylko w portal Azure, inne osoby w SQL Server Management Studio (SSMS), a niektóre zadania można wykonywać w jeden.

## <a name="azure-portal"></a>Azure portal
[Azure portal](http://portal.azure.com/) to miejsce, w którym można tworzyć i usuwać serwery, monitorowanie zasobów serwera, zmienianie rozmiaru i zarządzać kto ma dostęp do serwerów.  Jeśli występują problemy, możesz również przesłać prośbę o pomoc techniczną.

![Uzyskaj nazwę serwera platformy Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Nawiązywanie połączenia z serwerem w Azure działa tak samo jak nawiązywanie połączenia z wystąpieniem serwera w organizacji. Z SSMS można wykonywać te same zadania, takie jak dane procesu lub utworzyć skrypt przetwarzania, Zarządzanie rolami i za pomocą programu PowerShell.

![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

 Jedną z większym różnice jest uwierzytelniania, których używasz do łączenia się z serwerem. Aby połączyć się z serwerem usług Analysis Services Azure, musisz wybierz **Uwierzytelnianie hasła usługi Active Directory**.

### <a name="to-connect-with-ssms"></a>Aby połączyć się z SSMS
1. Przed nawiązaniem połączenia jest potrzebne do uzyskania nazwy serwera. W **portalu Azure** > serwera > **Przegląd** > **nazwę serwera**, skopiuj nazwę serwera.

    ![Uzyskaj nazwę serwera platformy Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

2. W SSMS > **Eksplorator obiektów**, kliknij pozycję **Połącz** > **Usług Analysis Services**.

3. W oknie dialogowym **połączenie z serwerem** Wklej w polu Nazwa serwera, a następnie w **uwierzytelniania**, wybierz jedną z następujących czynności:

    **Zintegrowane uwierzytelnianie usługi active Directory** za pomocą rejestracji jednokrotnej z usługą Active Directory federation usługi Azure Active Directory.

    **Uwierzytelnianie hasła usługi active Directory** za pomocą konta organizacji. Na przykład podczas nawiązywania połączenia z wartością domeny sprzężone komputera.

    Uwaga: Jeśli nie widzisz uwierzytelnianie usługi Active Directory, może być konieczne [Włączenie uwierzytelniania usługi Azure Active Directory](#enable-azure-active-directory-authentication) SSMS.

    ![Nawiązywanie połączenia w SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

Ponieważ zarządzanie serwerem platformy Azure za pomocą SSMS jest tak samo jak zarządzanie lokalnego serwera, firma Microsoft nie chce Przechodzenie do szczegółów tutaj. Cała Pomoc możesz muszą można znaleźć w [Zarządzania wystąpienia usług Analysis](https://msdn.microsoft.com/library/hh230806.aspx) w witrynie MSDN.

## <a name="server-administrators"></a>Administratorzy serwera
Umożliwia **Administratorów usług Analysis** w karta kontrolki serwera w Azure portal lub SSMS Zarządzanie administratorami serwera. Administratorzy usługi Analysis są Administratorzy serwera bazy danych z uprawnieniami dla typowych zadań administracyjnych bazy danych, takie jak dodawanie i usuwanie baz danych oraz zarządzanie użytkownikami. Domyślnie użytkownik, który tworzy serwer w Azure portal zostaną automatycznie dodane jako administrator Analysis Services.

Możesz również powinni wiedzieć:

-   Identyfikator Windows Live ID nie jest typ tożsamości obsługiwane Azure usług Analysis Services.  
-   Administratorzy usługi Analysis muszą być prawidłowymi użytkownikami usługi Azure Active Directory.
-   W przypadku tworzenia na serwerze usług Analysis Services Azure za pomocą Menedżera zasobów Azure szablony, administratorów usług Analysis trwa tablicę JSON użytkowników, które mają być dodane jako administratorzy.

Analysis Services administratorzy mogą się różnić od Administratorzy Azure zasobów, które umożliwia zarządzanie zasobami Azure subskrypcji. To zachowuje zgodność z istniejącymi XMLA i TSML Zarządzanie zachowania w usługach Analysis Services i pozwala oddzielnie różnic obowiązków między zarządzania zasobami Azure i Analysis Services Zarządzanie bazą danych.

Aby wyświetlić wszystkich ról i uzyskać dostęp do typów dla zasobu Azure usług Analysis Services, za pomocą kontroli dostępu (IAM) na karta kontrolki.

## <a name="database-users"></a>Użytkowników bazy danych
Azure użytkowników bazy danych modelu usług Analysis Services muszą być w usługi Azure Active Directory. Przy użyciu adresu e-mail organizacji lub głównej nazwy użytkownika musi być użytkowników określone dla modelu bazy danych. To różni się od lokalnej bazy danych modelu obsługujące użytkowników przez użytkowników domeny systemu Windows.

Możesz dodać użytkowników za pomocą [przypisania ról w usłudze Azure Active Directory](../active-directory/role-based-access-control-configure.md) lub przy użyciu [Języka skryptów modelu tabelarycznego](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) w programie SQL Server Management Studio.

**Przykładowy skrypt TMSL**

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Users"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@contoso.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="enable-azure-active-directory-authentication"></a>Włącz uwierzytelnianie usługi Azure Active Directory
Aby włączyć funkcję uwierzytelniania usługi Azure Active Directory dla SSMS w rejestrze, Utwórz plik tekstowy o nazwie EnableAAD.reg, a następnie skopiuj i wklej następujące czynności:


```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\Microsoft Analysis Services\Settings]
"AS AAD Enabled"="True"
```

Zapisz, a następnie uruchom plik.



## <a name="next-steps"></a>Następne kroki
Jeśli jeszcze tego nie wdrożono modelu tabelarycznego już nowego serwera, teraz jest odpowiednim czasie. Aby uzyskać więcej informacji, zobacz temat [Deploy Azure usług Analysis Services](analysis-services-deploy.md).

Jeśli model został wdrożony do swojego serwera, możesz połączyć się z nim przy użyciu klienta lub przeglądarki. Aby uzyskać więcej informacji, zobacz [Pobieranie danych z usług Analysis Services Azure serwera](analysis-services-connect.md).
