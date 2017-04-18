<properties 
   pageTitle="Tworzenie certyfikatów z podpisem własnym dla punktu do witryny wirtualnej sieci między lokalnego połączenia przy użyciu makecert | Microsoft Azure"
   description="W tym artykule opisano kroki umożliwiające tworzenie certyfikatów z podpisem własnym w systemie Windows 10 przy użyciu makecert."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# <a name="working-with-self-signed-certificates-for-point-to-site-connections"></a>Praca z certyfikatów z podpisem własnym połączeń punktu do witryny

Ten artykuł ułatwia tworzenie certyfikatu z podpisem własnym za pomocą **makecert**, a następnie wygeneruj certyfikaty klienta z niego. Czynności są zapisywane na makecert w systemie Windows 10. Tworzenie certyfikatów, które są zgodne z połączeniami P2S został zweryfikowany MakeCert. 

W przypadku połączeń P2S preferowaną metodą certyfikaty jest użycie rozwiązania certyfikatu przedsiębiorstwa, pamiętając wydać certyfikaty klienta popularnym formatem wartość Nazwa 'name@yourdomain.com', zamiast format "Nazwa domeny\nazwa NetBIOS".

Jeśli nie masz rozwiązanie enterprise certyfikat z podpisem własnym jest to konieczne umożliwić klientom P2S nawiązać połączenia wirtualnej sieci. Pracujemy pamiętać, że została zastąpiona makecert, ale nadal jest prawidłową metodę tworzenia certyfikatów z podpisem własnym, które są zgodne z połączeniami P2S. Pracujemy nad innego rozwiązania dotyczące tworzenia certyfikatów z podpisem własnym, ale w tej chwili makecert jest preferowana metoda.

## <a name="create-a-self-signed-certificate"></a>Tworzenie certyfikatu z podpisem własnym

MakeCert jest jednym ze sposobów Tworzenie certyfikatu z podpisem własnym. Poniższe kroki szczegółową Tworzenie certyfikatu z podpisem własnym za pomocą makecert. Te kroki są określonych model wdrożenia. Są one ważne dla Menedżera zasobów i klasyczny.

### <a name="to-create-a-self-signed-certificate"></a>Aby utworzyć certyfikat z podpisem własnym

1. Na komputerze z systemem Windows 10 Pobierz i zainstaluj [System Windows Software Development Kit (SDK) dla systemu Windows 10](https://dev.windows.com/en-us/downloads/windows-10-sdk).

2. Po zakończeniu instalacji, możesz znaleźć narzędzie makecert.exe w obszarze następującą ścieżkę: C:\Program Files (x86) \Windows Kits\10\bin\<arch >. 
        
    Przykład:`C:\Program Files (x86)\Windows Kits\10\bin\x64`

3. Następnie utwórz i zainstalować certyfikat w magazynie certyfikatów osobistych na komputerze. Poniższy przykład tworzy odpowiedni plik *cer* , który należy przekazać Azure podczas konfigurowania P2S. Uruchom następujące polecenie jako administrator. Zamień *ARMP2SRootCert* i *ARMP2SRootCert.cer* nazwę, która ma być używany dla certyfikatu.<br><br>Certyfikat zostanie umieszczony w swojej Certyfikaty — bieżący User\Personal\Certificates.

        makecert -sky exchange -r -n "CN=ARMP2SRootCert" -pe -a sha1 -len 2048 -ss My "ARMP2SRootCert.cer"


###  <a name="rootpublickey"></a>Aby uzyskać klucz publiczny

W ramach konfiguracji bramy sieci VPN dla połączenia punkt do witryny klucz publiczny certyfikatu głównego przekazaniu Azure.

1. Aby uzyskać plik cer z certyfikatu, otwórz **certmgr.msc**. Kliknij prawym przyciskiem myszy certyfikat główny podpisem, kliknij pozycję **Wszystkie zadania**i kliknij przycisk **Eksportuj**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.

2. W kreatorze kliknij przycisk **Dalej**, zaznacz **nie Eksportuj klucz prywatny**, a następnie kliknij przycisk **Dalej**.

3. Na stronie **Format pliku eksportu** wybierz **Base-64 zakodowany X.509 (. CER).** Następnie kliknij przycisk **Dalej**. 

4. W **pliku do wyeksportowania**, **Przejdź** do lokalizacji, do której chcesz wyeksportować certyfikat. W polu **Nazwa pliku**nadaj nazwę pliku certyfikatu. Następnie kliknij przycisk **Dalej**.

5. Kliknij przycisk **Zakończ** , aby wyeksportować certyfikat.

 
### <a name="export-the-self-signed-certificate-optional"></a>Eksportowanie certyfikatu z podpisem własnym (opcjonalnie)

Być może zechcesz eksportowanie certyfikatu z podpisem własnym i zapisanie w bezpiecznym. Jeśli potrzeba, później zainstalować go na innym komputerze i wygenerować więcej certyfikatów klienta lub inny plik cer eksportowanie. Dowolnym komputerze ze zainstalowany certyfikat klienta i skonfigurowano również pisane z wielkiej litery klienta VPN, ustawienia można nawiązać połączenie z siecią wirtualną za pośrednictwem P2S. Z tego powodu chcesz upewnij się, że certyfikaty klienta są generowane i zainstalowany tylko w razie potrzeby i że certyfikatu z podpisem własnym są przechowywane bezpiecznie.

Aby wyeksportować certyfikat z podpisem własnym jako PFX, wybierz certyfikat główny, a następnie Zastosuj te same kroki opisane w [Eksportowanie certyfikatu klienta](#clientkey) do wyeksportowania.

## <a name="create-and-install-client-certificates"></a>Tworzenie i zainstalować certyfikaty klienta

Nie instaluj certyfikatu z podpisem własnym bezpośrednio na komputerze klienckim. Musisz Generowanie certyfikat klienta na podstawie certyfikatu z podpisem własnym. Następnie wyeksportować i zainstalować certyfikat klienta na komputerze klienckim. Poniższe kroki nie są określonych model wdrożenia. Są one ważne dla Menedżera zasobów i klasyczny.

### <a name="part-1---generate-a-client-certificate-from-a-self-signed-certificate"></a>Część 1 — Generowanie certyfikat klienta na podstawie certyfikatu z podpisem własnym

Poniższe kroki szczegółową jednym ze sposobów Generowanie certyfikat klienta na podstawie certyfikatu z podpisem własnym. Z tego samego certyfikatu, może spowodować wiele certyfikatów klienta. Każdy certyfikat klienta można następnie eksportowane i zainstalowany na komputerze klienckim. 

1. Na tym samym komputerze, który umożliwia tworzenie certyfikatu z podpisem własnym Otwórz wiersz polecenia jako administrator.

2. W tym przykładzie "ARMP2SRootCert" odwołuje się do certyfikatu z podpisem własnym, który został wygenerowany. 
    - Zmień *"ARMP2SRootCert"* na nazwę jest generowany certyfikat klienta z podpisem własnym katalogu głównego. 
    - Zmienić nazwę, której chcesz wygenerować certyfikat klienta za *ClientCertificateName* . 


    Modyfikowanie i uruchom próbki, aby wygenerować certyfikat klienta. Po uruchomieniu poniższy przykład bez modyfikowania go, wynikiem jest certyfikat klienta o nazwie ClientCertificateName w magazynie certyfikatów osobistych, wygenerowanym z certyfikatu głównego ARMP2SRootCert.

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "ARMP2SRootCert" -is my -a sha1

4. Wszystkie certyfikaty są przechowywane w sieci "Certyfikaty — bieżący User\Personal\Certificates" przechowywane na komputerze. Istnieje możliwość wygenerowania jako wiele certyfikatów w razie potrzeby według tej procedury.

### <a name="clientkey"></a>Część 2 - wyeksportuj certyfikat klienta

1. Aby wyeksportować certyfikat klienta, otwórz **certmgr.msc**. Kliknij prawym przyciskiem myszy certyfikat klienta, który chcesz eksportu, kliknij polecenie **Wszystkie zadania**, a następnie kliknij polecenie **Eksportuj**. Spowoduje to otwarcie **Kreatora eksportu certyfikatów**.

2. W kreatorze kliknij przycisk **Dalej**, a następnie wybierz pozycję **Tak, Eksportuj klucz prywatny**, a następnie kliknij przycisk **Dalej**.

3. Na stronie **Format pliku eksportu** można pozostawić domyślne zaznaczone. Następnie kliknij przycisk **Dalej**. 
 
4. Na **stronie** możesz chronić klucz prywatny. Jeśli wybierzesz opcję Użyj hasła, upewnij się, że rekord lub Zapamiętaj hasło dla tego certyfikatu. Następnie kliknij przycisk **Dalej**.

5. W **pliku do wyeksportowania**, **Przejdź** do lokalizacji, do której chcesz wyeksportować certyfikat. W polu **Nazwa pliku**nadaj nazwę pliku certyfikatu. Następnie kliknij przycisk **Dalej**.

6. Kliknij przycisk **Zakończ** , aby wyeksportować certyfikat.  

### <a name="part-3---install-a-client-certificate"></a>Część 3 — Zainstaluj certyfikat klienta

Każdego klienta, którego chcesz utworzyć połączenie z siecią wirtualną przy użyciu połączenia punkt do witryny musi być zainstalowany certyfikat klienta. Ten certyfikat jest oprócz wymagany pakiet konfiguracji sieci VPN. Poniższe kroki szczegółową ręczne instalowanie certyfikatu klienta.

1. Znajdź i skopiuj plik *pfx* na komputer kliencki. Na komputerze klienckim kliknij dwukrotnie plik *pfx* do zainstalowania. Pozostaw **Lokalizacji przechowywania** jako **Bieżącego użytkownika**, a następnie kliknij przycisk **Dalej**.

2. **Plik** do zaimportowania strony nie wprowadzono żadnych zmian. Kliknij przycisk **Dalej**.

3. Na stronie **ochrony klucza prywatnego** wprowadzania hasło dla certyfikatu, jeśli użyto, i sprawdź, czy podmiot zabezpieczeń instaluje certyfikat jest poprawny, a następnie kliknij przycisk **Dalej**.

4. Na stronie **Magazyn certyfikatów** pozostaw domyślnej lokalizacji, a następnie kliknij przycisk **Dalej**.

5. Kliknij przycisk **Zakończ**. **Ostrzeżenie o zabezpieczeniach** dla instalacji certyfikatu kliknij przycisk **Tak**. Certyfikat jest pomyślnie zaimportowane.

## <a name="next-steps"></a>Następne kroki

Przejdź do swojej konfiguracji punktu do witryny. 

- **Menedżer zasobów** wdrożenia modelu instrukcje zobacz [Konfigurowanie połączenia punkt do witryny VNet przy użyciu programu PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md). 
- Aby **Klasyczny** instrukcje modelu wdrażania zobacz [połączenie VPN Konfiguruj punkt do witryny VNet za pomocą portalu klasyczny](vpn-gateway-point-to-site-create.md).