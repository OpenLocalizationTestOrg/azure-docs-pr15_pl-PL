<properties
   pageTitle="Wprowadzenie do programu SQL bazy danych dynamicznych danych maskowanie (klasyczny Portal Azure)"
   description="Jak rozpocząć pracę z SQL bazy danych dynamicznych danych maskowanie w portalu klasyczny Azure"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>

# <a name="get-started-with-sql-database-dynamic-data-masking-azure-classic-portal"></a>Wprowadzenie do programu SQL bazy danych dynamicznych danych maskowanie (klasyczny Portal Azure)

> [AZURE.SELECTOR]
- [Dynamiczne dane maskowanie — Azure Portal](sql-database-dynamic-data-masking-get-started.md)

## <a name="overview"></a>Omówienie

SQL baza danych dynamicznych danych maskowanie ogranicza Uwidacznianie ważnych danych przez maskowanie go do użytkowników bez uprawnień. Dynamiczne dane maskowanie jest obsługiwana w wersji 12 wersji bazy danych SQL Azure.

Dynamiczne dane maskowanie ułatwia zapobieganie nieupoważnionemu dostępowi do ważnych danych, włączając klientów wyznaczyć stopień poufnych danych w celu uwidocznienia z minimalnymi wpływ na warstwie aplikacji. Jest funkcją na podstawie zasad zabezpieczeń, który spowoduje ukrycie poufne dane w zestawie wyników kwerendy na pola wyznaczonych bazy danych, gdy nie zostanie zmieniony w bazie danych.

Na przykład z przedstawicielem biura obsługi pośrodku połączenia może być identyfikowanie osób dzwoniących przez kilka cyfr numeru karty kredytowej lub numer PESEL, ale te elementy danych powinien nie pełni podsumowującymi z przedstawicielem działu obsługi. Czy wszystkie maski, ale ostatnich czterech cyfr numeru PESEL ani numeru karty kredytowej w wyniku zestaw jakiejkolwiek kwerendy można zdefiniować reguły maskowanie. Inny przykład maski odpowiednich danych można zdefiniować do ochrony danych identyfikowalne dane osobowe, tak, aby deweloper może wykonywać kwerendy w środowisku produkcyjnym w celu rozwiązywania problemów bez naruszenie zgodności z przepisami.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL baza danych dynamicznych danych maskowanie — informacje podstawowe

Możesz skonfigurować dynamiczne dane maskowanie zasad w portalu klasyczny Azure na karcie inspekcji i zabezpieczeń dla bazy danych.


> [AZURE.NOTE] Aby skonfigurować dynamiczne dane maskowanie w Azure Portal, zobacz [Wprowadzenie do programu SQL bazy danych dynamicznych danych maskowanie (Azure Portal)](sql-database-dynamic-data-masking-get-started.md).


### <a name="dynamic-data-masking-permissions"></a>Dynamiczne dane maskowanie uprawnień

Maskowanie dynamiczne dane można skonfigurować przez administratora bazy danych Azure, administrator serwera lub inspektor role.

### <a name="dynamic-data-masking-policy"></a>Dynamiczne dane maskowanie zasad

* **Użytkownicy SQL wykluczone z maskowanie** - zestaw użytkowników SQL lub tożsamości AAD, które otrzymają danych zaznaczona w wynikach zapytania SQL. Zauważ, że użytkownicy z uprawnieniami administratora będzie zawsze wykluczać z maskowanie i zostanie wyświetlony oryginalne dane bez dowolnego maski.

* **Maskowanie reguły** - zestaw reguł, które definiują wyznaczonych pola, które mają zostać ukryte i funkcji maskowanie, która będzie używana. Wyznaczone pola można zdefiniować za pomocą nazwy schematu bazy danych, nazwa tabeli i nazwę kolumny.

* **Maskowanie funkcji** — zestaw metod, wpływających na narażenie danych dla różnych scenariuszach.

| Funkcja maskowanie | Maskowanie warunków logicznych |
|----------|---------------|
| **Domyślne**  |**Pełna maskowanie według typów danych w odpowiednich polach**<br/><br/>• Użyj XXXX lub mniej znaków x, jeśli rozmiar pola jest mniejsza niż 4 znaki dla danych typu ciąg (nchar, ntext, nvarchar).<br/>• Użyj wartości 0 dla typów danych liczbowych (bigint bitowej, dziesiętne, int, pieniądze, liczbowe, smallint, smallmoney, tinyint, ruchomości, rzeczywistą).<br/>• Za pomocą 1900-01-01 typu danych Data/Godzina (Data, datetime2, daty i godziny, datetimeoffset, smalldatetime, czasu).<br/>• Wariantu SQL, wartość domyślna bieżącego typu jest używana.<br/>• XML dokumentu <masked/> jest używana.<br/>• Użyj wartości pustej specjalnych danych typu (Tabela sygnatury czasowej, identyfikator hierarchii, identyfikator GUID, format binarny, obraz, typów przestrzenna zmiennej liczby dwójkowej).
| **Karta kredytowa** |**Maskowanie metodę, która udostępnia cztery ostatnie cyfry wyznaczone pola** i dodanie ciągu stałej jako prefiksu w formularzu karty kredytowej.<br/><br/>XXXX-XXXX-XXXX-1234|
| **Numer ubezpieczenia społecznego** |**Maskowanie metodę, która udostępnia cztery ostatnie cyfry wyznaczone pola** i dodanie ciągu stałej jako prefiksu w formularzu American numer PESEL.<br/><br/>XXX-XX-1234 |
| **Adres e-mail** | **Maskowanie metody, która udostępnia pierwszą literę i zastępuje domeny z XXX.com** przy użyciu prefiksu ciąg stałej w postaci adresu e-mail.<br/><br/>aXX@XXXX.com |
| **Liczba losowa z zakresu** | **Maskowanie metodę, która generuje liczbę losową z zakresu** od wybranego ograniczenia i typy danych rzeczywistych. Jeśli wyznaczonych granic są równe, funkcja maskowanie będzie stałą liczbową.<br/><br/>![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started-portal/1_DDM_Random_number.png) |
| **Niestandardowy tekst** | **Maskowanie metodę, która udostępnia pierwsze i ostatnie znaki** i dodanie ciągu niestandardowe dopełnienia w środku. Jeśli ciąg oryginalny jest krótsza udostępnionej prefiks i sufiks, będzie używany tylko ciąg dopełnieniem.<br/>sufiks prefiks [dopełnienia]<br/><br/>![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started-portal/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-classic-portal"></a>Konfigurowanie maskowanie dynamiczne dane dla bazy danych za pomocą portalu klasyczny Azure

1. Uruchom program klasyczny portalu Azure pod adresem [https://manage.windowsazure.com](https://manage.windowsazure.com).

2. Kliknij odpowiednią bazę danych, które chcesz maski, a następnie kliknij kartę **Inspekcja i zabezpieczenia** .

3. W obszarze **dane dynamiczne maskowanie**kliknij pozycję **włączone** , aby włączyć dynamiczne dane maskowanie funkcji.  

4. Wpisz użytkowników SQL lub tożsamości AAD, które powinny być wyłączone z maskowanie i mają dostęp do danych poufnych zaznaczona. Należy to rozdzielaną średnikami listę użytkowników. Należy zauważyć, że użytkownicy z uprawnieniami administratora zawsze mają dostęp do oryginalne dane zaznaczona.

    >[AZURE.TIP] Aby uniemożliwić warstwy aplikacji można wyświetlać dane poufne dla użytkowników aplikacji uprzywilejowane, Dodaj użytkownika SQL lub AAD tożsamości aplikacji używa do kwerendy bazy danych. Zdecydowanie zaleca się, że ta lista zawiera minimalnej liczby użytkowników uprzywilejowanych, aby zminimalizować ryzyko poufne dane.

    ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started-portal/4_ddm_policy_classic_portal.png)

5. U dołu strony na pasku menu kliknij pozycję **Dodaj MASKĘ** , aby otworzyć maskowanie okno Konfiguracja reguły.

6. Wybierz **schemat**, **tabel** i **kolumn** z listy rozwijane, aby zdefiniować wyznaczonych pola, które zostaną ukryte.

7. **Funkcja MASKOWANIE** wybierz z listy danych poufnych maskowanie kategorii.

    ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started-portal/5_DDM_Add_Masking_Rule_Classic_Portal.png)

8. Kliknij **przycisk OK** w danych maskowanie oknie reguły, aby zaktualizować zestaw maskowanie reguły w danych dynamicznych maskowanie zasad.

9. Kliknij przycisk **ZAPISZ** , aby zapisać zasady maskowanie nową lub zaktualizowaną.


## <a name="set-up-dynamic-data-masking-for-your-database-using-transact-sql-statements"></a>Konfigurowanie danych dynamicznych maskowanie bazy danych za pomocą instrukcji Transact-SQL

Zobacz [maskowanie danych dynamicznych](https://msdn.microsoft.com/library/mt130841.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Konfigurowanie danych dynamicznych maskowanie bazy danych przy użyciu poleceń cmdlet programu Powershell

Zobacz [polecenia cmdlet bazy danych Azure SQL](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Konfigurowanie maskowanie dynamiczne dane dla bazy danych za pomocą interfejsu API usługi REST

Zobacz [operacji dla baz danych Azure SQL](https://msdn.microsoft.com/library/dn505719.aspx).
