<properties
    pageTitle="Publikowanie aplikacji usługi HDInsight | Microsoft Azure"
    description="Dowiedz się, jak tworzyć i publikować aplikacji HDInsight."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/18/2016"
    ms.author="jgao"/>

# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>Publikowanie aplikacji usługi HDInsight do Azure Marketplace

Aplikacja HDInsight jest aplikacja, z której użytkownicy mogą zainstalować w klastrze HDInsight systemem Linux. Te aplikacje mogą opracowane przez firmę Microsoft, niezależnych dostawców oprogramowania (Model) lub przez siebie. W tym artykule dowiesz się, jak opublikować aplikację HDInsight do Azure Marketplace.  Aby uzyskać ogólne informacje na temat publikowania do Azure Marketplace zobacz [Publikowanie ofertę Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

Usługa HDInsight aplikacji za pomocą modelu *Wyświetlić swój własny licencji (BYOL)* , gdzie dostawcy aplikacji jest odpowiedzialny za Licencjonowanie aplikacji dla użytkowników końcowych, a użytkownicy końcowi tylko jest naliczany przez Azure zasobów tworzonych, takie jak klaster HDInsight i jego maszyny wirtualne/węzłów. W tej chwili rozliczenia dla samej aplikacji nie odbywa się Azure.

Aplikacja HDInsight związane z artykułu:

- [Instalowanie usługi HDInsight aplikacji](hdinsight-apps-install-applications.md): Dowiedz się, jak zainstalować aplikację HDInsight do klastrów.
- [Instalowanie niestandardowej aplikacji HDInsight](hdinsight-apps-install-custom-applications.md): Dowiedz się, jak instalować i testować niestandardowe aplikacje HDInsight.

 
## <a name="prerequisites"></a>Wymagania wstępne

Aby przesłać niestandardowej aplikacji do witryny marketplace programu, musisz mieć utworzone i testowanych niestandardowej aplikacji. Zobacz następujące artykuły:

- [Instalowanie niestandardowej aplikacji HDInsight](hdinsight-apps-install-custom-applications.md): Dowiedz się, jak instalować i testować niestandardowe aplikacje HDInsight.

Należy także mieć zarejestrować Twojego konta dewelopera. Zobacz [Publikowanie ofertę Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) i [Utwórz konto Microsoft Developer](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Definiowanie aplikacji

Dostępne są dwie czynności wymaganych do publikowania aplikacji dla usługi Azure Marketplace.  Najpierw zdefiniować plik **createUiDef.json** , aby wskazać, które klastrów aplikacji jest zgodny z; a następnie opublikuj szablon z portalu Azure. Poniżej przedstawiono przykładowy plik createUiDef.json.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


|Pole  | Opis   | Możliwe wartości|
|-------|---------------|----------------|
|typy  | Typy klaster, które aplikacja jest zgodny z.    |Hadoop, HBase, burza, Spark (lub dowolną kombinację)|
|warstwy  | Klaster poziomów, które aplikacja jest zgodny z.    |Standardowy, Premium (lub oba)|
|wersje|  Typy klaster HDInsight, które jest zgodny z aplikacji.    |3.4|

## <a name="package-application"></a>Pakiet aplikacji

Tworzenie pliku zip zawierający wszystkie pliki wymagane do zainstalowania aplikacji HDInsight. Konieczne będzie pliku zip w [aplikacji Publikuj](#publish-application).

- [createUiDefinition.json](#define-application).
- mainTemplate.json. Zobacz próbki [instalować aplikacje niestandardowe HDInsight](hdinsight-apps-install-custom-applications.md).

    >[AZURE.IMPORTANT] Nazwa nazw skrypt instalacji aplikacji musi być unikatowa dla klastrze z formatem poniżej. Ponadto dowolne instalowania i odinstalowywania akcje skrypt powinien być idempotent, co oznacza skrypty można wywołać repeatly przy jednoczesnym taki sam wynik.
    
    >   Nazwa":" ["concat" ("odcień Zainstaluj v0","-", uniquestring('applicationName')] "
        
    >Należy zauważyć, że istnieją trzy elementy do nazwy skryptu:
        
    >   1. Skrypt prefiksu nazwy, który zawiera nazwę aplikacji lub nazwę odpowiedniego do aplikacji.
    >   2. A "-" o czytelności.
    >   3. Funkcja unikatowy ciąg razem z nazwą aplikacji jako parametr.

    >   Przykładem jest powyżej zakończy się staje się: odcień Zainstaluj v0-4wkahss55hlas na liście akcji trwałe skrypt. Aby uzyskać przykładowe ładunek JSON zobacz [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).

- Wszystkie wymagane skryptów.

> [AZURE.NOTE] Pliki aplikacji (w tym pliki aplikacji sieci web Jeśli występuje dowolny) może znajdować się na dowolnym publicznie punktu końcowego.

## <a name="publish-application"></a>Publikowanie aplikacji

Wykonaj poniższe czynności, aby opublikować aplikację HDInsight:

1. Logowanie się do [portal Azure publikowania](https://publish.windowsazure.com/).
2. Kliknij pozycję **Szablony rozwiązanie** z lewej strony, aby utworzyć nowy szablon rozwiązanie.
3. Wprowadź tytuł, a następnie kliknij pozycję **Utwórz nowy szablon rozwiązanie**.
3. Kliknij pozycję **Centrum deweloperów Utwórz konto i dołączanie do programu Azure** do rejestrowania Twojej firmy, jeśli nie zostało to zrobione.  Zobacz [Tworzenie konta Microsoft Developer](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
4. Kliknij przycisk **Definiuj niektórych topologii, aby rozpocząć pracę**. Szablon rozwiązanie jest "parent" do wszystkich jej topologii. Wiele topologii można zdefiniować w jeden szablon oferty i rozwiązanie. Jeśli oferta zostanie przypisany do przemieszczania, zostanie przypisany ze wszystkimi jej topologii. 
4. Wprowadź nazwę topologii, a następnie kliknij znak plus.
5. Wprowadź nową wersję, a następnie kliknij znak Plus.
6. Przekaż plik zip przygotować w [pakiet aplikacji](#package-application).  
7. Kliknij pozycję **żądanie certyfikacji**. Zespół certyfikacji Microsoft Przejrzyj listę plików i certyfikowanie topologii.

## <a name="next-steps"></a>Następne kroki

- [Instalowanie usługi HDInsight aplikacji](hdinsight-apps-install-applications.md): Dowiedz się, jak zainstalować aplikację HDInsight do klastrów.
- [Instalowanie niestandardowej aplikacji HDInsight](hdinsight-apps-install-custom-applications.md): Dowiedz się, jak wdrożyć aplikację HDInsight nie opublikowanych w HDInsight.
- [Przy użyciu akcji skryptu klastrów systemem Linux Dostosowywanie HDInsight](hdinsight-hadoop-customize-cluster-linux.md): Dowiedz się, jak zainstalować dodatkowe aplikacje za pomocą skryptu akcji.
- [Oparte na tworzenie Linux Hadoop klastrów w korzystania z szablonów Menedżera zasobów Azure HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Dowiedz się, jak połączeń szablony Menedżera zasobów do tworzenia HDInsight klastrów.
- [Użyj pustego krawędzi węzłów na HDInsight](hdinsight-apps-use-edge-node.md): Dowiedz się, jak użyć węzła pustych krawędzi uzyskiwania dostępu do usługi HDInsight klaster, testowania aplikacji HDInsight oraz hostingu aplikacji HDInsight.

