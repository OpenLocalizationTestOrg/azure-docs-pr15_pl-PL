<properties
    pageTitle="Azure AD Connect synchronizacją: funkcje, odwołania | Microsoft Azure"
    description="Odwołanie deklaracyjnych obsługi administracyjnej wyrażeń w narzędzie Azure AD Connect synchronizacji."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect synchronizacją: funkcje, odwołania

W narzędzie Azure AD Connect funkcje są używane do manipulowania wartość atrybutu podczas synchronizacji.  
Składnia funkcji jest wyrażona w następującym formacie:  
`<output type> FunctionName(<input type> <position name>, ..)`

Jeśli funkcja nadmiernie i akceptuje wielu składni, znajdują się wszystkie prawidłowe składni.  
Funkcje są jednoznacznie określony i okaże się, że typ przekazywane dopasowany udokumentowane typu.  
Jeśli typ nie odpowiada, jest generowany błąd.

Typy wyraża się przy użyciu następującej składni:

- **Kosz** — binarny
- **wartość logiczna** — wartość logiczna
- **dt** — UTC Data/Godzina
- **Wyliczenie** — wyliczanie znane stałych
- **exp** — wyrażenia, która ma być obliczona wartość logiczna
- **mvbin** — wielowartościowe binarny
- **mvstr** — ciąg wielowartościowe
- **mvref** — odwołanie wielowartościowe
- **num** — liczbowych
- **ref** — odwołania
- **str** — ciągu
- **Wariancja** — wartość typu Wariant (niemal) innego typu
- **void** — nie zwraca wartości

Funkcje z typów **mvbin**, **mvstr**i **mvref** tylko pracować nad atrybuty wielowartościowe. Funkcje z **Kosza**, **str**i **ref** pracować nad zarówno jedno- i wielowartościowe atrybuty.

## <a name="functions-reference"></a>Funkcje odwołania

Lista funkcji | | | | |  
--------- | --------- | --------- | --------- | --------- | ---------
**Konwersja** |  
[CBool](#cbool) | [CDate](#cdate) | [CGuid](#cguid) | [ConvertFromBase64](#convertfrombase64)
[ConvertToBase64](#converttobase64) | [ConvertFromUTF8Hex](#convertfromutf8hex) | [ConvertToUTF8Hex](#converttoutf8hex) | [CNum](#cnum)
[CRef](#cref) | [CStr](#cstr) | [StringFromGuid](#StringFromGuid) | [StringFromSid](#stringfromsid)
**Data / Godzina** |  
[DateAdd](#dateadd) | [DateFromNum](#datefromnum) | [FormatDateTime](#formatdatetime) | [Teraz](#now)
[NumFromDate](#numfromdate) |  
**Katalogu** |  
[DNComponent](#dncomponent) | [DNComponentRev](#dncomponentrev) | [EscapeDNComponent](#escapedncomponent)
**Oceny** |  
[IsBitSet](#isbitset) | [Funkcja IsDate](#isdate) | [Funkcja IsEmpty](#isempty) | [IsGuid](#isguid)
[IsNull](#isnull) | [IsNullOrEmpty](#isnullorempty) | [Funkcja IsNumeric](#isnumeric) | [IsPresent](#ispresent) |
[IsString](#isstring) |  
**Funkcje matematyczne** |  
[BitAnd](#bitand) | [BitOr](#bitor) | [RandomNum](#randomnum)
**Wiele wartości** |  
[Zawiera](#contains) | [Liczba](#count) | [Element](#item) | [ItemOrNull](#itemornull)
[Dołączanie do](#join) | [RemoveDuplicates](#removeduplicates) | [Dzielenie](#split) |
**Przepływ sterowania programu** |  
[Błąd](#error) | [IIF](#iif)  | [Przełączanie](#switch)
**Tekst** |  
[IDENTYFIKATOR GUID](#guid) | [InStr](#instr) | [Funkcja InStrRev](#instrrev) | [LCase](#lcase)
[Lewej](#left) | [Funkcja DŁ](#len) | [Funkcje LTrim](#ltrim) | [MID](#mid)
[PadLeft](#padleft) | [PadRight](#padright) | [PCase](#pcase) | [Zamienianie](#replace)
[ReplaceChars](#replacechars) | [Prawo](#right) | [RTrim](#rtrim) | [Przycinanie](#trim)
[UCase](#ucase) | [Program Word](#word)

----------
### <a name="bitand"></a>BitAnd

**Opis:**  
BitAnd, funkcja ustawia bitów określoną wartość.

**Składnia:**  
`num BitAnd(num value1, num value2)`

- Wartość1; wartość2: wartościami liczbowymi, które powinny być ze sobą tematyczne

**Uwagi:**  
Ta funkcja konwertuje postać binarna oba parametry i służy do ustawiania nieco:

- 0 - Jeśli co najmniej jedną z odpowiednich bitów w *maski* i *flagi* są równe 0
- 1 — w przypadku spełnienia obu odpowiednich bitów 1.

Innymi słowy we wszystkich przypadkach z wyjątkiem po odpowiednich bity obu parametrów wynoszą 1 zwraca 0.

**Przykład:**  
`BitAnd(&HF, &HF7)`  
Zwraca wartość 7, ponieważ ta wartość są interpretowane jako liczbę szesnastkową "F" i "F7".

----------
### <a name="bitor"></a>BitOr

**Opis:**  
BitOr, funkcja ustawia bitów określoną wartość.

**Składnia:**  
`num BitOr(num value1, num value2)`

- Wartość1; wartość2: wartościami liczbowymi, które powinny być zsumować logicznie ze sobą

**Uwagi:**  
Ta funkcja konwertuje obu parametrów na postać binarna i zestawów nieco 1, jeśli co najmniej jedną z odpowiednich bitów w maski i flagi są 1 i 0 w przypadku spełnienia obu odpowiednich bitów 0. Innymi słowy we wszystkich przypadkach z wyjątkiem miejsce, w którym odpowiednie bity obu parametrów są równe 0 zwraca 1.

----------
### <a name="cbool"></a>CBool

**Opis:**  
Funkcja CBool zwraca wartość logiczną według szacowane wyrażenia

**Składnia:**  
`bool CBool(exp Expression)`

**Uwagi:**  
Jeśli wyrażenie ma wartość różną od zera, a następnie CBool zwraca wartość PRAWDA, jeszcze zwraca wartość False.

**Przykład:**  
`CBool([attrib1] = [attrib2])`  

Zwraca wartość PRAWDA, jeśli oba atrybuty mają tę samą wartość.

----------
### <a name="cdate"></a>CDate

**Opis:**  
Funkcja CDate zwraca daty/godziny UTC z ciągu. Data/godzina nie jest typem natywnych atrybut synchronizacji, ale jest używany przez niektóre funkcje.

**Składnia:**  
`dt CDate(str value)`

- Wartość: Ciąg z daty, godziny i opcjonalnie strefy czasowej

**Uwagi:**  
Zwracany ciąg jest zawsze UTC.

**Przykład:**  
`CDate([employeeStartTime])`  
Czas rozpoczęcia zwraca wartość daty i godziny na podstawie których pracownika

`CDate("2013-01-10 4:00 PM -8")`  
Zwraca przedstawiający daty i godziny "2013-01-11 12:00 AM"

----------
### <a name="cguid"></a>CGuid

**Opis:**  
Funkcja CGuid konwertuje Reprezentacja tekstowa identyfikator GUID na jego postać binarna.

**Składnia:**  
`bin CGuid(str GUID)`

- Ciąg sformatowaną w ten sposób: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx lub {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

----------
### <a name="contains"></a>Zawiera

**Opis:**  
Funkcja Contains umożliwia znalezienie ciągu wewnątrz atrybut wielowartościowy

**Składnia:**  
`num Contains (mvstring attribute, str search)`-uwzględniania wielkości liter  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-uwzględniania wielkości liter

- Atrybut: atrybut wielowartościowy do wyszukiwania.
- wyszukiwanie: ciąg, aby znaleźć w atrybucie.
- Casetype: CaseInsensitive lub CaseSensitive.

Indeks zwraca wartość atrybutu wielowartościowego wykryto ciąg. zwracana jest wartość 0, jeśli nie można odnaleźć tego ciągu.

**Uwagi:**  
Atrybuty wielowartościowe ciągu wyszukiwania znajduje podciągów w wartości.  
Atrybuty odwołanie wyszukiwane ciąg musi dokładnie odpowiadać wartości, które mają być traktowane jako dopasowanie.

**Przykład:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Jeśli atrybutu proxyAddresses ma podstawowego adresu e-mail (wskazywana przez wielkie litery "SMTP:"), zwracany jest atrybut proxyAddress, jeszcze zwraca błąd.

----------
### <a name="convertfrombase64"></a>ConvertFromBase64

**Opis:**  
Funkcja ConvertFromBase64 konwertuje wartość określonej base64 zakodowany na zwykłą ciąg.

**Składnia:**  
`str ConvertFromBase64(str source)`-przyjmuje kodowanie Unicode  
`str ConvertFromBase64(str source, enum Encoding)`

- źródła: Ciąg zakodowany Base64  
- Kodowanie: UTF8 Unicode, ASCII,

**Przykład**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Zwracana obu przykładach "*Witaj świecie!*"

----------
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex

**Opis:**  
Funkcja ConvertFromUTF8Hex konwertuje określona wartość zakodowany szesnastkowy UTF8 na ciąg.

**Składnia:**  
`str ConvertFromUTF8Hex(str source)`

- źródła: UTF8 2-bajtowa zakodowany ciągu

**Uwagi:**  
Różnica między tej funkcji i ConvertFromBase64([],UTF8) w wynikiem jest przyjazne dla atrybutu nazwy domeny.  
Ten format jest używany przez usługi Azure Active Directory jako nazwy domeny.

**Przykład:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Zwraca wartość "*Witaj świecie!*"

----------
### <a name="converttobase64"></a>ConvertToBase64

**Opis:**  
Funkcja ConvertToBase64 konwertuje ciąg ciąg base64 Unicode.  
Konwertuje wartość argumentu tablica wartości całkowitych jego reprezentacją równoważną ciąg, który jest kodowane z cyframi base-64.

**Składnia:**  
`str ConvertToBase64(str source)`

**Przykład:**  
`ConvertToBase64("Hello world!")`  
Zwraca wartość "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

----------
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex

**Opis:**  
Funkcja ConvertToUTF8Hex konwertuje ciąg wartość szesnastkowy UTF8 zakodowany.

**Składnia:**  
`str ConvertToUTF8Hex(str source)`

**Uwagi:**  
Format wyjściowy ta funkcja jest używana przez usługi Azure Active Directory w formacie atrybut nazwy domeny.

**Przykład:**  
`ConvertToUTF8Hex("Hello world!")`  
Zwraca 48656C6C6F20776F726C6421

----------
### <a name="count"></a>Liczba

**Opis:**  
Ile.liczb, funkcja zwraca liczbę elementów w atrybut wielowartościowy

**Składnia:**  
`num Count(mvstr attribute)`

----------
### <a name="cnum"></a>CNum

**Opis:**  
Funkcja CNum ciąg znaków i zwraca typ danych liczbowych.

**Składnia:**  
`num CNum(str value)`

----------
### <a name="cref"></a>CRef

**Opis:**  
Konwertuje ciąg atrybut odwołania

**Składnia:**  
`ref CRef(str value)`

**Przykład:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

----------
### <a name="cstr"></a>CStr

**Opis:**  
Funkcja CStr konwertuje na typ danych string.

**Składnia:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

- wartość: mogą być wartości liczbowej, odwołanie atrybut lub wartość logiczna.

**Przykład:**  
`CStr([dn])`  
Może zwrócić "cn = Jerzy kontrolera domeny = contoso, kontrolera domeny = com"

----------
### <a name="dateadd"></a>DateAdd

**Opis:**  
Zwraca datę zawierającą datę, do której dodano określony interwał.

**Składnia:**  
`dt DateAdd(str interval, num value, dt date)`

- Interwał: ciąg wyrażenie interwałem czasu, którego chcesz dodać. Ciąg musi mieć jedno z następujących wartości:
 - yyyy rok
 - pytania kwartale.
 - m miesiąc
 - y dzień roku
 - d dzień
 - w dzień tygodnia
 - ww tydzień
 - h Godzina
 - Minuta n
 - s drugi
- wartość: liczba jednostek, które chcesz dodać. Może być dodatnie (Aby uzyskać dat w przyszłości) lub ujemna (w celu otrzymania daty w przeszłości).
- Data: reprezentującą datę, do którego zostanie dodany Interwał daty/godziny.

**Przykład:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Dodaje 3 miesiące i zwraca DateTime reprezentującą "2001-04-01".

----------
### <a name="datefromnum"></a>DateFromNum

**Opis:**  
Konwertuje funkcja DateFromNum wartość daty Directory formatowanie do typu Data/Godzina.

**Składnia:**  
`dt DateFromNum(num value)`

**Przykład:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Zwraca wartość DateTime, reprezentującą 2012-01-01-23:00:00

----------
### <a name="dncomponent"></a>DNComponent

**Opis:**  
Funkcja DNComponent zwraca wartość określonego składnika nazwy domeny, przechodząc od lewej.

**Składnia:**  
`str DNComponent(ref dn, num ComponentNumber)`

- nazwy domeny: atrybut odwołania interpretować
- ComponentNumber: Składnik nazwy domeny, aby zwrócić

**Przykład:**  
`DNComponent([dn],1)`  
W przypadku nazwy domeny "cn = Jerzy ou =...," zwraca Joe

----------
### <a name="dncomponentrev"></a>DNComponentRev

**Opis:**  
Funkcja DNComponentRev zwraca wartość określonego składnika nazwy domeny, przechodząc od prawej (Zakończ).

**Składnia:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- nazwy domeny: atrybut odwołania interpretować
- ComponentNumber - składnik nazwy domeny, aby zwrócić
- Opcje: Kontrolera domeny — Ignoruj wszystkie składniki z "kontrolera domeny ="

**Przykład:**  
W przypadku nazwy domeny "cn = Jerzy ou = Atlanta ou = GA, ou = US, kontrolera domeny = contoso, kontrolera domeny = com" następnie  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Obie zwracają US.

----------
### <a name="error"></a>Błąd

**Opis:**  
Wartość funkcji błędu jest używane do zwracania błędów niestandardowych.

**Składnia:**  
`void Error(str ErrorMessage)`

**Przykład:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Jeśli nie ma nazwę konta atrybut, zgłoś błąd obiektu.

----------
### <a name="escapedncomponent"></a>EscapeDNComponent

**Opis:**  
Funkcja EscapeDNComponent pobiera jeden składnik nazwy domen i powoduje go, aby są przedstawiane w LDAP.

**Składnia:**  
`str EscapeDNComponent(str value)`

**Przykład:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Służy do sprawdzenia, czy obiekt mogą być tworzone w katalogu LDAP, nawet jeśli atrybucie displayName zawiera znaki, które należy wyjściowym w LDAP.

----------
### <a name="formatdatetime"></a>FormatDateTime

**Opis:**  
Funkcja FormatDateTime jest używany do formatowania daty i godziny do ciągu z określonym formacie

**Składnia:**  
`str FormatDateTime(dt value, str format)`

- wartość: wartością w formacie daty/godziny
- Format: ciąg reprezentujący formatowanie, aby przekonwertować.

**Uwagi:**  
Możliwe wartości dla formatu można znaleźć tutaj: [Zdefiniowane formaty daty/godziny (funkcja Format)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Przykład:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
Zwraca wynik "2007-12-25".

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Może spowodować "20140905081453.0Z"

----------
### <a name="guid"></a>IDENTYFIKATOR GUID

**Opis:**  
Funkcja GUID generuje nowy identyfikator GUID losowe

**Składnia:**  
`str GUID()`

----------
### <a name="iif"></a>IIF

**Opis:**  
IIF, funkcja zwraca jedną z zestawu możliwych wartości na podstawie określonych warunków.

**Składnia:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- warunek: dowolna wartość lub wyrażenie, które może przyjąć wartość PRAWDA lub FAŁSZ.
- Wartość_dla_prawdy: Jeśli warunek ma wartość true, zwracana wartość.
- Wartość_dla_fałszu: Jeśli warunek ma wartość FAŁSZ, zwracana wartość.

**Przykład:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Jeśli użytkownik jest intern, zwraca alias użytkownika z"t" na początku go jeszcze zwraca alias użytkownika jest.

----------
### <a name="instr"></a>InStr

**Opis:**  
Funkcja InStr znajduje pierwsze wystąpienie ciąg w ciągu

**Składnia:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- ciąg_przeszukiwany: ciąg do przeszukania
- ciąg_poszukiwany: ciąg, aby znaleźć
- Rozpoczynanie: pozycja znajdowanie podciągu początkowa
- Porównanie: vbTextCompare lub vbBinaryCompare

**Uwagi:**  
Zwraca położenie, w którym został znaleziony podciąg lub 0, jeśli nie można odnaleźć.

**Przykład:**  
`InStr("The quick brown fox","quick")`  
Evalues do 5

`InStr("repEated","e",3,vbBinaryCompare)`  
Wynikiem jest 7

----------
### <a name="instrrev"></a>Funkcja InStrRev

**Opis:**  
Funkcja InStrRev znajduje ostatnie wystąpienie ciąg w ciągu

**Składnia:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- ciąg_przeszukiwany: ciąg do przeszukania
- ciąg_poszukiwany: ciąg, aby znaleźć
- Rozpoczynanie: pozycja znajdowanie podciągu początkowa
- Porównanie: vbTextCompare lub vbBinaryCompare

**Uwagi:**  
Zwraca położenie, w którym został znaleziony podciąg lub 0, jeśli nie można odnaleźć.

**Przykład:**  
`InStrRev("abbcdbbbef","bb")`  
Zwraca 7

----------
### <a name="isbitset"></a>IsBitSet

**Opis:**  
Funkcja testy IsBitSet, jeśli bit jest ustawiony lub nie

**Składnia:**  
`bool IsBitSet(num value, num flag)`

- wartość: wartości liczbowej, która jest evaluated.flag: wartości liczbowej, która zawiera wartość bitu ma zostać obliczone

**Przykład:**  
`IsBitSet(&HF,4)`  
Zwraca wartość PRAWDA, ponieważ bit "4" jest ustawiony na wartość szesnastkową "F"

----------
### <a name="isdate"></a>Funkcja IsDate

**Opis:**  
Jeśli wyrażenie może być oblicza jako typu Data/Godzina, a następnie funkcja IsDate ma wartość PRAWDA.

**Składnia:**  
`bool IsDate(var Expression)`

**Uwagi:**  
Umożliwia określenie, jeśli CDate() może być pomyślnie.

----------
### <a name="isempty"></a>Funkcja IsEmpty

**Opis:**  
Jeśli ten atrybut znajduje się w KSW lub MV, ale daje w wyniku ciąg pusty, funkcja IsEmpty oblicza wartość PRAWDA.

**Składnia:**  
`bool IsEmpty(var Expression)`

----------
### <a name="isguid"></a>IsGuid

**Opis:**  
Jeśli można przekonwertować ciągu na identyfikator GUID, funkcja IsGuid Szacowana wartość true.

**Składnia:**  
`bool IsGuid(str GUID)`

**Uwagi:**  
Identyfikator GUID jest definiowana jako ciąg, wykonując jedną z następujących wzorów: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx lub {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Umożliwia określenie, jeżeli CGuid() może się niepowodzeniem.

**Przykład:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Jeśli StrAttribute formatem GUID, zwracana postać binarna, w przeciwnym razie zwraca wartość Null.

----------
### <a name="isnull"></a>IsNull

**Opis:**  
Jeśli wyrażenie ma wartość Null, funkcja IsNull zwraca wartość true.

**Składnia:**  
`bool IsNull(var Expression)`

**Uwagi:**  
Atrybut Null jest wyrażona z powodu braku atrybutu.

**Przykład:**  
`IsNull([displayName])`  
Zwraca wartość True, jeśli ten atrybut nie ma w KSW lub MV.

----------
### <a name="isnullorempty"></a>IsNullOrEmpty

**Opis:**  
Jeśli wyrażenie jest wartość null lub ciąg pusty, funkcja IsNullOrEmpty zwraca wartość true.

**Składnia:**  
`bool IsNullOrEmpty(var Expression)`

**Uwagi:**  
Atrybut to chcesz zwrócić wartość True, jeśli ten atrybut istnieje lub jest zainstalowany, ale jest pusty ciąg.  
Odwrotność funkcji nosi nazwę IsPresent.

**Przykład:**  
`IsNullOrEmpty([displayName])`  
Zwraca wartość True, jeśli ten atrybut nie ma lub jest pusty ciąg w KSW lub MV.

----------
### <a name="isnumeric"></a>Funkcja IsNumeric

**Opis:**  
Funkcja IsNumeric zwraca wartość logiczną wskazującą, czy wyrażenie może być obliczana jako typ Liczba.

**Składnia:**  
`bool IsNumeric(var Expression)`

**Uwagi:**  
Umożliwia określenie, jeśli CNum() może być pomyślnie przeanalizować wyrażenia.

----------
### <a name="isstring"></a>IsString

**Opis:**  
Jeśli wyrażenie może przyjąć typu ciąg, następnie funkcja IsString ma wartość True.

**Składnia:**  
`bool IsString(var expression)`

**Uwagi:**  
Umożliwia określenie, jeśli CStr() może być pomyślnie przeanalizować wyrażenia.

----------
### <a name="ispresent"></a>IsPresent

**Opis:**  
Jeśli wyrażenie ma ciąg, który jest inna niż zero i nie jest pusta, funkcja IsPresent zwraca wartość true.

**Składnia:**  
`bool IsPresent(var expression)`

**Uwagi:**  
Odwrotność funkcji nosi nazwę IsNullOrEmpty.

**Przykład:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

----------
### <a name="item"></a>Element

**Opis:**  
Funkcja element zwraca jeden element z wielowartościowych ciąg/atrybutu.

**Składnia:**  
`var Item(mvstr attribute, num index)`

- Atrybut: atrybut wielowartościowy
- Indeks: indeks do elementu w ciągu wiele wartości.

**Uwagi:**  
Funkcja elementu jest przydatna razem z funkcji zawiera, ponieważ ostatnie funkcja zwraca indeks do elementu w atrybucie wielowartościowym.

Jeśli indeks znajduje się poza zakresem, zgłasza błąd.

**Przykład:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Zwraca podstawowy adres e-mail.

----------
### <a name="itemornull"></a>ItemOrNull

**Opis:**  
Funkcja ItemOrNull zwraca jeden element z wielowartościowe ciąg/atrybutu.

**Składnia:**  
`var ItemOrNull(mvstr attribute, num index)`

- Atrybut: atrybut wielowartościowy
- Indeks: indeks do elementu w ciągu wielowartościowych.

**Uwagi:**  
Funkcja ItemOrNull jest przydatna razem z funkcji zawiera, ponieważ ostatnie funkcja zwraca indeks do elementu w atrybucie wielowartościowym.

Jeśli indeks znajduje się poza zakresem, następnie zwraca wartość Null.

----------
### <a name="join"></a>Dołączanie do

**Opis:**  
Funkcja sprzężenia wielowartościowe ciąg znaków i zwraca wartość typu ciąg pojedynczej wartości z określonej separator wstawiona między znakami każdego elementu.

**Składnia:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

- Atrybut: atrybut wielowartościowy zawierający ciągów mają zostać połączone.
- ogranicznika: dowolny ciąg, używanym do rozdzielania podciągów w zwracanym ciągu. Jeśli pominięty, znak spacji ("") jest używana. W przypadku ogranicznika ciąg o zerowej długości ("") lub nic się nie, wszystkie elementy na liście są łączone bez dodawania ograniczników.

**Uwagi**  
Istnieje spójności między funkcjami dołączania i podziału. Funkcja sprzężenia pobiera tablicę ciągów i łączy je za pomocą ciągu ogranicznika zwraca jeden ciąg znaków. Funkcja Podziel ciąg znaków i rozdziela go na ogranicznik, aby zwrócić tablica ciągów. Różnica klucza jest jednak, że sprzężenie można łączyć ze sobą ciągów dowolny ciąg ogranicznika, dzielenie tylko oddzielić ciągów za pomocą ogranicznika pojedynczy znak.

**Przykład:**  
`Join([proxyAddresses],",")`  
Może zwrócić:"SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

----------
### <a name="lcase"></a>LCase

**Opis:**  
Funkcja LCase konwertuje wszystkie znaki w ciągu na małe litery.

**Składnia:**  
`str LCase(str value)`

**Przykład:**  
`LCase("TeSt")`  
Zwraca ciąg "test".

----------
### <a name="left"></a>Lewej

**Opis:**  
Funkcja LEWY zwraca określoną liczbę znaków z lewej strony innego ciągu.

**Składnia:**  
`str Left(str string, num NumChars)`

- ciąg: zwrócenie znaków z ciągu znaków
- NumChars: Liczba identyfikująca liczby znaków, aby zwrócić od początku ciągu (od lewej)

**Uwagi:**  
Ciąg zawierający pierwszych znaków numChars w ciągu:

- Jeśli numChars = 0, pusty ciąg.
- Jeśli numChars < 0, zwróci ciąg wejściowy.
- Jeśli ciąg ma wartość null, zwraca ciąg pusty.

Jeśli ciąg zawiera mniej znaków niż liczba numChars określonych w, zwracany jest ciąg identyczne ciąg (zawierający wszystkie znaki w parametrze 1).

**Przykład:**  
`Left("John Doe", 3)`  
Zwraca wartość "Joh".

----------
### <a name="len"></a>Funkcja DŁ

**Opis:**  
Funkcja DŁ zwraca liczbę znaków w ciągu.

**Składnia:**  
`num Len(str value)`

**Przykład:**  
`Len("John Doe")`  
Zwraca 8

----------
### <a name="ltrim"></a>Funkcje LTrim

**Opis:**  
Funkcja LTrim usuwa spacje wiodące biały z ciągu.

**Składnia:**  
`str LTrim(str value)`

**Przykład:**  
`LTrim(" Test ")`  
Zwraca wartość "Test"

----------
### <a name="mid"></a>MID

**Opis:**  
Funkcja fragment.tekstu zwraca określoną liczbę znaków od określonej pozycji w ciągu.

**Składnia:**  
`str Mid(str string, num start, num NumChars)`

- ciąg: zwrócenie znaków z ciągu znaków
- Rozpoczynanie: Liczba identyfikująca początkowej umieść w ciągu na zwrócenie znaków z
- NumChars: Liczba identyfikująca liczby znaków, aby wrócić na pozycji w ciągu

**Uwagi:**  
Uruchom zwrotu numChars znaków, rozpoczynając od pozycji w ciągu.  
Ciąg zawierający numChars znaków od początku pozycji w ciągu:

- Jeśli numChars = 0, pusty ciąg.
- Jeśli numChars < 0, zwróci ciąg wejściowy.
- Jeśli start > długość ciągu, zwracana parametrów wejściowych.
- Jeśli start < = 0, zwrócić ciąg wejściowy.
- Jeśli ciąg ma wartość null, zwraca ciąg pusty.

Jeśli nie są numChar znaków pozostały w ciągu od początku położenie, tyle możliwie są zwracane znaki.

**Przykład:**  
`Mid("John Doe", 3, 5)`  
Zwraca wartość "hn nie".

`Mid("John Doe", 6, 999)`  
Zwraca wartość "Kowalski"

----------
### <a name="now"></a>Teraz

**Opis:**  
Funkcja Now zwraca wartość Data/Godzina określającą bieżącą datę i godzinę według daty i godziny na komputerze.

**Składnia:**  
`dt Now()`

----------
### <a name="numfromdate"></a>NumFromDate

**Opis:**  
Funkcja NumFromDate zwraca datę w formacie daty Directory.

**Składnia:**  
`num NumFromDate(dt value)`

**Przykład:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Zwraca 129699324000000000

----------
### <a name="padleft"></a>PadLeft

**Opis:**  
PadLeft funkcja lewej PAD typu ciąg do określonej długości przy użyciu znaku dostarczonych dopełnieniem.

**Składnia:**  
`str PadLeft(str string, num length, str padCharacter)`

- ciąg: ciąg do konsoli.
- długość: liczbę całkowitą odpowiadającą odpowiednią długość ciągu.
- padCharacter: ciąg składający się pojedynczego znaku ma zostać użyte jako znak konsoli

**Uwagi:**

- Jeśli długość ciągu jest mniejsza niż długość, padCharacter wielokrotnie jest dołączany do początku (po lewej) ciągu dopóki o długości równej długości.
- PadCharacter może być znak spacji, ale nie może być wartość null.
- Jeśli długość ciągu jest równa lub większa niż długość, ciąg jest zwracana bez zmian.
- Jeśli ciąg ma długość większe niż lub równa długości, zwracany jest ciąg identyczny z ciągu.
- Jeśli długość ciągu jest mniejsza niż długość, nowy ciąg odpowiednią długość zwracany ciąg zawierający dopełniony padCharacter.
- Jeśli ciąg ma wartość null, funkcja zwraca ciąg pusty.

**Przykład:**  
`PadLeft("User", 10, "0")`  
Zwraca wartość "000000User".

----------
### <a name="padright"></a>PadRight

**Opis:**  
PadRight funkcja prawej — PAD typu ciąg do określonej długości przy użyciu znaku dostarczonych dopełnieniem.

**Składnia:**  
`str PadRight(str string, num length, str padCharacter)`

- ciąg: ciąg do konsoli.
- długość: liczbę całkowitą odpowiadającą odpowiednią długość ciągu.
- padCharacter: ciąg składający się pojedynczego znaku ma zostać użyte jako znak konsoli

**Uwagi:**

- Jeśli długość ciągu jest mniejsza niż długość, następnie padCharacter wielokrotnie widnieje (po prawej) końca ciągu dopóki o długości równej długości.
- padCharacter może być znak spacji, ale nie może być wartość null.
- Jeśli długość ciągu jest równa lub większa niż długość, ciąg jest zwracana bez zmian.
- Jeśli ciąg ma długość większe niż lub równa długości, zwracany jest ciąg identyczny z ciągu.
- Jeśli długość ciągu jest mniejsza niż długość, nowy ciąg odpowiednią długość zwracany ciąg zawierający dopełniony padCharacter.
- Jeśli ciąg ma wartość null, funkcja zwraca ciąg pusty.

**Przykład:**  
`PadRight("User", 10, "0")`  
Zwraca wartość "User000000".

----------
### <a name="pcase"></a>PCase

**Opis:**  
Funkcja PCase konwertuje pierwszy znak każdego wyrazu rozdzielany spacjami w ciągu na wielkie litery, a wszystkie pozostałe znaki są konwertowane na małe litery.

**Składnia:**  
`String PCase(string)`

**Uwagi:**

- Ta funkcja nie umożliwia obecnie z.wielkiej przekonwertować wyraz, który jest całkowicie wielkie litery, takich jak skrót.

**Przykład:**  
`PCase("TEsT")`  
Zwraca wartość "Test".

`PCase(LCase("TEST"))`  
Zwraca wartość "Test"

----------
### <a name="randomnum"></a>RandomNum

**Opis:**  
Funkcja RandomNum zwraca liczbę losową z zakresu określonego czasu.

**Składnia:**  
`num RandomNum(num start, num end)`

- Rozpoczynanie: Liczba identyfikująca wykrywana losowe wartości do generowania
- Koniec: Liczba identyfikująca górną granicę losowe wartości do generowania

**Przykład:**  
`Random(100,999)`  
Można przywrócić 734.

----------
### <a name="removeduplicates"></a>RemoveDuplicates

**Opis:**  
Funkcja RemoveDuplicates wielowartościowe ciąg znaków i upewnij się, że każda wartość jest unikatowa.

**Składnia:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Przykład:**  
`RemoveDuplicates([proxyAddresses])`  
Zwraca wartość atrybutu oczyszczona proxyAddress miejsce, w którym zostały usunięte wszystkie zduplikowane wartości.

----------
### <a name="replace"></a>Zamienianie

**Opis:**  
Funkcja ZASTĄP zastępuje wszystkie wystąpienia ciągu do innego ciągu.

**Składnia:**  
`str Replace(str string, str OldValue, str NewValue)`

- ciąg: ciąg, aby zamienić wartości.
- OldValue: Ciąg aby wyszukać i zastąpić.
- Nowa_wartość: Ciąg Zamień na.

**Uwagi:**  
Funkcja rozpoznaje następujące monikerów specjalnych:

- \n — nowy wiersz
- \r — powrót karetki
- \t — karta

**Przykład:**  
`Replace([address],"\r\n",", ")`  
Zamienia CRLF jest przecinek i spacja i może prowadzić do "Jeden Microsoft sposób, Redmond, WA, USA"

----------
### <a name="replacechars"></a>ReplaceChars

**Opis:**  
Funkcja ReplaceChars zastępuje wszystkie wystąpienia znaków w ciągu ReplacePattern.

**Składnia:**  
`str ReplaceChars(str string, str ReplacePattern)`

- ciąg: Aby zamienić znaki w ciągu.
- ReplacePattern: ciąg zawierający słownik znakami Zamień.

{Źródło1} jest format: {nazwach target1}, {źródło2}: {target2}, {źródłoN}, {targetN} źródła w przypadku znaku Znajdowanie i docelowych ciąg, aby zamienić.

**Uwagi:**

- Funkcja przyjmuje każdego wystąpienia zdefiniowanych źródeł i zamienia celów.
- Źródło musi być dokładnie jeden znak (unicode).
- Źródło nie może być puste ani dłużej niż jeden znak (Błąd analizy).
- Obiekt docelowy może zawierać wiele znaków, na przykład ö:oe, β:ss.
- Celem może być pusta, wskazująca, że należy usunąć znak.
- Źródła uwzględniana jest wielkość liter i musi być dokładne dopasowanie.
- (Przecinek) i: (dwukropek) są zarezerwowane znaków i nie można zastąpić korzystania z tej funkcji.
- Spacje i inne białe znaki w ciągu ReplacePattern są ignorowane.

**Przykład:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Zwraca Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Zwraca wartość "ONeil", jeden znacznik jest definiowana do usunięcia.

----------
### <a name="right"></a>Prawo

**Opis:**  
Funkcja Right zwraca określoną liczbę znaków od prawej (Zakończ) w ciągu.

**Składnia:**  
`str Right(str string, num NumChars)`

- ciąg: zwrócenie znaków z ciągu znaków
- NumChars: Liczba identyfikująca liczby znaków, aby zwracało z (po prawej) końca ciągu

**Uwagi:**  
Od ostatniej pozycji ciągu są zwracane NumChars znaki.

Ciąg zawierający ostatnie znaki numChars w ciągu:

- Jeśli numChars = 0, pusty ciąg.
- Jeśli numChars < 0, zwróci ciąg wejściowy.
- Jeśli ciąg ma wartość null, zwraca ciąg pusty.

Jeśli ciąg zawiera mniej znaków niż liczba NumChars określonych w, zwracany jest ciąg identyczny z ciągu.

**Przykład:**  
`Right("John Doe", 3)`  
Zwraca wartość "Kowalski".

----------
### <a name="rtrim"></a>RTrim

**Opis:**  
Funkcja RTrim usuwa spacji końcowych z ciągu.

**Składnia:**  
`str RTrim(str value)`

**Przykład:**  
`RTrim(" Test ")`  
Zwraca wartość "Test".

----------
### <a name="split"></a>Dzielenie

**Opis:**  
Funkcja podziału oddzielonych ogranicznikiem ciąg znaków i ułatwia ciągu wiele wartości.

**Składnia:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

- wartość: ciąg z ogranicznika znak oddzielający.
- ogranicznika: pojedynczy znak może być używany jako ogranicznik.
- limit: Maksymalna liczba wartości, które może zwracać.

**Przykład:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Zwraca ciąg wielowartościowych z elementami 2 przydatne dla atrybutu proxyAddress.

----------
### <a name="stringfromguid"></a>StringFromGuid

**Opis:**  
Funkcja StringFromGuid przyjmuje binarny identyfikator GUID i konwertuje ją na ciąg

**Składnia:**  
`str StringFromGuid(bin GUID)`

----------
### <a name="stringfromsid"></a>StringFromSid

**Opis:**  
Funkcja StringFromSid konwertuje tablicy bajtów zawierającą identyfikator zabezpieczeń na ciąg.

**Składnia:**  
`str StringFromSid(bin ObjectSID)`  

----------
### <a name="switch"></a>Przełączanie

**Opis:**  
Funkcja Switch służy do zwracają pojedynczą wartość na podstawie warunków szacowane.

**Składnia:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- wyrażenie: Wyrażenie wariantowe ma zostać obliczony.
- wartość: wartość zwracaną, jeśli odpowiednie wyrażenie ma wartość PRAWDA.

**Uwagi:**  
Na liście argumentów funkcji Przełącz składa się z par wyrażeń i wartości. Wyrażenia są przetwarzane od lewej do prawej i zwracana jest wartość skojarzone z pierwszym wyrażeniem, które dają wynik PRAWDA. Jeśli elementy nie są poprawnie sparowany, wystąpi błąd wykonania.

Na przykład jeśli Wyr1 ma wartość PRAWDA, Przełącz zwraca wartość1. Jeśli wyrażenie-1 ma wartość FAŁSZ, ale 2 wyrażenie ma wartość PRAWDA, Przełącz zwraca wartość 2 i tak dalej.

Przełączanie zwraca żaden element nie jeśli:

- Wyrażenia są prawdziwe.
- Pierwsze wyrażenie PRAWDA ma odpowiedniej wartości, która ma wartość Null.

Przełączanie oblicza wszystkie wyrażenia, mimo że zwracany jest tylko jeden z nich. Z tego powodu należy wyszukiwać niepożądane po stronie. Na przykład jeśli obliczenie dowolnego wyrażenia w wyniku dzielenie przez zero, wystąpi błąd.

Wartości można także wartość funkcji błędu, co powoduje zwrócenie niestandardowy ciąg.

**Przykład:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Zwraca język używany w niektórych większych miastach, w przeciwnym razie zwraca błąd.

----------
### <a name="trim"></a>Przycinanie

**Opis:**  
Funkcja Usuń.zbędne.odstępy usuwa pierwszą spację i końcowe spacji z ciągu.

**Składnia:**  
`str Trim(str value)`  

**Przykład:**  
`Trim(" Test ")`  
Zwraca wartość "Test".

`Trim([proxyAddresses])`  
Usuwa pierwszą spację i końcowe spacje dla każdej wartości atrybutu proxyAddress.

----------
### <a name="ucase"></a>UCase

**Opis:**  
Funkcja UCase konwertuje wszystkie znaki w ciągu na wielkie litery.

**Składnia:**  
`str UCase(str string)`

**Przykład:**  
`UCase("TeSt")`  
Zwraca ciąg "TEST".

----------
### <a name="word"></a>Program Word

**Opis:**  
Program Word funkcja wyraz zawartych w ciągu, na podstawie parametrów opisem ograniczniki i liczbę programu word, aby zwrócić.

**Składnia:**  
`str Word(str string, num WordNumber, str delimiters)`

- ciąg: ciąg zwraca słowa.
- WordNumber: numer, w których program word numer identyfikacyjny powinna zwrócić.
- ograniczniki: ciąg odpowiadający delimiter(s), które powinny być używane do identyfikowania wyrazów

**Uwagi:**  
Każdy ciąg znaków w ciągu, oddzielając je spośród znaków w ograniczniki są uznawane za wyrazy:

- Jeśli liczba < 1, zwraca pusty ciąg.
- Jeśli ciąg ma wartość null, zwraca ciąg pusty.

Jeśli ciąg zawiera mniej niż liczba wyrazów lub ciąg nie zawiera wszystkie wyrazy oznaczona ograniczniki, zwracany jest pusty ciąg.

**Przykład:**  
`Word("The quick brown fox",3," ")`  
Zwraca wartość "Kowalski"

`Word("This,string!has&many separators",3,",!&#")`  
Zwracana jest wartość "zawiera"

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Opis deklaracyjnych wyrażeń obsługi administracyjnej](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD Sync połączenia: Opcje dostosowywania synchronizacji](active-directory-aadconnectsync-whatis.md)
* [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](active-directory-aadconnect.md)
