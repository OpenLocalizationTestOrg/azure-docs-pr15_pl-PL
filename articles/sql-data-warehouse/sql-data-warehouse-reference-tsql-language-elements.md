<properties
   pageTitle="Elementy języka SQL danych magazynu języku Transact-SQL | Microsoft Azure"
   description="Lista łączy do materiały dla elementów języka Transact-SQL używane dla magazynu danych SQL."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/08/2016"
   ms.author="barbkess;sonyama;kevin"/>

# <a name="language-elements"></a>Elementy języka

## <a name="core-elements"></a>Podstawowe elementy

- [Konwencje składni](https://msdn.microsoft.com/library/ms177563.aspx)
- [reguły nazewnictwa obiektów](https://msdn.microsoft.com/library/ms175874.aspx)
- [Zastrzeżone słowa kluczowe](https://msdn.microsoft.com/library/ms189822.aspx)
- [Sortowanie](https://msdn.microsoft.com/library/ff848763.aspx)
- [komentarze](https://msdn.microsoft.com/library/ms181627.aspx)
- [stałe](https://msdn.microsoft.com/library/ms179899.aspx)
- [typy danych](https://msdn.microsoft.com/library/ms187752.aspx)
- [WYKONYWANIE](https://msdn.microsoft.com/library/ms188332.aspx)
- [wyrażenia](https://msdn.microsoft.com/library/ms190286.aspx)
- [SKASOWAĆ](https://msdn.microsoft.com/library/ms173730.aspx)
- [Obejście właściwości tożsamości](https://msdn.microsoft.com/library/ms186775.aspx)
- [DRUKOWANIE](https://msdn.microsoft.com/library/ms176047.aspx)
- [UŻYJ](https://msdn.microsoft.com/library/ms188366.aspx)

## <a name="batches-control-of-flow-and-variables"></a>Partie, przepływ sterowania i zmiennych

- [ZACZNIJ... KONIEC](https://msdn.microsoft.com/library/ms190487.aspx)
- [PODZIAŁ](https://msdn.microsoft.com/library/ms181271.aspx)
- [DEKLAROWANIE@local_variable](https://msdn.microsoft.com/library/ms188927.aspx)
- [JEŚLI... JESZCZE](https://msdn.microsoft.com/library/ms182717.aspx)
- [RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx)
- [SET@local_variable](https://msdn.microsoft.com/library/ms189484.aspx)
- [ZGŁOŚ](https://msdn.microsoft.com/library/ee677615.aspx)
- [SPRÓBUJ WYKONAĆ... EFEKTYWNEJ](https://msdn.microsoft.com/library/ms175976.aspx)
- [PODCZAS](https://msdn.microsoft.com/library/ms178642.aspx)

## <a name="operators"></a>Operatory
- [+ (Dodaj)](https://msdn.microsoft.com/library/ms178565.aspx)
- [+ (Łączenie ciągów)](https://msdn.microsoft.com/library/ms177561.aspx)
- [-(Negacja)](https://msdn.microsoft.com/library/ms189480.aspx)
- [-(Odejmowanie)](https://msdn.microsoft.com/library/ms189518.aspx)
- [* (Mnożenie)](https://msdn.microsoft.com/library/ms176019.aspx)
- [/ (Dzielenie)](https://msdn.microsoft.com/library/ms175009.aspx)
- [Modulo](https://msdn.microsoft.com/library/ms190279.aspx)

## <a name="wildcard-characters-to-match"></a>Znaki symboli wieloznacznych do dopasowania
- [= (Równa się)](https://msdn.microsoft.com/library/ms175118.aspx)
- [> (Większe niż)](https://msdn.microsoft.com/library/ms178590.aspx)
- [< (Mniejsze niż)](https://msdn.microsoft.com/library/ms179873.aspx)
- [> = (wielkiej niż lub równe)](https://msdn.microsoft.com/library/ms181567.aspx)
- [< = (mniejsze niż lub równe)](https://msdn.microsoft.com/library/ms174978.aspx)
- [<> (Nie równa się)](https://msdn.microsoft.com/library/ms176020.aspx)
- [! = (Nie równa się)](https://msdn.microsoft.com/library/ms190296.aspx)
- [ORAZ](https://msdn.microsoft.com/library/ms188372.aspx)
- [MIĘDZY](https://msdn.microsoft.com/library/ms187922.aspx)
- [ISTNIEJE](https://msdn.microsoft.com/library/ms188336.aspx)
- [W](https://msdn.microsoft.com/library/ms177682.aspx)
- [[NOT] MA WARTOŚĆ NULL](https://msdn.microsoft.com/library/ms188795.aspx)
- [PRZYKŁAD](https://msdn.microsoft.com/library/ms179859.aspx)
- [NIE](https://msdn.microsoft.com/library/ms189455.aspx)
- [LUB](https://msdn.microsoft.com/library/ms188361.aspx)

### <a name="bitwise-operators"></a>Operatory operacji

- [& (Operacji oraz)](https://msdn.microsoft.com/library/ms174965.aspx)
- [| (Lub operacji)](https://msdn.microsoft.com/library/ms186714.aspx)
- [^ (Bitowej lub wyłączności)](https://msdn.microsoft.com/library/ms190277.aspx)
- [~ (Nie operacji)](https://msdn.microsoft.com/library/ms173468.aspx)
- [^ = (Z wyłączeniem operacji lub równa się)](https://msdn.microsoft.com/library/cc627413.aspx)
- [| = (Operacji lub równe)](https://msdn.microsoft.com/library/cc627409.aspx)
- [& = (równa się operacji oraz)](https://msdn.microsoft.com/library/cc627427.aspx)

## <a name="functions"></a>Funkcje

- [@@DATEFIRST](https://msdn.microsoft.com/library/ms187766.aspx)
- [@@ERROR](https://msdn.microsoft.com/library/ms188790.aspx)
- [@@LANGUAGE](https://msdn.microsoft.com/library/ms177557.aspx)
- [@@SPID](https://msdn.microsoft.com/library/ms189535.aspx)
- [@@TRANCOUNT](https://msdn.microsoft.com/library/ms187967.aspx)
- [@@VERSION](https://msdn.microsoft.com/library/ms177512.aspx)
- [MODUŁ.LICZBY](https://msdn.microsoft.com/library/ms189800.aspx)
- [ACOS](https://msdn.microsoft.com/library/ms178627.aspx)
- [ASCII](https://msdn.microsoft.com/library/ms177545.aspx)
- [ASIN](https://msdn.microsoft.com/library/ms181581.aspx)
- [ATAN](https://msdn.microsoft.com/library/ms181746.aspx)
- [ATN2](https://msdn.microsoft.com/library/ms173854.aspx)
- [BINARY_CHECKSUM](https://msdn.microsoft.com/library/ms173784.aspx)
- [WIELKOŚĆ LITER](https://msdn.microsoft.com/library/ms181765.aspx)
- [CAST i CONVERT](https://msdn.microsoft.com/library/ms187928.aspx)
- [ZAOKR.W.GÓRĘ](https://msdn.microsoft.com/library/ms189818.aspx)
- [ZNAK](https://msdn.microsoft.com/library/ms187323.aspx)
- [FUNKCJA CHARINDEX](https://msdn.microsoft.com/library/ms186323.aspx)
- [SUMA KONTROLNA](https://msdn.microsoft.com/library/ms189788.aspx)
- [POŁĄCZENIE](https://msdn.microsoft.com/library/ms190349.aspx)
- [COL_NAME](https://msdn.microsoft.com/library/ms174974.aspx)
- [COLLATIONPROPERTY](https://msdn.microsoft.com/library/ms190305.aspx)
- ["CONCAT"](https://msdn.microsoft.com/library/hh231515.aspx)
- [COS](https://msdn.microsoft.com/library/ms188919.aspx)
- [COT](https://msdn.microsoft.com/library/ms188921.aspx)
- [LICZBA](https://msdn.microsoft.com/library/ms175997.aspx)
- [COUNT_BIG](https://msdn.microsoft.com/library/ms190317.aspx)
- [CUME_DIST](https://msdn.microsoft.com/library/hh231078.aspx)
- [CURRENT_TIMESTAMP](https://msdn.microsoft.com/library/ms188751.aspx)
- [CURRENT_USER](https://msdn.microsoft.com/library/ms176050.aspx)
- [DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx)
- [DŁUGOŚĆ_DANYCH](https://msdn.microsoft.com/library/ms173486.aspx)
- [DATEADD](https://msdn.microsoft.com/library/ms186819.aspx)
- [DATEDIFF](https://msdn.microsoft.com/library/ms189794.aspx)
- [DATEFROMPARTS](https://msdn.microsoft.com/library/hh213228.aspx)
- [DATENAME](https://msdn.microsoft.com/library/ms174395.aspx)
- [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx)
- [DATETIME2FROMPARTS](https://msdn.microsoft.com/library/hh213312.aspx)
- [DATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213233.aspx)
- [DATETIMEOFFSETFROMPARTS](https://msdn.microsoft.com/library/hh231077.aspx)
- [DZIEŃ](https://msdn.microsoft.com/library/ms176052.aspx)
- [DB_ID](https://msdn.microsoft.com/library/ms186274.aspx)
- [DB_NAME](https://msdn.microsoft.com/library/ms189753.aspx)
- [STOPNIE](https://msdn.microsoft.com/library/ms178566.aspx)
- [DENSE_RANK](https://msdn.microsoft.com/library/ms173825.aspx)
- [RÓŻNICA](https://msdn.microsoft.com/library/ms188753.aspx)
- [FUNKCJA EOMONTH](https://msdn.microsoft.com/library/hh213020.aspx)
- [ERROR_MESSAGE](https://msdn.microsoft.com/library/ms190358.aspx)
- [ERROR_NUMBER](https://msdn.microsoft.com/library/ms175069.aspx)
- [ERROR_PROCEDURE](https://msdn.microsoft.com/library/ms188398.aspx)
- [ERROR_SEVERITY](https://msdn.microsoft.com/library/ms178567.aspx)
- [ERROR_STATE](https://msdn.microsoft.com/library/ms180031.aspx)
- [EXP](https://msdn.microsoft.com/library/ms179857.aspx)
- [FIRST_VALUE](https://msdn.microsoft.com/library/hh213018.aspx)
- [POWIERZCHNIA](https://msdn.microsoft.com/library/ms178531.aspx)
- [GETDATE](https://msdn.microsoft.com/library/ms188383.aspx)
- [GETUTCDATE](https://msdn.microsoft.com/library/ms178635.aspx)
- [HAS_DBACCESS](https://msdn.microsoft.com/library/ms187718.aspx)
- [HASHBYTES](https://msdn.microsoft.com/library/ms174415.aspx)
- [INDEXPROPERTY](https://msdn.microsoft.com/library/ms187729.aspx)
- [FUNKCJA ISDATE](https://msdn.microsoft.com/library/ms187347.aspx)
- [ISNULL](https://msdn.microsoft.com/library/ms184325.aspx)
- [FUNKCJA ISNUMERIC](https://msdn.microsoft.com/library/ms186272.aspx)
- [ZWŁOKI](https://msdn.microsoft.com/library/hh231256.aspx)
- [LAST_VALUE](https://msdn.microsoft.com/library/hh231517.aspx)
- [POTENCJALNY KLIENT](https://msdn.microsoft.com/library/hh213125.aspx)
- [LEWEJ](https://msdn.microsoft.com/library/ms177601.aspx)
- [FUNKCJA DŁ](https://msdn.microsoft.com/library/ms190329.aspx)
- [LOG](https://msdn.microsoft.com/library/ms190319.aspx)
- [LOG10](https://msdn.microsoft.com/library/ms175121.aspx)
- [DOLNYM](https://msdn.microsoft.com/library/ms174400.aspx)
- [FUNKCJE LTRIM](https://msdn.microsoft.com/library/ms177827.aspx)
- [MAX](https://msdn.microsoft.com/library/ms187751.aspx)
- [MIN](https://msdn.microsoft.com/library/ms179916.aspx)
- [MIESIĄC](https://msdn.microsoft.com/library/ms187813.aspx)
- [NCHAR](https://msdn.microsoft.com/library/ms182673.aspx)
- [NTILE](https://msdn.microsoft.com/library/ms175126.aspx)
- [NULLIF](https://msdn.microsoft.com/library/ms177562.aspx)
- [OBJECT_ID](https://msdn.microsoft.com/library/ms190328.aspx)
- [: NAZWA_OBIEKTU](https://msdn.microsoft.com/library/ms186301.aspx)
- [OBJECTPROPERTY](https://msdn.microsoft.com/library/ms176105.aspx)
- [OIBJECTPROPERTYEX](https://msdn.microsoft.com/library/ms188390.aspx)
- [Funkcje skalarne ODBCS](https://msdn.microsoft.com/library/bb630290.aspx)
- [Klauzula OVER](https://msdn.microsoft.com/library/ms189461.aspx)
- [PARSENAME](https://msdn.microsoft.com/library/ms188006.aspx)
- [PATINDEX](https://msdn.microsoft.com/library/ms188395.aspx)
- [PERCENTILE_CONT](https://msdn.microsoft.com/library/hh231473.aspx)
- [PERCENTILE_DISC](https://msdn.microsoft.com/library/hh231327.aspx)
- [PERCENT_RANK](https://msdn.microsoft.com/library/hh213573.aspx)
- [PI](https://msdn.microsoft.com/library/ms189512.aspx)
- [POWER](https://msdn.microsoft.com/library/ms174276.aspx)
- [QUOTENAME](https://msdn.microsoft.com/library/ms176114.aspx)
- [RADIANY](https://msdn.microsoft.com/library/ms189742.aspx)
- [LOS](https://msdn.microsoft.com/library/ms177610.aspx)
- [POZYCJA](https://msdn.microsoft.com/library/ms176102.aspx)
- [ZAMIENIANIE](https://msdn.microsoft.com/library/ms186862.aspx)
- [REPLIKACJI](https://msdn.microsoft.com/library/ms174383.aspx)
- [ODWROTNA KOLEJNOŚĆ](https://msdn.microsoft.com/library/ms180040.aspx)
- [PRAWO](https://msdn.microsoft.com/library/ms177532.aspx)
- [ZAOKR](https://msdn.microsoft.com/library/ms175003.aspx)
- [ROW_NUMBER](https://msdn.microsoft.com/library/ms186734.aspx)
- [RTRIM](https://msdn.microsoft.com/library/ms178660.aspx)
- [SCHEMA_ID](https://msdn.microsoft.com/library/ms188797.aspx)
- [SCHEMA_NAME](https://msdn.microsoft.com/library/ms175068.aspx)
- [SERVERPROPERTY](https://msdn.microsoft.com/library/ms174396.aspx)
- [SESSION_USER](https://msdn.microsoft.com/library/ms177587.aspx)
- [ZNAK](https://msdn.microsoft.com/library/ms188420.aspx)
- [SIN](https://msdn.microsoft.com/library/ms188377.aspx)
- [SMALLDATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213396.aspx)
- [SOUNDEX](https://msdn.microsoft.com/library/ms187384.aspx)
- [MIEJSCA](https://msdn.microsoft.com/library/ms187950.aspx)
- [SQL_VARIANT_PROPERTY](https://msdn.microsoft.com/library/ms178550.aspx)
- [PIERWIASTEK](https://msdn.microsoft.com/library/ms176108.aspx)
- [KWADRATOWY](https://msdn.microsoft.com/library/ms173569.aspx)
- [STATS_DATE](https://msdn.microsoft.com/library/ms190330.aspx)
- [ODCH.STANDARDOWE](https://msdn.microsoft.com/library/ms190474.aspx)
- [ODCH.STANDARD.POPUL](https://msdn.microsoft.com/library/ms176080.aspx)
- [STR](https://msdn.microsoft.com/library/ms189527.aspx)
- [RZECZY](https://msdn.microsoft.com/library/ms188043.aspx)
- [PODCIĄG](https://msdn.microsoft.com/library/ms187748.aspx)
- [SUMA](https://msdn.microsoft.com/library/ms187810.aspx)
- [SUSER_SNAME](https://msdn.microsoft.com/library/ms174427.aspx)
- [SWITCHOFFSET](https://msdn.microsoft.com/library/bb677244.aspx)
- [SYSDATETIME](https://msdn.microsoft.com/library/bb630353.aspx)
- [SYSDATETIMEOFFSET](https://msdn.microsoft.com/library/bb677334.aspx)
- [SYSTEM_USER](https://msdn.microsoft.com/library/ms179930.aspx)
- [SYSUTCDATETIME](https://msdn.microsoft.com/library/bb630387.aspx)
- [TAN](https://msdn.microsoft.com/library/ms190338.aspx)
- [TERTIARY_WEIGHTS](https://msdn.microsoft.com/library/ms186881.aspx)
- [TIMEFROMPARTS](https://msdn.microsoft.com/library/hh213398.aspx)
- [FUNKCJA TODATETIMEOFFSET](https://msdn.microsoft.com/library/bb630335.aspx)
- [TYPE_ID](https://msdn.microsoft.com/library/ms181628.aspx)
- [TYPE_NAME](https://msdn.microsoft.com/library/ms189750.aspx)
- [TYPEPROPERTY](https://msdn.microsoft.com/library/ms188419.aspx)
- [UNICODE](https://msdn.microsoft.com/library/ms180059.aspx)
- [GÓRNA](https://msdn.microsoft.com/library/ms180055.aspx)
- [UŻYTKOWNIKA](https://msdn.microsoft.com/library/ms186738.aspx)
- [NAZWA_UŻYTKOWNIKA](https://msdn.microsoft.com/library/ms188014.aspx)
- [WARIANCJA](https://msdn.microsoft.com/library/ms186290.aspx)
- [WARIANCJA](https://msdn.microsoft.com/library/ms188735.aspx)
- [ROK](https://msdn.microsoft.com/library/ms186313.aspx)
- [XACT_STATE](https://msdn.microsoft.com/library/ms189797.aspx)

## <a name="transactions"></a>Transakcje

- [transakcje](https://msdn.microsoft.com/library/mt204031.aspx)

## <a name="diagnostic-sessions"></a>Sesje diagnostyczne

- [TWORZENIE SESJI DIAGNOSTYKI](https://msdn.microsoft.com/library/mt204029.aspx)

## <a name="procedures"></a>Procedury

- [sp_addrolemember](https://msdn.microsoft.com/library/ms187750.aspx)
- [sp_columns](https://msdn.microsoft.com/library/ms176077.aspx)
- [sp_configure](https://msdn.microsoft.com/library/ms188787.aspx)
- [sp_datatype_info_90](https://msdn.microsoft.com/library/mt204014.aspx)
- [sp_droprolemember](https://msdn.microsoft.com/library/ms188369.aspx)
- [sp_execute](https://msdn.microsoft.com/library/ff848746.aspx)
- [sp_executesql](https://msdn.microsoft.com/library/ms188001.aspx)
- [sp_fkeys](https://msdn.microsoft.com/library/ms175090.aspx)
- [sp_pdw_add_network_credentials](https://msdn.microsoft.com/library/mt204011.aspx)
- [sp_pdw_database_encryption](https://msdn.microsoft.com/library/mt219360.aspx)
- [sp_pdw_database_encryption_regenerate_system_keys](https://msdn.microsoft.com/library/mt204033.aspx)
- [sp_pdw_log_user_data_masking](https://msdn.microsoft.com/library/mt204023.aspx)
- [sp_pdw_remove_network_credentials](https://msdn.microsoft.com/library/mt204038.aspx)
- [sp_pkeys](https://msdn.microsoft.com/library/ms189813.aspx)
- [sp_prepare](https://msdn.microsoft.com/library/ff848808.aspx)
- [sp_spaceused](https://msdn.microsoft.com/library/ms188776.aspx)
- [sp_special_columns_100](https://msdn.microsoft.com/library/mt204025.aspx)
- [sp_sproc_columns](https://msdn.microsoft.com/library/ms182705.aspx)
- [sp_statistics](https://msdn.microsoft.com/library/ms173842.aspx)
- [sp_tables](https://msdn.microsoft.com/library/ms186250.aspx)
- [sp_unprepare](https://msdn.microsoft.com/library/ff848735.aspx)



## <a name="set-statements"></a>Instrukcje SET

- [USTAWIANIE ANSI_DEFAULTS](https://msdn.microsoft.com/library/ms188340.aspx)
- [USTAWIANIE ANSI_NULL_DFLT_OFF](https://msdn.microsoft.com/library/ms187356.aspx)
- [USTAWIANIE ANSI_NULL_DFLT_ON](https://msdn.microsoft.com/library/ms187375.aspx)
- [USTAWIANIE ANSI_NULLS](https://msdn.microsoft.com/library/ms188048.aspx)
- [USTAWIANIE ANSI_PADDING](https://msdn.microsoft.com/library/ms187403.aspx)
- [USTAWIANIE ANSI_WARNINGS](https://msdn.microsoft.com/library/ms190368.aspx)
- [USTAWIANIE ARITHABORT](https://msdn.microsoft.com/library/ms190306.aspx)
- [USTAWIANIE ARITHIGNORE](https://msdn.microsoft.com/library/ms184341.aspx)
- [INSTRUKCJA SET CONCAT_NULL_YIELDS_NULL](https://msdn.microsoft.com/library/ms176056.aspx)
- [USTAWIANIE DATEFIRST](https://msdn.microsoft.com/library/ms181598.aspx)
- [USTAWIANIE FORMAT_DATY](https://msdn.microsoft.com/library/ms189491.aspx)
- [USTAWIANIE FMTONLY](https://msdn.microsoft.com/library/ms173839.aspx)
- [USTAWIANIE IMPLICIT_TRANSACITONS](https://msdn.microsoft.com/library/ms187807.aspx)
- [USTAWIANIE LOCK_TIMEOUT](https://msdn.microsoft.com/library/ms189470.aspx)
- [USTAWIANIE NUMBERIC_ROUNDABORT](https://msdn.microsoft.com/library/ms188791.aspx)
- [USTAWIANIE QUOTED_IDENTIFIER](https://msdn.microsoft.com/library/ms174393.aspx)
- [USTAWIANIE ROWCOUNT](https://msdn.microsoft.com/library/ms188774.aspx)
- [USTAWIANIE TEXTSIZE](https://msdn.microsoft.com/library/ms186238.aspx)
- [USTAWIANIE POZIOMU IZOLACJI TRANSAKCJI](https://msdn.microsoft.com/library/ms173763.aspx)
- [USTAWIANIE XACT_ABORT](https://msdn.microsoft.com/library/ms188792.aspx)


## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacje zobacz [Omówienie magazynu danych SQL][].

<!--Image references-->

<!--Article references-->
[Omówienie magazynu danych SQL]: sql-data-warehouse-overview-reference.md

<!--MSDN references-->
