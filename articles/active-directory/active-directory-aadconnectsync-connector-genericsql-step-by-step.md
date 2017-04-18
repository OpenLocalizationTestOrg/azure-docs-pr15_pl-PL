<properties
   pageTitle="Rodzajowy łącznik SQL krok po kroku | Microsoft Azure"
   description="Ten artykuł jest Instruktaż prosty system HR krok po kroku, przy użyciu ogólnego łącznik SQL."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-step-by-step"></a>Rodzajowy łącznik SQL krok po kroku
W tym temacie jest przewodnik krok po kroku. Tworzy HR prostych przykładową bazę danych i jej używać na potrzeby importowania danych niektórych użytkowników i członkostwa w grupach.

## <a name="prepare-the-sample-database"></a>Przygotowywanie przykładowej bazy danych
Na serwerze z programem SQL Server należy uruchomić skrypt SQL w [dodatku A](#appendix-a). Ten skrypt tworzy przykładowej bazy danych o nazwie GSQLDEMO. Model obiektowy dla bazy danych utworzonej wygląda tego obrazu:  
![Model obiektowy](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\objectmodel.png)

Również utworzyć użytkownika, którego chcesz użyć do połączenia z bazą danych. W tym przykładzie użytkownik o nazwie FABRIKAM\SQLUser i znajduje się w domenie.

## <a name="create-the-odbc-connection-file"></a>Utwórz plik połączenia ODBC
Rodzajowy łącznik SQL używa ODBC nawiązania połączenia z serwerem zdalnym. Najpierw trzeba utworzyć plik z informacjami o połączeniu ODBC.

1. Uruchom narzędzie do zarządzania ODBC na serwerze:  
![ODBC](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc.png)
2. Wybierz kartę **Plik DSN**. Kliknij przycisk **Dodaj**.
![ODBC1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc1.png)
3. Współpraca w nowym polu sterownik precyzyjnie, więc zaznacz je i kliknij **Dalej >**.  
![ODBC2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc2.png)
4. Nadaj plikowi nazwę, na przykład **GenericSQL**.  
![ODBC3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc3.png)
5. Kliknij przycisk **Zakończ**.  
![ODBC4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc4.png)
6. Czas, który skonfiguruje połączenie. Nadaj dobry opis źródła danych i wprowadź nazwę serwera z programem SQL Server.  
![ODBC5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc5.png)
7. Umożliwia wybranie sposobu uwierzytelniania SQL. W tym przypadku firma Microsoft korzysta z uwierzytelniania systemu Windows.  
![ODBC6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc6.png)
8. Wprowadź nazwę bazy danych przykładowej **GSQLDEMO**.  
![ODBC7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc7.png)
9. Zachowaj domyślne wszystko na tym ekranie. Kliknij przycisk **Zakończ**.  
![ODBC8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc8.png)
10. Aby sprawdzić, czy wszystko działa zgodnie z oczekiwaniami, kliknij przycisk **Testuj źródło danych**.  
![ODBC9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc9.png)
11. Upewnij się, że wynik testu się pomyślnie.  
![ODBC10](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc10.png)
12. Plik konfiguracyjny ODBC powinna być teraz widoczna w pliku DSN.  
![ODBC11](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc11.png)

Teraz mamy pliku było potrzebne i będą mieli możliwość tworzenia łącznik.

## <a name="create-the-generic-sql-connector"></a>Tworzenie łącznika ogólnego SQL

1. W Menedżerze usługi synchronizacji interfejsu użytkownika zaznacz **łączników** i **Tworzenie**. Wybierz pozycję **Ogólne SQL (Microsoft)** i nadaj mu nazwę opisową.  
![Connector1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector1.png)
2. Znajdź plik DSN, który został utworzony w poprzedniej sekcji, a następnie przekazanie go na serwerze. Podanie poświadczeń w celu połączenia z bazą danych.  
![Connector2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector2.png)
3. W tym instruktażu pracujemy są co ułatwi abyśmy i powiedzieć, że istnieją dwa typy obiektów, **użytkowników** i **grup**.
![Connector3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector3.png)
4. Aby znaleźć atrybuty, potrzebne jest łącznika w celu wykrywania atrybutach sprawdzając samej tabeli. Ponieważ **użytkowników** jest słowo zastrzeżone w języku SQL, należy podać go w nawiasy kwadratowe [].  
![Connector4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector4.png)
5. Nadszedł czas na definiowanie atrybut kontrolnych i atrybut nazwy domeny. Dla **użytkowników**firma Microsoft korzysta z kombinacji dwa atrybuty nazwy użytkownika i pole Identyfikator pracownika. **Grupa**, firma Microsoft korzysta z grupy (nie rzeczywistych w rzeczywistych, ale w tym instruktażu działa).
![Connector5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector5.png)
6. Nie wszystkie typy atrybut można wykryć w bazie danych SQL. Typ atrybut odwołania w szczególności nie. Typ obiektu grupy trzeba zmienić OwnerID i MemberID zostać utworzone odwołanie.  
![Connector6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector6.png)
7. Atrybuty, które Wybraliśmy atrybuty odwołanie w poprzednim kroku wymagać typ obiektu wartości te są odwołanie do. W naszym przypadku typ obiektu użytkownika.  
![Connector7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector7.png)
8. Na stronie parametry globalne wybierz **znak wodny** jako strategia różnicy. Wpisać w formacie daty/godziny **RRRR MM-dd HH: mm:**.
![Connector8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector8.png)
9. Na stronie **Konfigurowanie partycje i hierarchie** wybierz oba typy obiektów.
![Connector9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector9.png)
10. **Wybierz typy obiektów** i **Atrybutów, wybierz pozycję**wybierz typy obiektów i wszystkich atrybutów. Na stronie **Konfigurowanie zakotwiczenia** kliknij przycisk **Zakończ**.

## <a name="create-run-profiles"></a>Tworzenie profili uruchamiania

1. W Menedżerze usługi synchronizacji interfejsu użytkownika zaznacz **Łączniki**i **Konfigurowanie profili uruchamiania**. Kliknij przycisk **Nowy profil**. Firma Microsoft Uruchom **Pełne importowanie**.  
![Runprofile1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile1.png)
2. Wybierz typ **Pełne importowanie (tylko etap)**.  
![Runprofile2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile2.png)
3. Wybierz pozycję partycją **obiektu = użytkownik**.  
![Runprofile3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile3.png)
4. Zaznacz **tabelę** , a następnie wpisz **[USERS]**. Przewiń w dół do sekcji Typ obiektu wiele wartości i wprowadzanie danych, jak pokazano na poniższej ilustracji. Wybierz pozycję **Zakończ** , aby zapisać krok.
![Runprofile4a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4a.png)  
![Runprofile4b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4b.png)  
5. Wybierz **Nowy krok**. Tym razem, wybierz pozycję **obiektu = Grupa**. Na ostatniej stronie przy użyciu konfiguracji, jak pokazano na poniższej ilustracji. Kliknij przycisk **Zakończ**.  
![Runprofile5a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5a.png)  
![Runprofile5b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5b.png)  
6. Opcjonalnie: Jeśli chcesz, można skonfigurować dodatkowe profili uruchamiania. W tym instruktażu jest używana tylko pełne importowanie.
7. Kliknij **przycisk OK** , aby zakończyć zmianę profili uruchamiania.

## <a name="add-some-test-data-and-test-the-import"></a>Dodaj dane testowe i testowania importu
Wypełnij niektórych testowymi danymi w bazie danych przykładowych. Gdy skończysz, wybierz pozycję **Uruchom** i **pełne importowanie**.

Poniżej przedstawiono użytkownika z dwóch numery telefonów i grupy z wybranych członków grupy.  
![cs1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs1.png)  
![CS2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs2.png)  

## <a name="appendix-a"></a>Dodatek A
**Skrypt SQL w celu utworzenia przykładowej bazy danych**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
