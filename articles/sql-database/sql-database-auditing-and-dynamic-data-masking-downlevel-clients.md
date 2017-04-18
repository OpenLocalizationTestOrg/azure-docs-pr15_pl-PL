<properties
    pageTitle="Obsługuje klientów niższego poziomu bazy danych SQL i końcowy IP zmiany Inspekcja | Microsoft Azure"
    description="Informacje o Obsługa klientów niższych poziomów bazy danych SQL i IP zmiany punkt końcowy dla inspekcja."
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/10/2016"
    ms.author="ronitr"/>

# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-auditing"></a>Baza danych SQL — Obsługa klientów niższych poziomów i IP punktu końcowego zmiany dotyczące inspekcji


[Inspekcja](sql-database-auditing-get-started.md) automatycznie sprawdza się z klientami SQL, które obsługują przekierowywanie TDS.


##<a id="subheading-1"></a>Obsługa klientów niższych poziomów

Wszelkie klienta, który wykonuje TDS 7.4 należy również obsługuje przekierowywania. Wyjątki to zawierać 4.0 JDBC, w którym funkcję przekierowywania nie jest w pełni obsługiwany i nie wprowadzono Tedious dla Node.JS, w których przekierowywania.

Dla "Klientów niższego poziomu" to znaczy których obsługa TDS 7.3 i poniżej - długość nazwy FQDN serwera w połączeniu ciąg wersji należy zmodyfikować:

Długość nazwy FQDN serwera oryginalny w parametrach połączenia: <*Nazwa serwera*>. database.windows.net

Długość nazwy FQDN serwera zmienione w parametrach połączenia: .database <*Nazwa serwera*>. **bezpieczny**. windows.net

Część listy "Klientów niższego poziomu" zawiera:

- .NET 4.0 i poniżej,
- ODBC 10.0 i poniżej.
- JDBC (podczas JDBC obsługuje TDS 7.4, funkcja przekierowania TDS nie jest w pełni obsługiwane)
- Nużące (w przypadku Node.JS)

**Uwaga:** Serwer powyżej modyfikacji FDQN może być przydatne również w odniesieniu do zasady SQL Server inspekcji bez konieczności kroku konfiguracji w każdej bazy danych (tymczasowe łagodzenia).

##<a id="subheading-2"></a>Punkt końcowy IP zmian podczas włączania inspekcji

Pamiętaj, że po włączeniu inspekcja punkt końcowy IP bazy danych zmieni się. Jeśli masz ustawienia zapory ściśle, zaktualizuj te ustawienia zapory odpowiednio.

Nowy punkt końcowy IP bazy danych zależy od obszaru bazy danych:

| Region bazy danych | Możliwe IP punkty końcowe |
|----------|---------------|
| Ameryka Północna w Chinach  | 139.217.29.176, 139.217.28.254 |
| Chiny wschód  | 42.159.245.65, 42.159.246.245 |
| Wschód Australia  | 104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Australia kopiec | 191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Brazylia Południowej  | 104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| Centralny Stany Zjednoczone  | 104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Wschodnioazjatyckie   | 23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| Wschodniej USA 2 | 104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Wschodniej Stany Zjednoczone   | 23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| Indie centralnej  | 104.211.98.219, 104.211.103.71 |
| Indie Płd.   | 104.211.227.102, 104.211.225.157 |
| Indie zachód  | 104.211.161.152, 104.211.162.21 |
| Japonia wschód   | 104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Japonia zachód    | 104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Ameryka Północna centralnej w Stanach Zjednoczonych  | 191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Europa Północna  | 104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Południowej centralnej Stany Zjednoczone  | 191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Kraje Azji Południowo  | 104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| Europa Zachodnia  | 104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| Zachód Stany Zjednoczone  | 191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Kanada Środkowa  | 13.88.248.106 |
| Wschód Kanada  |  40.86.227.82 |
