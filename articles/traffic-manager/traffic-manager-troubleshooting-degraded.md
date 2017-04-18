<properties
    pageTitle="Rozwiązywanie problemów z obniżonej wydajności stanu w Menedżerze ruch Azure"
    description="Rozwiązywanie problemów z profilami Menedżera ruch jest wyświetlany jako obniżonej wydajności stanu."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Rozwiązywanie problemów z obniżonej wydajności stanu w Menedżerze ruch Azure

W tym artykule opisano, jak rozwiązywać problemy z profilu Menedżera ruch Azure, który jest wyświetlany stan ograniczone. W tym scenariuszu należy rozważyć, czy skonfigurowano profilu Menedżer ruchu wskazującą do niektórych usług cloudapp.net hostowana. Podczas sprawdzania kondycji Menedżera ruch pojawi się obniża się stanu.

![Stan ograniczone](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degraded.png)

Jeśli przejdziesz do karta punkty końcowe tego profilu, zobacz jednej lub większej liczby punkty końcowe ze stanem Offline:

![trybu offline](./media/traffic-manager-troubleshooting-degraded/traffic-manager-offline.png)

## <a name="understanding-traffic-manager-probes"></a>Opis ruch Menedżera sondy

- Menedżer ruchu uważa punktu końcowego się tylko wtedy, gdy sondy odbiera odpowiedź HTTP 200 z ścieżkę sondy ONLINE. Inne odpowiedzi-200 jest błąd.
- Przekieruj 30 x nie powiedzie się, nawet jeśli adresu URL zwraca 200.
- Dla sondy HTTPs błędy certyfikatów są ignorowane.
- Rzeczywistej zawartości ścieżkę sondy nie ma znaczenia, ile jest zwracany 200. Badanie adresu URL niektórych zawartość statyczną, takie jak "/ favicon.ico" jest techniką typowych. Zawartość dynamiczna, podobnie jak strony ASP, nie może być zawsze zwraca 200, nawet wtedy, gdy aplikacja nie jest uszkodzony.
- Dobrym rozwiązaniem jest równa ścieżkę sondy coś, co ma za mało logiczny, aby określić, że witryna jest w górę lub w dół. W poprzednim przykładzie, ustawiając ścieżkę "/ favicon.ico", są tylko badania tego w3wp.exe odpowiada. Sonda nie może oznaczać, że aplikacji sieci web jest uszkodzony. Lepszym rozwiązaniem jest ustawienie ścieżki elementy takie jak "/ Probe.aspx", który zawiera logikę do określenia kondycji witryny usługi. Na przykład możesz użyć liczników wydajności do procesora lub zmierzyć liczba zakończonych niepowodzeniem żądań. Lub może próbować uzyskać dostęp do bazy danych zasobów lub stan sesji, aby upewnić się, że działa aplikacji sieci web.
- Jeśli wszystkie punkty końcowe w profilu są ograniczone, następnie Menedżer ruchu potraktuje wszystkie punkty końcowe jako prawidłowy i ruch trasy do wszystkich punktów końcowych. To zachowanie gwarantuje, że problemy z mechanizmu sondowania spowoduje awaria wykonania tej usługi.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Rozwiązywanie problemów z awarii sondy, wymaga narzędzia, które zawiera kod stanu HTTP zwrotu z adresu URL sondy. Istnieje wiele narzędzi pokazujące nieprzetworzonych odpowiedź HTTP.

* [Fiddler](http://www.telerik.com/fiddler)
* [Zwinięcie](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Ponadto służy karta Sieć narzędzia debugowania F12 w programie Internet Explorer wyświetlanie odpowiedzi HTTP.

W tym przykładzie chcemy zobacz odpowiedź URL sondy: http://watestsdp2008r2.cloudapp.net:80-sondy. W poniższym przykładzie programu PowerShell przedstawia ten problem.

```powershell
    Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Przykład danych wyjściowych:

```text
    StatusCode StatusDescription
    ---------- -----------------
            301 Moved Permanently
```

Zwróć uwagę, że Otrzymaliśmy odpowiedź przekierowywania. Jak wspomniano wcześniej, wszelkie StatusCode innych niż 200 jest traktowany jako błąd. Menedżer ruchu zmienia stan końcowy na tryb Offline. Aby rozwiązać ten problem, sprawdź w konfiguracji witryny sieci Web, aby upewnić się, że pisane z wielkiej litery StatusCode mogą zostać zwrócone z ścieżkę sondy. Zmień konfigurację sondy Menedżer ruchu, aby wskazywały na ścieżkę, która zwraca wartość 200.

Sonda usługi jest przy użyciu protokołu HTTPS, może być konieczne wyłączanie sprawdzania w celu uniknięcia błędów SSL/TLS podczas do badania certyfikatu. Poniższe instrukcje programu PowerShell, aby wyłączyć sprawdzanie poprawności certyfikatu dla bieżącej sesji programu PowerShell:

```powershell
    add-type @"
    using System.Net;
    using System.Security.Cryptography.X509Certificates;
    public class TrustAllCertsPolicy : ICertificatePolicy {
        public bool CheckValidationResult(
        ServicePoint srvPoint, X509Certificate certificate,
        WebRequest request, int certificateProblem) {
        return true;
        }
    }
    "@
    [System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Następne kroki

[Informacje o metody routingu ruchu Menedżer ruchu](traffic-manager-routing-methods.md)

[Co to jest Menedżer ruchu](traffic-manager-overview.md)

[Usług w chmurze](http://go.microsoft.com/fwlink/?LinkId=314074)

[Aplikacje Azure Web](https://azure.microsoft.com/documentation/services/app-service/web/)

[Operacje na Menedżer ruchu (REST interfejsu API informacje)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure poleceń cmdlet Menedżera ruchu][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
