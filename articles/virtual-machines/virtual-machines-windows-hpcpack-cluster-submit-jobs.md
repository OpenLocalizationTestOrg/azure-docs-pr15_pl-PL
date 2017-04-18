<properties
 pageTitle="Przesyłanie zadań HPC Pack klaster w Azure | Microsoft Azure"
 description="Dowiedz się, jak skonfigurować na komputerze lokalnym, aby przesłać zadania do klastrze HPC Pack platformy Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>Wysyłanie zadań HPC z komputera lokalnego do klastrów HPC Pack wdrożony platformy Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Skonfiguruj komputer kliencki lokalnego przesyłać zadania do klastrów [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) platformy Azure. W tym artykule pokazano, jak skonfigurować komputera lokalnego narzędziami klienta, aby przesłać zadanie za pomocą protokołu HTTPS do klastrów platformy Azure. W ten sposób kilku użytkowników klaster mogli przesyłać zadania do klastrów HPC Pack opartej na chmurze, ale bez połączenia bezpośrednio do węzła głównego maszyn wirtualnych i uzyskiwanie dostępu do subskrypcji usługi Azure.

![Prześlij zadanie do klastrów platformy Azure][jobsubmit]

## <a name="prerequisites"></a>Wymagania wstępne

* **Węzła głównego HPC Pack wdrożony w maszyn wirtualnych Azure** - zaleca się używanie automatyczną narzędzi, takich jak [szablon Azure Szybki Start](https://azure.microsoft.com/documentation/templates/) lub [skrypt programu PowerShell Azure](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) wdrożyć węzła głównego i klaster. Potrzebujesz nazwę DNS węzła głównego i poświadczenia administratora klastrów do wykonania kroków w tym artykule.

* **Komputer kliencki** - potrzebujesz Windows lub Windows Server komputer kliencki, który może zostać uruchomiony HPC Pack narzędzi klienta (zobacz [wymagania systemowe](https://technet.microsoft.com/library/dn535781.aspx)). Jeśli chcesz przesyłać zadania za pomocą portalu HPC Pack lub interfejsu API usługi REST, używając dowolnego komputera klienckiego w dowolnym miejscu.

* **Nośnik instalacyjny HPC Pack** — do zainstalowania narzędzia klienta HPC Pack pakietu instalacyjnego bezpłatne najnowszą wersję pakietu HPC (HPC Pack 2012 R2) jest dostępny w [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=328024). Upewnij się, aby pobrać tę samą wersję programu HPC Pack zainstalowanego na węzła głównego maszyn wirtualnych.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Krok 1: Instalowanie i konfigurowanie składników sieci web na węzła głównego

Aby włączyć interfejsu REST przesyłać zadania z klastrem za pomocą protokołu HTTPS, upewnij się, że składniki web HPC Pack są skonfigurowane w węźle głowy HPC Pack. Jeśli jeszcze nie są zainstalowane, należy najpierw zainstalować składników sieci web, uruchamiając plik instalacji HpcWebComponents.msi. Następnie należy skonfigurować składniki, uruchamiając skrypt programu HPC PowerShell **HPCWebComponents.ps1 zestawu**.

Aby uzyskać szczegółowe procedury zobacz [Instalowanie składników sieci Web pakietu Microsoft HPC Pack](http://technet.microsoft.com/library/hh314627.aspx).

>[AZURE.TIP] Niektóre szablony Azure Szybki Start dla HPC Pack zainstalować i skonfigurować składnik web automatycznie. Jeśli [skrypt wdrożenia HPC Pack IaaS](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) umożliwia tworzenie klaster, można opcjonalnie zainstalowaniu i skonfigurowaniu składników sieci web w ramach wdrożenia.

**Aby zainstalować składnik sieci web**

1. Połącz się węzła głównego maszyn wirtualnych przy użyciu poświadczeń administratora klastrów.

2. W folderze HPC pakiet Instalatora uruchom HpcWebComponents.msi na węzła głównego.

3. Postępuj zgodnie z instrukcjami kreatora, aby zainstalować składnik sieci web

**Aby skonfigurować składniki sieci web**

1. Uruchom jako administrator programu PowerShell HPC na węzła głównego.

2. Aby zmienić katalog do lokalizacji skryptu konfiguracji, wpisz następujące polecenie:

    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. Aby skonfigurować interfejsu REST i uruchomić usługę sieci Web HPC, wpisz następujące polecenie:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. Gdy zostanie wyświetlony monit o wybranie certyfikatu, wybierz certyfikat, który odpowiada publiczna nazwa DNS węzła głównego. Na przykład po wdrożeniu węzła głównego maszyn wirtualnych przy użyciu modelu Klasyczny wdrożenia, nazwa certyfikatu wygląda CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Jeśli używasz model wdrożenia Menedżera zasobów, nazwa certyfikatu wygląda CN =&lt;*HeadNodeDnsName*&gt;. &lt; *regionu*&gt;. cloudapp.azure.com.

    >[AZURE.NOTE] Możesz wybrać ten certyfikat później podczas przesyłania zadań do węzła głównego z komputera lokalnego. Brak zaznaczenia lub skonfigurować certyfikat, który odpowiada nazwie komputera węzła głównego w domenie usługi Active Directory (na przykład CN =*MyHPCHeadNode.HpcAzure.local*).

5. Aby skonfigurować portalu składania zadania, wpisz następujące polecenie:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Po ukończeniu działania skryptu, Zatrzymaj i ponownie uruchomić usługę Harmonogram zadań HPC, wpisując następujące polecenia:

    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Krok 2: Instalowanie narzędzi klienta HPC Pack na komputerze lokalnym

Jeśli chcesz zainstalować pakiet HPC narzędzi klienta na komputerze, należy pobrać pliki instalacji HPC Pack (pełnej instalacji) z [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=328024). Po rozpoczęciu instalacji, wybierz opcję ustawienia **narzędzi klienta HPC Pack**.

Aby użyć narzędzia klienckie HPC Pack Aby przesłać zadania do węzła głównego maszyn wirtualnych, należy wyeksportować certyfikat z węzła głównego i zainstaluj go na komputerze klienckim. Certyfikat musi znajdować się w. CER format.

**Aby wyeksportować certyfikat z węzła głównego**

1. Na węzła głównego Dodawanie przystawki Certyfikaty do programu Microsoft Management Console dla konta na komputerze lokalnym. Aby uzyskać procedury Dodawanie przystawki zobacz [Dodawanie przystawki Certyfikaty do konsoli MMC](https://technet.microsoft.com/library/cc754431.aspx).

2. W drzewie konsoli rozwiń **Certyfikaty — komputer lokalny** > **osobistych**, a następnie kliknij pozycję **Certyfikaty**.

3. Znajdź certyfikat, który skonfigurowano HPC Pack składników sieci web w [Krok 1: Instalowanie i konfigurowanie składników sieci web na węzła głównego](#step-1:-install-and-configure-the-web-components-on-the-head-node) (na przykład CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net).

4. Kliknij prawym przyciskiem myszy certyfikat, a następnie kliknij pozycję **Wszystkie zadania** > **Eksportowanie**.

5. W Kreatorze eksportu certyfikatów kliknij przycisk **Dalej**i upewnij się, że jest zaznaczona opcja **nie Eksportuj klucz prywatny** .

6. Wykonaj pozostałe kroki kreatora, aby wyeksportować certyfikat w X.509 binarny DER zakodowany (. Format CER).


**Aby zaimportować certyfikat na komputerze klienckim**


1. Kopiowanie certyfikat, który wyeksportowanego z węzła głównego w folderze na komputerze klienckim.

2. Na komputerze klienckim uruchom certmgr.msc.

3. W Menedżerze certyfikatów rozwiń **Certyfikaty — bieżący użytkownik** > **Zaufane główne urzędy certyfikacji**, kliknij prawym przyciskiem myszy **Certyfikaty**, a następnie kliknij **Wszystkie zadania** > **importu**.

4. W Kreatorze importu certyfikatów kliknij przycisk **Dalej** , a następnie postępuj zgodnie z instrukcjami, aby zaimportować certyfikat, który wyeksportowanego z węzła głównego w magazynie Zaufane główne urzędy certyfikacji.



>[AZURE.TIP] Ostrzeżenie o zabezpieczeniach, może zostać wyświetlony, ponieważ urząd certyfikacji na węzła głównego nie został rozpoznany przez komputer kliencki. Podczas testowania, można zignorować to ostrzeżenie i ukończyć Importowanie certyfikatu.

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Krok 3: Uruchom test zadania w klastrze

Aby sprawdzić konfigurację, spróbuj uruchomić zadań w klastrze platformy Azure z komputera lokalnego. Na przykład można użyć narzędzi graficznego interfejsu użytkownika HPC Pack lub polecenia przesyłać zadania z klastrem. Aby przesłać zadania umożliwia także aplikacja portal oparta na sieci web.


**Aby uruchomić polecenia przesyłania zadania na komputerze klienckim**


1. Na komputerze klienckim, gdzie są zainstalowane narzędzia klienta HPC Pack Uruchom wiersz polecenia.

2. Wpisz polecenie przykładowe. Na przykład aby wyświetlić wszystkie zadania w klastrze, wpisz polecenie podobny do jednego z następujących opcji, w zależności od pełną nazwę DNS węzła głównego:

    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
    
    lub
    
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```

    >[AZURE.TIP] Użyj pełną nazwę DNS węzła głównego nie adresu IP w adresie URL harmonogram. Jeśli użytkownik określi adres IP, komunikat o błędzie jest podobny do "certyfikat serwera potrzebuje albo prawidłowy łańcuch zaufania lub mają być umieszczone w magazynie Zaufane główne".

3. Po wyświetleniu monitu wpisz nazwę użytkownika (w postaci &lt;nazwa_domeny&gt;\\&lt;nazwa_użytkownika&gt;) i hasło administratora klastrów HPC lub innego użytkownika klaster, który skonfigurowano. Istnieje możliwość przechowywanie poświadczeń lokalnie w celu więcej operacji zadań.

    Zostanie wyświetlona lista zadań.


**Za pomocą Menedżera zadań HPC na komputerze klienckim**

1. Jeśli nie został wcześniej przechowywanie poświadczeń domeny użytkownika, klaster podczas przesyłania zadanie, możesz dodać poświadczenia w Menedżerze poświadczeń.

    . W Panelu sterowania na komputerze klienckim zacznij Menedżer poświadczeń.

    b. Kliknij pozycję **Poświadczeń systemu Windows** > **Dodaj ogólnego poświadczeń**.

    c. Określ adres internetowy (na przykład https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler lub https://&lt;HeadNodeDnsName&gt;.&lt; region&gt;.cloudapp.azure.com/HpcScheduler) i nazwa użytkownika (&lt;nazwa_domeny&gt;\\&lt;nazwa_użytkownika&gt;) i hasło administratora klastrów lub innego użytkownika klaster, który skonfigurowano.

2. Na komputerze klienckim uruchom Menedżera zadań HPC.

3. W oknie dialogowym **Wybierz węzeł głowy** , wpisz adres URL węzła głównego w Azure (na przykład https://&lt;HeadNodeDnsName&gt;. cloudapp.net lub https://&lt;HeadNodeDnsName&gt;.&lt; region&gt;. cloudapp.azure.com).

    Menedżer zadań HPC zostanie otwarty i jest wyświetlana lista zadań na węzła głównego.

**Aby korzystać z portalu sieci web uruchomionych węzła głównego**

1. Uruchom przeglądarkę sieci web na komputerze klienckim i wprowadź jedną z następujących adresów, w zależności od pełną nazwę DNS węzła głównego:

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
    
    lub
    
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. W oknie dialogowym zabezpieczeń wpisz poświadczenia administratora klastrów HPC domeny. (Możesz również dodać innym użytkownikom klaster w różne role. Zobacz [Zarządzanie użytkownikami klaster](https://technet.microsoft.com/library/ff919335.aspx)).

    W portalu sieci web po otwarciu widoku listy zadań.

3. Aby przesłać zadanie próbki, która zwraca ciąg "Witaj świecie" z klastrem, kliknij przycisk **Nowe zadanie** w lewym menu.

4. Na stronie **Nowe zadanie** w obszarze **ze stron przesyłania**, kliknij **HelloWorld**. Zostanie wyświetlona strona przesyłania zadania.

5. Kliknij przycisk **Prześlij**. Jeśli zostanie wyświetlony monit, należy podać poświadczenia domeny administratora klastrów HPC. Zadanie zostaje przesłane, a identyfikator zadania jest wyświetlany na stronie **Moje zadania** .

6. Aby wyświetlić wyniki zadania, które przesłane, kliknij identyfikator zadania, a następnie kliknij **Wyświetlanie zadań** w celu wyświetlania danych wyjściowych poleceń (w obszarze **dane wyjściowe**).

## <a name="next-steps"></a>Następne kroki

* Możesz również przesłać zadania do klastrów Azure z [Dodatkiem Service Pack HPC interfejsu API usługi REST](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).

* Jeśli chcesz przesyłać zadania klaster w kliencie Linux, zobacz przykładowe Python [HPC Pack 2012 R2 SDK oraz przykładowy kod](https://www.microsoft.com/download/details.aspx?id=41633).


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
