<properties
   pageTitle="Wprowadzenie do programu SQL bazy danych dynamicznych danych maskowanie (Azure Portal)"
   description="Jak rozpocząć pracę z SQL bazy danych dynamicznych danych maskowanie w Azure Portal"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>


# <a name="get-started-with-sql-database-dynamic-data-masking-azure-portal"></a>Wprowadzenie do programu SQL bazy danych dynamicznych danych maskowanie (Azure Portal)

> [AZURE.SELECTOR]
- [Dynamiczne dane maskowanie — Portal Azure klasyczny](sql-database-dynamic-data-masking-get-started-portal.md)

## <a name="overview"></a>Omówienie

SQL baza danych dynamicznych danych maskowanie ogranicza Uwidacznianie ważnych danych przez maskowanie go do użytkowników bez uprawnień. Dynamiczne dane maskowanie jest obsługiwana w wersji 12 wersji bazy danych SQL Azure.

Dynamiczne dane maskowanie ułatwia zapobieganie nieupoważnionemu dostępowi do ważnych danych, włączając klientów wyznaczyć stopień poufnych danych w celu uwidocznienia z minimalnymi wpływ na warstwie aplikacji. Jest funkcją na podstawie zasad zabezpieczeń, który spowoduje ukrycie poufne dane w zestawie wyników kwerendy na pola wyznaczonych bazy danych, gdy nie zostanie zmieniony w bazie danych.

Na przykład z przedstawicielem biura obsługi pośrodku połączenia może być identyfikowanie osób dzwoniących przez kilka cyfr numeru karty kredytowej lub numer PESEL, ale te elementy danych powinien nie pełni podsumowującymi z przedstawicielem działu obsługi. Czy wszystkie maski, ale ostatnich czterech cyfr numeru PESEL ani numeru karty kredytowej w wyniku zestaw jakiejkolwiek kwerendy można zdefiniować reguły maskowanie. Inny przykład maski odpowiednich danych można zdefiniować do ochrony danych identyfikowalne dane osobowe, tak, aby deweloper może wykonywać kwerendy w środowisku produkcyjnym w celu rozwiązywania problemów bez naruszenie zgodności z przepisami.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL baza danych dynamicznych danych maskowanie — informacje podstawowe

Możesz skonfigurować dynamiczne dane maskowanie zasad w Azure Portal, wybierając operację maskowanie danych dynamicznych w karta konfiguracji bazy danych SQL lub karta Ustawienia.


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
| **Liczba losowa z zakresu** | **Maskowanie metodę, która generuje liczbę losową z zakresu** od wybranego ograniczenia i typy danych rzeczywistych. Jeśli wyznaczonych granic są równe, funkcja maskowanie będzie stałą liczbową.<br/><br/>![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Niestandardowy tekst** | **Maskowanie metodę, która udostępnia pierwsze i ostatnie znaki** i dodanie ciągu niestandardowe dopełnienia w środku. Jeśli ciąg oryginalny jest krótsza udostępnionej prefiks i sufiks, będzie używany tylko ciąg dopełnieniem. <br/>sufiks prefiks [dopełnienia]<br/><br/>![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>
### <a name="recommended-fields-to-mask"></a>Zalecane pola do maski

Aparat zalecenia DDM flagi dla niektórych pól z bazy danych jako potencjalnie poufne pola, które mogą być wskazane w przypadku maskowanie. W karta maskowanie danych dynamicznych w portalu zalecane kolumny będą widoczne dla bazy danych. To wszystko, co należy zrobić, kliknij przycisk **Dodaj maskę** dla jednej lub wielu kolumn, a następnie **zapisać** w celu zastosowania maski dla tych pól.

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Konfigurowanie maskowanie dynamiczne dane dla bazy danych przy użyciu Azure Portal

1. Uruchamianie portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com).

2. Przejdź do karta Ustawienia bazy danych, która zawiera dane poufne, które ma zostać utworzona maska.

3. Kliknij pozycję **Maskowanie danych dynamicznych** kafelków, co powoduje uruchomienie karta konfiguracji **Maskowanie danych dynamicznych** .

    * Możesz także przewiń w dół do sekcji **operacji** i kliknij pozycję **Maskowanie danych dynamicznych**.

    ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>


4. W karta konfiguracji **Dynamicznej maskowanie danych** może pojawić się kilka kolumn bazy danych jest oflagowane aparat zalecenia dla maskowanie. Aby zaakceptować zalecenia, po prostu kliknij pozycję **Dodaj maskę** dla jednej lub wielu kolumn i maski zostanie utworzony w oparciu o domyślny typ dla tej kolumny. Funkcja maskowanie można zmienić, klikając w regule maskowanie i edytowania maskowanie format pola na inny format wybranych przez użytkownika. Należy kliknąć przycisk **Zapisz** , aby zapisać ustawienia.

    ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>


5. Aby dodać maskę dla każdej kolumny w bazie danych, w górnej części karta konfiguracji **Dynamicznej maskowanie danych** kliknij przycisk **Dodaj maskę** , aby otworzyć karta konfiguracji **Dodawanie reguły maskowanie**

    ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>

6. Wybierz **schemat**, **tabel** i **kolumn** do definiowania wyznaczonych pole, które zostaną ukryte.

7. Wybierz **Format maskowanie pola** z listy ważnych danych maskowanie kategorii.

    ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>     

8. W oknie danych maskowanie karta reguły, aby zaktualizować zestaw maskowanie reguły w danych dynamicznych maskowanie zasad, kliknij przycisk **Zapisz** .

9. Wpisz użytkowników SQL lub tożsamości AAD, które powinny być wyłączone z maskowanie i mają dostęp do danych poufnych zaznaczona. Należy to rozdzielaną średnikami listę użytkowników. Należy zauważyć, że użytkownicy z uprawnieniami administratora zawsze mają dostęp do oryginalne dane zaznaczona.

    ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)

    >[AZURE.TIP] Aby uniemożliwić warstwy aplikacji można wyświetlać dane poufne dla użytkowników aplikacji uprzywilejowane, Dodaj użytkownika SQL lub AAD tożsamości aplikacji używa do kwerendy bazy danych. Zdecydowanie zaleca się, że ta lista zawiera minimalnej liczby użytkowników uprzywilejowanych, aby zminimalizować ryzyko poufne dane.

10. Maskowanie karta konfiguracji, aby zapisać nową lub zaktualizowaną maskowanie zasady dotyczące danych, kliknij przycisk **Zapisz** .

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Konfigurowanie danych dynamicznych maskowanie bazy danych przy użyciu poleceń cmdlet programu Powershell

Zobacz [polecenia cmdlet bazy danych Azure SQL](https://msdn.microsoft.com/library/azure/mt574084.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Konfigurowanie maskowanie dynamiczne dane dla bazy danych za pomocą interfejsu API usługi REST

Zobacz [operacji dla baz danych Azure SQL](https://msdn.microsoft.com/library/dn505719.aspx).
