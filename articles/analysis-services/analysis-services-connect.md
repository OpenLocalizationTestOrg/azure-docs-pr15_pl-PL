<properties
   pageTitle="Pobieranie danych z usług Analysis Services Azure | Microsoft Azure"
   description="Dowiedz się, jak nawiązać połączenie i pobieranie danych z serwera usług Analysis Services w Azure."
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

# <a name="get-data-from-azure-analysis-services"></a>Pobieranie danych z usług Analysis Services Azure
Po została utworzona serwer w Azure i używany model tabelaryczny, użytkownicy w Twojej organizacji przystąpić do łączenia i rozpocząć eksplorowanie danych.

Azure usług Analysis Services obsługuje połączenia klienta przy użyciu [aktualizacji modeli obiektów](#client-libraries); TOMA, AMO, Adomd.Net lub MSOLAP, połączenia za pomocą xmla na serwerze. Na przykład usługi Power BI, Power BI Desktop, Excel lub dowolnej aplikacji innych firm klienta, która obsługuje obiektowych modeli.

## <a name="server-name"></a>Nazwa serwera
Po utworzeniu serwer usług Analysis Services w Azure Określ unikatową nazwę i region, w którym ma być utworzony serwer. Określając nazwę serwera w połączeniu, nazewnictwa serwera jest:
```
<protocol>://<region>/<servername>
```
 Protocol (protokół) w przypadku ciąg **asazure**, region jest identyfikator Uri obszaru, w której utworzono serwera (na przykład w USA Zachodnia westus.asazure.windows.net) i nazwa_serwera jest nazwą serwera unikatowe w regionie.

## <a name="get-the-server-name"></a>Uzyskaj nazwę serwera
Przed nawiązaniem połączenia jest potrzebne do uzyskania nazwy serwera. W **portalu Azure** > serwera > **Przegląd** > **nazwę serwera**, skopiuj nazwę całego serwera. Jeśli inni użytkownicy w Twojej organizacji łączy do tego serwera zbyt, chcesz udostępnić ten nazwy serwera. Określając nazwę serwera, należy użyć całą ścieżkę.

![Uzyskaj nazwę serwera platformy Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connect-in-power-bi-desktop"></a>Nawiązywanie połączenia w Power BI Desktop

> [AZURE.NOTE] Ta funkcja jest podgląd.

1. W aplikacji [Power BI Desktop](https://powerbi.microsoft.com/desktop/), kliknij przycisk **Pobierz dane** > **baz danych** > **Azure usług Analysis Services**.

2. W polu **serwer**Wklej nazwę serwera ze Schowka.

3. W **bazie danych**Jeśli znasz nazwę bazy danych modelu tabelarycznego lub perspektywy, który chcesz nawiązać połączenie, wklej go w tym miejscu. W przeciwnym razie użytkownik może pozostaw to pole puste. Możesz wybrać bazy danych lub perspektywy na następnym ekranie.

4. Opuść domyślna opcja **live Połącz** zaznaczone, a następnie naciśnij **Połącz**. Jeśli zostanie wyświetlony monit o wprowadzenie konta, wprowadź konta swojej organizacji.

5. W **Nawigatorze**rozwiń serwer, a następnie wybierz model lub perspektywy, który chcesz nawiązać połączenie, a następnie kliknij przycisk **Połącz**. Jednym kliknięciem w modelu lub Perspektywa zawiera wszystkie obiekty w tym widoku.


## <a name="connect-in-power-bi"></a>Nawiązywanie połączenia w usłudze Power BI
1. Tworzenie pliku Power BI Desktop, która ma aktywne połączenie modelu na serwerze.

2. W [Usłudze Power BI](https://powerbi.microsoft.com)kliknij pozycję **Pobierz dane** > **plików**. Znajdź i zaznacz plik.


## <a name="connect-in-excel"></a>Nawiązywanie połączenia w programie Excel
Łączenie z serwera Azure Analysis Services w programie Excel jest obsługiwane za pomocą pobieranie danych w programie Excel 2016 lub dodatku Power Query we wcześniejszych wersjach. [Dostawca MSOLAP.7](https://aka.ms/msolap) jest wymagane. Nawiązywanie połączenia przy użyciu Kreatora importu tabeli w dodatku Power Pivot nie jest obsługiwane.

1. W programie Excel 2016, na karcie **dane** Wstążki kliknij pozycję **Pobierz dane zewnętrzne** > **Z innych źródeł** > **Z usług Analysis Services**.

2. W Kreatorze połączenia danych w polu **Nazwa serwera**Wklej nazwę serwera ze Schowka. Następnie **poświadczenia logowania**, wybierz **za pomocą następujących nazwę użytkownika i hasło**, a następnie wpisz nazwę organizacji użytkownika, na przykład nancy@adventureworks.com, i hasło.

    ![Nawiązywanie połączenia w programie Excel logowania](./media/analysis-services-connect/aas-connect-excel-logon.png)

4. W **Wybieranie bazy danych i tabeli**wybierz bazę danych i modelu lub perspektywy i kliknij przycisk **Zakończ**.

    ![Nawiązywanie połączenia w programie Excel wybierz modelu](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Parametry połączenia
Podczas łączenia się przy użyciu modelu obiektów programu tabelarycznym usług Analysis Services Azure, użyj następujących formatów parametry połączenia:

###### <a name="integrated-azure-active-directory-authentication"></a>Zintegrowane uwierzytelnianie usługi Azure Active Directory
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Zintegrowane uwierzytelnianie wpłynie na usługi Azure Active Directory pamięci podręcznej poświadczeń Jeśli to możliwe. W przeciwnym razie zostanie wyświetlone okno Azure logowania.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure uwierzytelniania usługi Active Directory za pomocą nazwy użytkownika i hasła
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Bibliotek klienta
Podczas łączenia się z usługami Analysis Services Azure z programu Excel lub inne interfejsy, takie jak TOMA, AsCmd, ADOMD.NET, może być konieczne zainstaluj najnowszą bibliotek klienta dostawcy. Pobierz najnowsze informacje:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[BIBLIOTEKA ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>



## <a name="next-steps"></a>Następne kroki
[Zarządzanie serwerem](analysis-services-manage.md)
