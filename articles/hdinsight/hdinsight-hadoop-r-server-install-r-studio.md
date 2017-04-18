<properties
    pageTitle="Instalowanie RStudio z serwerem R na HDInsight (wersja preview) | Microsoft Azure"
    description="Jak zainstalować RStudio z serwerem R na HDInsight (wersja preview)."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/16/2016"
   ms.author="jeffstok"/>


# <a name="installing-rstudio-with-r-server-on-hdinsight-preview"></a>Instalowanie RStudio z serwerem R na HDInsight (wersja preview)

Istnieje wiele zintegrowanych środowiskach programistycznych (IDE) R dzisiaj, tym firmy Microsoft ostatnio ogłoszone [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS), rodzina narzędzia Pulpit i serwer z [RStudio](https://www.rstudio.com/products/rstudio-server/)lub Walware na podstawie Zaćmienie [StatET](http://www.walware.de/goto/statet). Między najpopularniejszych w systemie Linux jest użycie [Serwera RStudio](https://www.rstudio.com/products/rstudio-server/) , który zapewnia IDE zgodny z przeglądarką do użycia przez klientów zdalnych.  Instalowanie serwera RStudio węzeł krawędzi klaster HDInsight Premium zapewnia pełną obsługę IDE rozwoju i wykonywanie skryptów R R serwera w klastrze i może być znacznie wydajniej niż domyślne używanie konsoli R.

W tym artykule dowiesz się, jak zainstalować społeczności (bezpłatnie) wersji serwera RStudio węzeł krawędzi klastrze przy użyciu skryptu niestandardowego. Jeśli wolisz komercyjnego licencjonowana wersja Pro RStudio serwera, należy wykonać instrukcje dotyczące instalacji z [Serwera RStudio](https://www.rstudio.com/products/rstudio/download-server/).

> [AZURE.NOTE] Kroki opisane w tym dokumencie wymagają serwera R w klastrze HDInsight i nie będzie działać poprawnie, jeśli korzystasz z usługi HDInsight klaster w przypadku, gdy R został zainstalowany przy użyciu [Zainstalować Akcja skrypt R](hdinsight-hadoop-r-scripts-linux.md).

## <a name="prerequisites"></a>Wymagania wstępne

* Klaster Azure HDInsight z serwerem R zainstalowany. Aby uzyskać instrukcje zobacz [Rozpoczynanie pracy z serwerem R dotyczących klastrów HDInsight](hdinsight-hadoop-r-server-get-started.md).
* Klient SSH. Rozkład Linux i Unix lub Macintosh OS X `ssh` polecenie jest dostępne w systemie operacyjnym. Dla systemu Windows zaleca się [programów Cygwin](http://www.redhat.com/services/custom/cygwin/) za pomocą [opcji OpenSSH](https://www.youtube.com/watch?v=CwYSvvGaiWU)lub [Kit](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  


## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Instalowanie RStudio w klastrze za pomocą skryptu niestandardowego

1. Identyfikowanie węzeł krawędzi klaster. W przypadku klastrze HDInsight z serwerem R następujące jest konwencję nazewnictwa dla węzła głównego i węzeł krawędzi.

    * Węzeł głowy —`CLUSTERNAME-ssh.azurehdinsight.net`
    * Węzeł krawędź —`R-Server.CLUSTERNAME-ssh.azurehdinsight.net` 

2. SSH do węzła krawędzi klaster przy użyciu podanych wzorzec nazewnictwa. 
 
    * Jeśli łączysz się z klienta Linux, zobacz [Łączenie do klastrów HDInsight systemem Linux](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).
    * Jeśli łączysz się z klientem systemu Windows, zobacz [Łączenie z klaster systemem Linux HDInsight za pomocą Kit](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster).

3. Po nawiązaniu połączenia stają się użytkownik root w klastrze. W danej sesji SSH Użyj następującego polecenia.

        sudo su -

4. Pobierz skryptu niestandardowego w celu zainstalowania RStudio. Użyj następującego polecenia.

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Zmienianie uprawnień do pliku skryptu niestandardowego i uruchom skrypt. Za pomocą następujących poleceń.

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Jeżeli używasz hasła SSH podczas tworzenia klastrze HDInsight z serwerem R, możesz pominąć ten krok i przejść do następnego. Jeśli do utworzyć klaster został użyty klucz SSH zamiast tego, należy ustawić hasło dla użytkownika SSH. Konieczne będzie to hasło podczas łączenia się RStudio. Uruchom następujące polecenia. Po wyświetleniu monitu o **Kerberos bieżące hasło**, po prostu naciśnij klawisz **ENTER**.  Należy zauważyć, że należy zastąpić `USERNAME` użytkownikowi SSH dla klaster HDInsight.

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:
        
    Jeśli hasło pomyślnie jest ustawiona, powinien zostać wyświetlony następujący komunikat.

        passwd: password updated successfully


    Kończenie sesji SSH.

7. Tworzenie tunelem SSH z klastrem dzięki mapowaniu `localhost:8787` w klastrze HDInsight do komputera klienckiego. Musisz utworzyć tunelem SSH przed otwarciem nową sesję przeglądarki.

    * Na kliencie Linux lub klienta systemu Windows z [programów Cygwin](http://www.redhat.com/services/custom/cygwin/) następnie otwórz sesję końcowych i użyj następującego polecenia.

            ssh -L localhost:8787:localhost:8787 USERNAME@R-Server.CLUSTERNAME-ssh.azurehdinsight.net
            
        Zamień **nazwę użytkownika** dla klaster HDInsight użytkownika SSH i zamienić **NAZWAKLASTRA** o nazwie klaster HDInsight umożliwia także klucz SSH zamiast hasła, dodając`-i id_rsa_key`     

    * Jeśli przy użyciu klienta systemu Windows z Kit następnie

        1.  Otwórz Kit i wprowadź informacje o połączeniu. Jeśli nie znasz Kit, zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md) dla informacji na temat sposobu używania go z usługi HDInsight.
        2.  W sekcji **kategorii** po lewej stronie okna dialogowego rozwiń **połączenie**, rozwiń **SSH**, a następnie wybierz pozycję **tuneli**.
        3.  Wprowadź następujące informacje w formularzu **Opcje przekierowania portów SSH** :

            * **Port źródłowy** — port klienta, który chcesz przesłać dalej. Na przykład **8787**.
            * **Miejsce docelowe** - miejsce docelowe, które muszą zostać zamapowane na komputer lokalny klient. Na przykład **localhost:8787**.

            ![Tworzenie tunelem SSH] (./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Tworzenie tunelem SSH")

        4. Kliknij przycisk **Dodaj** , aby dodać ustawienia, a następnie kliknij przycisk **Otwórz** , aby otworzyć połączenie SSH.
        5. Gdy zostanie wyświetlony monit, zaloguj się na serwerze. Spowoduje to ustanowić sesję SSH i Włącz tunelem.

8. Otwórz przeglądarkę sieci web i wprowadź następujący adres URL, na podstawie wprowadzonych w tunelem portu.

        http://localhost:8787/ 

9. Wyświetli monit o wprowadź nazwę SSH użytkownika i hasło, aby połączyć się z klastrem. Jeśli został użyty klucz SSH podczas tworzenia klaster, należy wprowadzić hasło utworzone w kroku 5.

    ![Nawiązywanie połączenia z R Studio] (./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Tworzenie tunelem SSH")

10. Aby sprawdzić, czy instalacja RStudio zakończyła się pomyślnie, może zostać uruchomiony odpowiedzi w teście skrypt, który wykonuje R według klaster MapReduce i Spark zadania. Wróć do konsoli SSH i wpisz następujące polecenia w celu pobrania testowy skrypt do uruchomienia w RStudio.

    * Jeśli utworzono klastrze Hadoop z R, użyj tego polecenia.
        
            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r

    * Jeśli utworzono klaster Spark z R, użyj tego polecenia.

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

11. W RStudio zostanie wyświetlony skryptu test, który został pobrany. Kliknij dwukrotnie plik, aby go otworzyć, wybierz pozycję zawartość pliku i wybierz polecenie **Uruchom**. Powinien zostać wyświetlony wynik w okienku **konsoli** .
 
    ![Testowanie instalacji] (./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Testowanie instalacji")

Innym rozwiązaniem jest wpisać `source(testhdi.r)` lub `source(testhdi_spark.r)` wykonać skryptu.

## <a name="see-also"></a>Zobacz też

- [Obliczanie kontekstu opcje serwera R na klastrów HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure Opcje miejsca do magazynowania dla serwera R na HDInsight premium](hdinsight-hadoop-r-server-storage.md)


 
