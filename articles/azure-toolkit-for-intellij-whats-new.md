<properties
    pageTitle="Co nowego w Azure zestaw narzędzi dla IntelliJ | Microsoft Azure"
    description="Informacje o najnowszych funkcji narzędzi Azure dla IntelliJ."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Co nowego w Azure zestaw narzędzi dla IntelliJ

## <a name="azure-toolkit-for-intellij-releases"></a>Azure zestaw narzędzi dla wersji IntelliJ

Ten artykuł zawiera informacje na temat różnych wersji i najnowszych aktualizacji dla narzędzi Azure dla IntelliJ.

> [AZURE.NOTE] Istnieje także zestaw narzędzi Azure dla IDE Zaćmienie. Aby uzyskać więcej informacji zobacz [Zestaw narzędzi Azure dla programu Eclipse].

### <a name="august-26-2016"></a>26 sierpnia 2016

Zestaw narzędzi Azure dla IntelliJ - wersji 2016 sierpnia zawiera następujące ulepszenia:

* **Rozkład JDK niestandardowe**. Zestaw narzędzi Azure dla IntelliJ obsługuje teraz określanie i wdrażanie dowolnego wersji JDK do kontenera usługi Azure aplikacji sieci Web:
  - Oprócz JDKs dostarczony przez Azure można wybierać szeroki wybór wersji Zulu OpenJDK udostępnione Azure przez systemy Azul.
  - Można również określić własne rozkładu JDK podczas przesyłania jako pliku ZIP do swojego konta miejsca do magazynowania.
* **Ulepszenia widoku Eksplorator Azure**:
  - Obsługa zarządzania maszyn wirtualnych przy użyciu nowego modelu Menedżera zasobów firmy Azure: może listy, tworzyć i usuwać maszyn wirtualnych oparte na Menedżera zasobów bez opuszczania IDE.
  - Obsługa magazyn obiektów blob zarządzaniem za pomocą Menedżera zasobów Azure osoby, które uzupełnia istniejące funkcje do zarządzania kontami "klasycznego" przestrzeni dyskowej.
* **Sterownik Microsoft JDBC 6.0 dla programu SQL Server**. Ta aktualizacja zawiera najnowszy sterownik JDBC dla programu Microsoft SQL Server (wersja 6.0), która jest obecnie dostępny jako biblioteki, który można łatwo dodawać do projektów języka Java, co zastępowanie starszej wersji.

### <a name="june-29-2016"></a>29 czerwca 2016

Zestaw narzędzi Azure dla IntelliJ - wersji 2016 czerwca zawiera następujące ulepszenia:

* **Wymaganie Java 8**. Zestaw narzędzi Azure dla IntelliJ teraz wymaga Java 8, mimo że to wymaganie dotyczy tylko dla zestawu narzędzi — aplikacji nadal korzystać wszystkie wersje Java, które są obsługiwane przez Azure.
* **Obsługa najnowszych JDKs Java**. Najnowsze wersje Java JDKs są teraz obsługiwane przez zestaw narzędzi Azure dla IntelliJ.
* **Obsługa v2.9.1 Azure SDK**. Najnowsza wersja pakietu Azure jest teraz minimalne wstępnie wymagane dla narzędzi Azure dla IntelliJ.
* **Zintegrowane próbki**. Zestaw narzędzi Azure dla IntelliJ funkcji teraz kilka przykładowych aplikacji ułatwiające deweloperów rozpocząć pracę.
* **Integracja usługi HDInsight narzędzie**. Osoby Azure HDInsight narzędzia są teraz połączone z zestawu narzędzi Azure dla IntelliJ. Aby uzyskać więcej informacji zobacz [Dodatek Narzędzia HDInsight dla IntelliJ].
* **Zdalnego debugowania Java aplikacji sieci Web**. Zestaw narzędzi Azure dla IntelliJ obsługuje teraz zdalne debugowanie aplikacji sieci web języka Java na Azure aplikacji usługi.

### <a name="april-12-2016"></a>12 kwietnia 2016

Zestaw narzędzi Azure dla IntelliJ - wersji 2016 kwietnia zawiera następujące ulepszenia:

* **Obsługa v2.9.0 Azure SDK**. Najnowsza wersja pakietu Azure jest teraz minimalne wstępnie wymagane dla narzędzi Azure dla IntelliJ.
* **Różne użyteczności, czas reakcji i wydajności ulepszenia dotyczące obsługi aplikacji sieci Web Azure**. Liczba optymalizacji wydajności w jak zestaw narzędzi komunikuje Azure wynik w usprawnia interfejsu użytkownika.
* **Możliwość usuwania istniejącego kontenera aplikacji sieci Web platformy Azure z poziomu IntelliJ**. Zestaw narzędzi Azure dla IntelliJ umożliwia teraz usunąć istniejącego kontenera Azure Web bez opuszczania IntelliJ.

## <a name="see-also"></a>Zobacz też ##

Aby uzyskać więcej informacji na temat narzędzi Azure dla języka Java IDEs zobacz poniższe łącza:

- [Azure zestaw narzędzi dla programu Eclipse]
  - [Instalowanie narzędzi Azure dla programu Eclipse]
  - [Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w Zaćmienie]
  - [Co nowego w Azure zestaw narzędzi dla programu Eclipse]
- [Azure zestaw narzędzi dla IntelliJ]
  - [Instalowanie narzędzi Azure dla IntelliJ]
  - [Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ]
  - *Co nowego w Azure zestaw narzędzi dla IntelliJ (ten artykuł)*

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure].

<!-- URL List -->

[Azure zestaw narzędzi dla programu Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure zestaw narzędzi dla IntelliJ]: ./azure-toolkit-for-intellij.md
[Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w Zaćmienie]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Tworzenie aplikacji sieci Web dla Witaj świecie dla Azure w IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalowanie narzędzi Azure dla programu Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalowanie narzędzi Azure dla IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Co nowego w Azure zestaw narzędzi dla programu Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547

[Dodatek Narzędzia HDInsight dla IntelliJ]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
