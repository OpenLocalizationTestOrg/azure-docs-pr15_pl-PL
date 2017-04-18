<properties 
    pageTitle="Dodawanie certyfikatu do magazynu urzędu certyfikacji Java | Microsoft Azure" 
    description="Dowiedz się, jak dodać certyfikat urząd certyfikacji w magazynie certyfikatów (cacerts) urzędu certyfikacji Java usługi Twilio lub Bus usługi Azure." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Dodawanie certyfikatu w magazynie certyfikatów urzędu certyfikacji Java
Poniższe kroki pokazują jak dodać certyfikat urząd certyfikacji w magazynie certyfikatów (cacerts) Java urząd certyfikacji. Przykład używany jest wymagane przez usługę Twilio certyfikatu urząd certyfikacji. Informacji podanych dalej w tym temacie opisano, jak zainstalować certyfikat urzędu certyfikacji dla Bus usługi Azure. 

Narzędzie klucza umożliwia dodawanie certyfikatu urzędu certyfikacji przed pakowanie usługi JDK i dodanie go do folderu **approot** Azure programu project lub można uruchomić zadania programu Azure uruchamiania, którego użyto narzędzie klucza w celu dodania certyfikatu. W tym przykładzie założono, że zostaną dodane certyfikatem urzędu certyfikacji przed JDK jest zip. Ponadto określony certyfikat urzędu certyfikacji zostanie użyte w tym przykładzie, ale kroki uzyskiwania różnych certyfikatem urzędu certyfikacji, a następnie importując go w magazynie cacerts będzie podobna.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Dodawanie certyfikatu w magazynie cacerts

1. W wierszu polecenia jest ustawiona na folder **jdk\jre\lib\security** usługi JDK uruchom następujące czynności, aby sprawdzić, jakie certyfikaty są zainstalowane:

    `keytool -list -keystore cacerts`

    Zostanie wyświetlony monit o podanie hasła magazynu. Hasło domyślne to **changeit**. (Jeśli chcesz zmienić hasło, zobacz dokumentację narzędzie klucza w <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>). W tym przykładzie zakłada się, że certyfikat z MD5 linii papilarnych 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 nie ma na liście, a chcesz zaimportować (ten certyfikat określonego są wymagane przez usługę Twilio interfejsu API).
2. Uzyskaj certyfikat z listy certyfikaty wymienione na [Certyfikatów głównych GeoTrust](http://www.geotrust.com/resources/root-certificates/). Kliknij prawym przyciskiem myszy łącze certyfikat z 35:DE:F4:CF numer seryjny i zapisać go w folderze **jdk\jre\lib\security** . W celu w tym przykładzie, został zapisany w pliku o nazwie **firmy Equifax\_bezpiecznego\_certyfikat\_Authority.cer**.
3. Importowanie certyfikatu przez następujące polecenie:

    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`

    Po wyświetleniu monitu o ten certyfikat, jeśli certyfikat ma MD5 odcisku palca 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, odpowiedź, wpisując **y**.
4. Uruchom następujące polecenie, aby upewnić się, że certyfikat urzędu certyfikacji został pomyślnie zaimportowany:

    `keytool -list -keystore cacerts`

5. ZIP JDK i dodaj go do folderu **approot** Azure programu project.

Aby uzyskać informacje na temat narzędzie klucza zobacz <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Certyfikatów głównych Azure

Aplikacje korzystające z usług Azure (na przykład Bus usługi Azure) muszą zaufania dla certyfikatu Baltimore CyberTrust Root. (Począwszy od 15 kwietnia 2013 Azure rozpoczęcia migracji z GTE CyberTrust Root globalnej do Baltimore CyberTrust Root. Tej migracji zajęła kilka miesięcy do wykonania).

Baltimore certyfikat może być już zainstalowany w sklepie cacerts więc pamiętać o uruchamianiu **Narzędzie klucza — lista** polecenie, aby dowiedzieć się, jeśli już istnieje.

Jeśli chcesz dodać Baltimore CyberTrust Root ma numer seryjny 02:00:00:b9 i c d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 odcisku palca SHA1: 78:db:28:52:ca:e4:74. Czy można pobrać z <https://cacert.omniroot.com/bc2025.crt>, zapisane lokalny plik z rozszerzeniem **cer**, a następnie zaimportowane przy użyciu **Narzędzie klucza** , jak pokazano powyżej.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat certyfikatów głównych używane przez Azure zobacz [Migracji certyfikatów głównych Azure](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Aby uzyskać więcej informacji na temat języka Java zobacz [Centrum deweloperów języka Java](/develop/java/).
