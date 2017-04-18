<properties
   pageTitle="Topologii Burza Apache przy użyciu programu Visual Studio i C# | Microsoft Azure"
   description="Dowiedz się, jak utworzyć topologii Burza języka C#, tworząc topologię Statystyka prostych programu word w programie Visual Studio za pomocą narzędzi HDInsight programu Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/27/2016"
   ms.author="larryfr"/>

# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>Można opracowywać C# topologii dla Burza Apache na HDInsight przy użyciu narzędzia Hadoop programu Visual Studio

Dowiedz się, jak utworzyć topologii C# Burza przy użyciu narzędzia HDInsight programu Visual Studio. Ten samouczek przeprowadzi przez proces tworzenia nowego projektu Burza w programie Visual Studio, przetestowaniu lokalnie i wdrożenie go na Burza Apache w klastrze HDInsight.

Będzie również sposób tworzenie topologii hybrydowe, korzystające z C# i składników Java.

> [AZURE.IMPORTANT] Podczas czynności opisane w tym dokumencie korzystania z systemu Windows środowiska Visual Studio, skompilowany projektu można wysłać do klastrów Linux lub HDInsight systemu Windows. Systemem Linux klastrów tworzone tylko po pomocy technicznej 2016-10-28 SCP.NET topologii.
>
> Aby użyć topologii C# z systemem Linux klaster, możesz zaktualizować pakiet Microsoft.SCP.Net.SDK NuGet używanych w danym projekcie do wersji 0.10.0.6 lub nowszej. Wersja pakietu musi się zgadzać wersji głównej Burza zainstalowanym HDInsight. Na przykład Burza w wersjach HDInsight 3.3 i 3.4 użyć wersji Burza 0.10.x podczas burzy korzysta z usługi HDInsight 3.5 1.0.x.
> 
> C# topologii na podstawie Linux klastrów należy użyć 4,5 .NET i używać jedno do uruchamiania w klastrze HDInsight. Większość poleceń będzie działać, jednak należy sprawdzić dokument [Zgodności jedno](http://www.mono-project.com/docs/about-mono/compatibility/) pod kątem potencjalnych niezgodności.

## <a name="prerequisites"></a>Wymagania wstępne

- Jedną z następujących wersji programu Visual Studio

    - Program Visual Studio 2012 w przypadku [aktualizacji 4](http://www.microsoft.com/download/details.aspx?id=39305)

    - Visual Studio 2013 [Aktualizacja 4](http://www.microsoft.com/download/details.aspx?id=44921) lub [Program Visual Studio 2013 społeczności](http://go.microsoft.com/fwlink/?LinkId=517284)

    - Visual Studio 2015 lub [Programu Visual Studio 2015 społeczności](https://go.microsoft.com/fwlink/?LinkId=532606)

- Azure SDK 2.9.5 lub nowsza

- Usługa HDInsight Tools for Visual Studio: [Wprowadzenie do korzystania z usługi HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) , aby zainstalować i skonfigurować narzędzia HDInsight programu Visual Studio.

    > [AZURE.NOTE] Narzędzia HDInsight programu Visual Studio nie są obsługiwane w Visual Studio Express

-   Burza Apache w klastrze HDInsight: [Wprowadzenie do Burza Apache na HDInsight](hdinsight-apache-storm-tutorial-get-started.md) czynności aby utworzyć klaster.

## <a name="templates"></a>Szablony

Narzędzia HDInsight programu Visual Studio zapewniają następujące szablony:

| Typ projektu | Zaprezentowano |
| ------------ | ------------- |
| Aplikacja Burza | Pusty projekt topologii Burza |
| Przykładowe Writer Azure SQL Burza | Jak zapisywać w bazie danych SQL Azure |
| Przykładowe czytnika DocumentDB Burza | Jak odczytać z Azure DocumentDB |
| Przykładowe DocumentDB Writer Burza | Jak zapisać Azure DocumentDB |
| Przykładowe czytnika EventHub Burza | Jak odczytać z koncentratorów zdarzenia Azure |
| Przykładowe Edytor EventHub Burza | Jak pisać do koncentratorów zdarzenia Azure |
| Przykładowe czytnika HBase Burza | Jak odczytać z HBase na klastrów HDInsight |
| Przykładowe Edytor HBase Burza | Jak zapisać HBase na klastrów HDInsight |
| Przykładowe hybrydowych Burza | Jak używać składnika Java |
| Przykładowe Burza | Topologia Statystyka podstawowe programu word |

> [AZURE.NOTE] Przykłady czytnika i Twórca HBase za pomocą interfejsu API usługi REST HBase można komunikować się z HBase na klastrze HDInsight, nie HBase API języka Java.

W krokach w tym dokumencie będzie utworzyć nową topologię za pomocą odpowiedniego typu projektu Burza aplikacji.

## <a name="create-a-c-topology"></a>Tworzenie topologii C#

1. Jeśli nie masz już zainstalowany najnowszej wersji narzędzia HDInsight programu Visual Studio, zobacz [rozpocząć korzystanie z narzędzia HDInsight programu Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Otwórz program Visual Studio, wybierz **plik** > **Nowy**, a następnie **projektu**.

3. Na ekranie **Nowego projektu** rozwiń **zainstalowano** > **Szablony**i wybierz pozycję **HDInsight**. Z listy szablonów wybierz **Aplikację Burza**. U dołu ekranu wprowadź **WordCount** jako nazwa aplikacji.

    ![Obraz](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. Po utworzeniu projektu powinien mieć następujące pliki:

    - **Plik program.cs**: definiuje topologię projektu. Pamiętaj, że domyślną topologię składający się z jednego dziobek i jeden śrubę jest utworzone domyślnie.

    - **Spout.cs**: dziobek przykład, który emituje liczby losowe.

    - **Bolt.cs**: błyskawicy przykład, która śledzi liczbę elementów dostarczanych przez dziobek.

    Podczas tworzenia projektu najnowszych [pakietów SCP.NET](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/) będą pobierane z NuGet.

    [AZURE.INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

W następnej sekcji należy zmodyfikować tego projektu do podstawowej aplikacji WordCount.

### <a name="implement-the-spout"></a>Implementowanie dziobek

1. Otwórz **Spout.cs**. Spouts są używane do odczytu z zewnętrznego źródła danych w topologii. Główne składniki dziobek są:

    - **NextTuple**: wywoływane przez burza, gdy dziobek może emitować nowej krotki.

    - **ACK** (tylko w przypadku topologii transakcji): obsługuje inicjowane przez inne składniki topologii dla krotki wysłanych z tym dziobek potwierdzenia. Przyznając krotki umożliwia dziobek, o których wiesz, że jej został pomyślnie przetworzony przez składniki podrzędne.

    - **Niepowodzenie** (tylko w przypadku topologii transakcji): obsługuje krotki, które są błędów przetwarzania inne składniki w topologii. Dzięki temu możliwość ponownego emitować krotki, dzięki czemu mogą być przetwarzane ponownie.

2. Zamień treść klasy **Spout** następujące czynności. Spowoduje to utworzenie dziobek, który losowo emituje zdania w topologii.

        private Context ctx;
        private Random r = new Random();
        string[] sentences = new string[] {
            "the cow jumped over the moon",
            "an apple a day keeps the doctor away",
            "four score and seven years ago",
            "snow white and the seven dwarfs",
            "i am at two with nature"
        };

        public Spout(Context ctx)
        {
            // Set the instance context
            this.ctx = ctx;

            Context.Logger.Info("Generator constructor called");

            // Declare Output schema
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // The schema for the default output stream is
            // a tuple that contains a string field
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        }

        // Get an instance of the spout
        public static Spout Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Spout(ctx);
        }

        public void NextTuple(Dictionary<string, Object> parms)
        {
            Context.Logger.Info("NextTuple enter");
            // The sentence to be emitted
            string sentence;

            // Get a random sentence
            sentence = sentences[r.Next(0, sentences.Length - 1)];
            Context.Logger.Info("Emit: {0}", sentence);
            // Emit it
            this.ctx.Emit(new Values(sentence));

            Context.Logger.Info("NextTuple exit");
        }

        public void Ack(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }

        public void Fail(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }
    
    Poświęć chwilę na zapoznaj się z komentarzy, aby dowiedzieć się, co oznacza ten kod.

### <a name="implement-the-bolts"></a>Implementowanie tekst "Śruby"

1. Usuń istniejący plik **Bolt.cs** z projektu.

2. W **Eksploratorze rozwiązań**, kliknij prawym przyciskiem myszy projektu, a następnie wybierz pozycję **Dodaj** > **Nowy element**. Z listy zaznacz **Błyskawicy Burza**, a następnie wprowadź **Splitter.cs** jako nazwę. Powtórz tę opcję, aby utworzyć drugą bolcem nazwany **Counter.cs**.

    - **Splitter.cs**: wykonuje błyskawicy, który dzieli zdania na pojedyncze wyrazy i emituje nowego strumienia wyrazów.

    - **Counter.cs**: wykonuje beli zlicza każdego wyrazu, który emituje nowego strumienia wyrazów i liczbę elementów dla każdego wyrazu.

    > [AZURE.NOTE] Te tekst "Śruby" tylko do odczytu i zapisu strumienie, ale beli umożliwia komunikowanie się za pomocą źródłami, takimi jak bazy danych lub usługi.

3. Otwórz **Splitter.cs**. Uwaga Domyślnie za zawiera tylko jedną metodę: **Wykonywanie**. Jest to po odebraniu śruby krotki przetwarzania. W tym miejscu znajdziesz proces krotki przychodzących i emitować krotki ruchu wychodzącego.

4. Zastąp treść klasy **podziału** następujący kod:

        private Context ctx;

        // Constructor
        public Splitter(Context ctx)
        {
            Context.Logger.Info("Splitter constructor called");
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // Input contains a tuple with a string field (the sentence)
            inputSchema.Add("default", new List<Type>() { typeof(string) });
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // Outbound contains a tuple with a string field (the word)
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance of the bolt
        public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Splitter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the sentence from the tuple
            string sentence = tuple.GetString(0);
            // Split at space characters
            foreach (string word in sentence.Split(' '))
            {
                Context.Logger.Info("Emit: {0}", word);
                //Emit each word
                this.ctx.Emit(new Values(word));
            }

            Context.Logger.Info("Execute exit");
        }

    Poświęć chwilę na zapoznaj się z komentarzy, aby dowiedzieć się, co oznacza ten kod.

5. Otwórz **Counter.cs** i zastąpić zawartość zajęć z następujących czynności:

        private Context ctx;

        // Dictionary for holding words and counts
        private Dictionary<string, int> counts = new Dictionary<string, int>();

        // Constructor
        public Counter(Context ctx)
        {
            Context.Logger.Info("Counter constructor called");
            // Set instance context
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string field - the word
            inputSchema.Add("default", new List<Type>() { typeof(string) });

            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string and integer field - the word and the word count
            outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance
        public static Counter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Counter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the word from the tuple
            string word = tuple.GetString(0);
            // Do we already have an entry for the word in the dictionary?
            // If no, create one with a count of 0
            int count = counts.ContainsKey(word) ? counts[word] : 0;
            // Increment the count
            count++;
            // Update the count in the dictionary
            counts[word] = count;

            Context.Logger.Info("Emit: {0}, count: {1}", word, count);
            // Emit the word and count information
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
            Context.Logger.Info("Execute exit");
        }

    Poświęć chwilę na zapoznaj się z komentarzy, aby dowiedzieć się, co oznacza ten kod.

### <a name="define-the-topology"></a>Definiowanie topologii

Na wykresie, która określa sposób przepływu danych między składnikami są rozmieszczane spouts i tekst "Śruby". Ta topologia wykres jest w następujący sposób:

![Obraz przedstawiający sposób rozmieszczenia składników](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Zdania emisję z dziobek, które są rozmieszczone w wystąpieniach śruby podziału. Śruby pasek podziału podziały zdania do wyrazów, które są rozdzielane śruby licznik.

Ponieważ wyrazów jest przechowywanych lokalnie w wystąpieniu licznika, chcemy upewnij się, że określone wyrazy przepływ w tym samym wystąpieniu błyskawicy licznik, więc możemy tylko jedno wystąpienie rejestrowanie informacji o określony wyraz. Ale dla śruby podziału naprawdę nie ma znaczenia, która błyskawicy otrzymuje które zdania, więc chcemy po prostu obciążenie zdań saldo tych wystąpień.

Otwórz **Plik Program.cs**. Metoda ważne jest **GetTopologyBuilder**, który jest używany do definiowania topologii, który jest przesyłany do Burza. Zastąp zawartość **GetTopologyBuilder** następujący kod w celu zaimplementowania topologii wspomniano:

        // Create a new topology named 'WordCount'
        TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

        // Add the spout to the topology.
        // Name the component 'sentences'
        // Name the field that is emitted as 'sentence'
        topologyBuilder.SetSpout(
            "sentences",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
            },
            1);
        // Add the splitter bolt to the topology.
        // Name the component 'splitter'
        // Name the field that is emitted 'word'
        // Use suffleGrouping to distribute incoming tuples
        //   from the 'sentences' spout across instances
        //   of the splitter
        topologyBuilder.SetBolt(
            "splitter",
            Splitter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
            },
            1).shuffleGrouping("sentences");

        // Add the counter bolt to the topology.
        // Name the component 'counter'
        // Name the fields that are emitted 'word' and 'count'
        // Use fieldsGrouping to ensure that tuples are routed
        //   to counter instances based on the contents of field
        //   position 0 (the word). This could also have been
        //   List<string>(){"word"}.
        //   This ensures that the word 'jumped', for example, will always
        //   go to the same instance
        topologyBuilder.SetBolt(
            "counter",
            Counter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
            },
            1).fieldsGrouping("splitter", new List<int>() { 0 });

        // Add topology config
        topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
        {
            {"topology.kryo.register","[\"[B\"]"}
        });

        return topologyBuilder;

Poświęć chwilę na zapoznaj się z komentarzy, aby dowiedzieć się, co oznacza ten kod.

## <a name="submit-the-topology"></a>Przesyłanie topologii

1. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy projektu, a następnie wybierz pozycję **Prześlij, aby Burza na HDInsight**.

    > [AZURE.NOTE] Jeśli zostanie wyświetlony monit, wprowadź poświadczenia logowania dla subskrypcji Azure. Jeśli masz więcej niż jedną subskrypcję, zaloguj się do szablonu, który zawiera usługi Burza w klastrze HDInsight.

2. Wybierz swojego Burza w klastrze HDInsight z listy rozwijanej **Klaster Burza** , a następnie wybierz pozycję **Prześlij**. Można monitorować, jeśli przekazywania zakończyło się pomyślnie za pomocą okna **dane wyjściowe** .

3. Po pomyślnym przesłaniu topologię **Topologii Burza** dla klaster powinien być wyświetlany. Topologia **WordCount** wybierz z listy, aby wyświetlić informacje o topologii uruchomionego.

    > [AZURE.NOTE] Możesz także wyświetlić **Topologii Burza** Eksploratora **Serwera**: rozwijanie **Azure** > **HDInsight**, kliknij prawym przyciskiem myszy Burza w klastrze HDInsight, a następnie wybierz **Widok Burza topologii**.

    Umożliwia wyświetlanie informacji dotyczących tych składników łącza spouts lub tekst "Śruby". Nowe okno zostanie otwarty dla każdego wybrany element.

4. W widoku **Podsumowanie topologii** kliknij **skasować** , aby zatrzymać topologii.

    > [AZURE.NOTE] Topologii Burza nadal działać, dopóki nie zostaną one wyłączone lub usunięto klaster.

## <a name="transactional-topology"></a>Transakcji topologii

Poprzedni topologia jest wartością transakcji. Składniki w topologii nie implementowania dowolnego odtwarzanie wiadomości, jeśli przetwarzanie nie powiedzie się przez składnik w topologii. Dla topologii transakcji przykład tworzenie nowego projektu i wybierz **Przykładowy Burza** jako typu projektu.

Transakcji topologii wykonania następujących czynności, aby obsługiwać powtarzania danych:

- **Buforowanie metadanych**: dziobek muszą być przechowywane metadane dotyczącego danych emisji, aby dane można pobrać i emisję ponownie w przypadku awarii. Ponieważ dane wysyłane przez próbka jest mały, dane dla każdej krotki znajduje się w słowniku powtarzania.

- **ACK**: każdego błyskawicy w topologii można zadzwonić `this.ctx.Ack(tuple)` w celu potwierdzenia pomyślnie występują przetwarzane krotki. Jeśli wszystkie śruby prześledzone krotki, `Ack` metody dziobek. Dzięki temu dziobek usunąć buforowane powtarzania, ponieważ dane całkowicie została przetworzona.

- **Niepowodzenie**: każdego błyskawicy można zadzwonić `this.ctx.Fail(tuple)` oznaczającą, że przetwarzanie nie dla krotki. Awarii propaguje do `Fail` metody dziobek, w którym można odtworzyć krotki przy użyciu buforowanych metadanych.

- **Identyfikator sekwencji**: gdy wysyłających krotki, można określić identyfikator sekwencji. Należy to wartość identyfikująca krotki przetwarzania powtarzania (Ack i błędów). Na przykład dziobek w programie project **Przykładowe Burza** korzysta z następujących po wysyłających danych:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    To emituje nowej krotki zawierający zdania w strumieniu domyślne z identyfikatorem sekwencji zawarte w **lastSeqId**. W tym przykładzie **lastSeqId** jest po prostu zwiększany dla każdej krotki emisji.

Jak pokazano w **Burza przykładowy** projekt, czy składnik jest transakcji można ustawić w czasie wykonywania na podstawie konfiguracji.

## <a name="hybrid-topology"></a>Topologia hybrydowego

Narzędzia HDInsight programu Visual Studio można również utworzyć topologii hybrydowe, gdzie są niektóre składniki C#, a niektóre Java.

Dla topologii hybrydowych przykład tworzenie nowego projektu i wybierz **Burza hybrydowych próbki**. Spowoduje to utworzenie pełni komentarze próbka, która zawiera kilka topologii, pokazujących, następujące czynności:

- **Java spout** i **C# śrubę**: zdefiniowane w **HybridTopology_javaSpout_csharpBolt**

    - Transakcji wersji jest zdefiniowana w **HybridTopologyTx_javaSpout_csharpBolt**

- **C# spout** i **śrubę Java**: zdefiniowane w **HybridTopology_csharpSpout_javaBolt**

    - Transakcji wersji jest zdefiniowana w **HybridTopologyTx_csharpSpout_javaBolt**

        > [AZURE.NOTE] Ta wersja również przedstawiono sposób użycia kodu Clojure z pliku tekstowego jako składnik Java.

Aby przełączyć się między topologię, która jest używana po przesłaniu projektu, po prostu przesuń `[Active(true)]` instrukcji topologii ma być używany przed przesłaniem go z klastrem.

> [AZURE.NOTE] Wszystkie pliki języka Java, które są wymagane są dostarczane w ramach tego projektu w folderze **JavaDependency** .

Należy pamiętać o następujących podczas tworzenia i przesyłania topologii hybrydowych:

- **JavaComponentConstructor** musi można używać do tworzenia nowego wystąpienia klasy języka Java dziobek lub błyskawicy.

- **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** należy szeregować danych do lub z składników Java z obiektów Java JSON.

- Podczas przesyłania topologię na serwerze, należy użyć opcji **dodatkowo** określ **ścieżki pliku Java**. Ścieżka określona powinny być katalog zawierający pliki JAR, które zawierają swojej klasy języka Java.

### <a name="azure-event-hubs"></a>Koncentratory Azure zdarzenia

SCP.Net wersji 0.9.4.203 wprowadzono nową klasę i metody specjalnie do pracy z wydarzeniem Centrum Spout (Java dziobek który odczytuje z Centrum zdarzenia.) Podczas tworzenia topologii używa tej dziobek, należy skorzystać z następujących metod:

- Klasy **EventHubSpoutConfig** : tworzy obiekt, który zawiera konfiguracji składnika dziobek

- Metoda **TopologyBuilder.SetEventHubSpout** : dodanie składnika Spout Centrum zdarzeń do topologii

> [AZURE.NOTE] Gdy te ułatwienia pracy z Centrum zdarzenia Spout niż inne składniki Java, musisz nadal za pomocą CustomizedInteropJSONSerializer szeregować wytwarzane przez dziobek danych.

## <a id="configurationmanager"></a>Przy użyciu ConfigurationManager

Nie za pomocą ConfigurationManager wartości konfiguracji pobrać z błyskawicy i spout składników; Spowoduje to wyjątku wskaźnik null. Zamiast tego konfiguracji projektu jest przekazywany do topologii Burza jako parę klucz wartość w kontekście topologii. Każdy składnik, który zależy od wartości konfiguracji należy je przywrócić z kontekstu podczas inicjowania.

Poniższy kod przedstawia sposób pobrać te wartości:

    public class MyComponent : ISCPBolt
    {
        // To hold configuration information loaded from context
        Configuration configuration;
        ...
        public MyComponent(Context ctx, Dictionary<string, Object> parms)
        {
            // Save a copy of the context for this component instance
            this.ctx = ctx;
            // If it exists, load the configuration for the component
            if(parms.ContainsKey(Constants.USER_CONFIG))
            {
                this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
            }
            // Retrieve the value of "Foo" from configuration
            var foo = this.configuration.AppSettings.Settings["Foo"].Value;
        }
        ...
    }

Jeśli korzystasz z `Get` metody zwraca wystąpienie składnika, należy się upewnić, że przekazuje obie `Context` i `Dictionary<string, Object>` parametrów konstruktora. Poniższy przykład to basic `Get` metodę prawidłowo przekaże te wartości:

    public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new MyComponent(ctx, parms);
    }

## <a name="how-to-update-scpnet"></a>Jak zaktualizować SCP.NET

Najnowsze wersje SCP.NET obsługuje uaktualnienia pakietu za pośrednictwem NuGet. Dostępna jest nowa aktualizacja, otrzymasz powiadomienie o uaktualnieniu. Aby ręcznie sprawdzić do uaktualnienia, wykonaj następujące czynności:

1. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy projektu i wybierz pozycję **Zarządzaj NuGet pakietów**.

2. W Menedżerze pakietów wybierz **aktualizacje**. Jeśli jest dostępna aktualizacja, będzie on wymieniony. Kliknij przycisk **Aktualizuj** pakietu go zainstalować.

> [AZURE.IMPORTANT] Jeśli projekt został utworzony przy użyciu jednej z wcześniejszych wersji programu SCP.NET, który nie użył NuGet aktualizacje pakietu, należy wykonać następujące czynności, aby zaktualizować do nowej wersji:
>
> 1. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy projektu i wybierz pozycję **Zarządzaj NuGet pakietów**.
> 2. Za pomocą pola **wyszukiwania** , Wyszukaj, a następnie dodać, **Microsoft.SCP.Net.SDK** do projektu.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="null-pointer-exceptions"></a>Wyjątki wskaźnik zerowy

Korzystając z klastrem systemem Linux HDInsight topologii C#, śruby i spout składników korzystających z ConfigurationManager do czytania, że ustawienia konfiguracji w czasie wykonywania może zwracać wyjątki pustego wskaźnika. Dzieje się tak, ponieważ konfiguracja załadowanego domeny nie jest z zestawu, który zawiera projektu.

Konfiguracja projektu jest przekazywany do topologii Burza jako parę klucz wartość w kontekście topologii i mogą być pobierane z obiektu słownika, który jest przekazywany do składniki, gdy są one inicjowane.

W poniższym przykładzie pokazano ładowania wartości konfiguracji z kontekstu topologii, zobacz sekcję [ConfigurationManager](#configurationmanager) tego dokumentu.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Korzystając z klastrem systemem Linux HDInsight topologii C#, można napotkać następujący komunikat o błędzie:

    System.TypeLoadException: Failure has occurred while loading a type.

Tym zazwyczaj przyszłą podczas używania binarne to nie jest zgodna z wersją programu .NET, która obsługuje jedno.

Dla klastrów systemem Linux HDInsight należy upewnić używanego plików binarnych dla 4,5 .NET w projekcie.


### <a name="test-a-topology-locally"></a>Testowanie topologię lokalnie

Mimo że ułatwia wdrażanie topologii z klastrem, w niektórych przypadkach może być konieczne przetestować topologii lokalnie. Wykonaj następujące czynności, aby uruchomić i przetestować topologię przykład w tym samouczku lokalnie w środowisku rozwoju.

> [AZURE.WARNING] Lokalne badania działa tylko dla podstawowe, C# tylko topologii. Nie należy używać lokalnego badań dla topologii hybrydowego lub topologii, które używają wielu strumieni otrzymasz błędy.

1. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy projektu, a następnie wybierz pozycję **Właściwości**. W oknie dialogowym właściwości projektu zmień **Typ danych wyjściowych** na **Aplikacji konsoli**.

    ![Typ danych wyjściowych](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] Pamiętaj zmienić **Typ danych wyjściowych** na **Biblioteka klas** przed wdrożeniem topologii klastrze.

2. W **Eksploratorze rozwiązań**, kliknij prawym przyciskiem myszy projektu, a następnie wybierz pozycję **Dodaj** > **Nowy element**. Wybierz **klasy** i wprowadź **LocalTest.cs** jako nazwę klasy. Na koniec kliknij przycisk **Dodaj**.

3. Otwórz **LocalTest.cs** i dodaj następującą instrukcję **przy użyciu** u góry:

        using Microsoft.SCP;

4. Jak zawartość zajęć **LocalTest** , należy użyć następującej składni:

        // Drives the topology components
        public void RunTestCase()
        {
            // An empty dictionary for use when creating components
            Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

            #region Test the spout
            {
                Console.WriteLine("Starting spout");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext spoutCtx = LocalContext.Get();
                // Get a new instance of the spout, using the local context
                Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

                // Emit 10 tuples
                for (int i = 0; i < 10; i++)
                {
                    sentences.NextTuple(emptyDictionary);
                }
                // Use LocalContext to persist the data stream to file
                spoutCtx.WriteMsgQueueToFile("sentences.txt");
                Console.WriteLine("Spout finished");
            }
            #endregion

            #region Test the splitter bolt
            {
                Console.WriteLine("Starting splitter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext splitterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


                // Set the data stream to the data created by the spout
                splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    splitter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                splitterCtx.WriteMsgQueueToFile("splitter.txt");
                Console.WriteLine("Splitter bolt finished");
            }
            #endregion

            #region Test the counter bolt
            {
                Console.WriteLine("Starting counter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext counterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Counter counter = Counter.Get(counterCtx, emptyDictionary);

                // Set the data stream to the data created by splitter bolt
                counterCtx.ReadFromFileToMsgQueue("splitter.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    counter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                counterCtx.WriteMsgQueueToFile("counter.txt");
                Console.WriteLine("Counter bolt finished");
            }
            #endregion
        }

    Poświęć chwilę na zapoznaj się z komentarzy do kodu. Kod używa **LocalContext** do uruchomienia składniki w środowisku projektowania, a istnieje strumienia danych między składnikami do plików tekstowych na dysku lokalnym.

5. Otwórz **Plik Program.cs** i dodaj następujące metody **główne** :

        Console.WriteLine("Starting tests");
        System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
        // Initialize the runtime
        SCPRuntime.Initialize();

        //If we are not running under the local context, throw an error
        if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
        {
            throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
        }
        // Create test instance
        LocalTest tests = new LocalTest();
        // Run tests
        tests.RunTestCase();
        Console.WriteLine("Tests finished");
        Console.ReadKey();

6. Zapisanie zmian, a następnie kliknij **F5** lub wybierz **Debugowanie** > **Rozpocznij debugowanie** , aby rozpocząć projektu. Okno konsoli należy są wyświetlane i rejestrować stan postępu testów. Po **zakończeniu testów** zostanie wyświetlone, naciśnij dowolny klawisz, aby zamknąć okno.

7. Zlokalizuj katalog zawierający projektu, na przykład za pomocą **Eksploratora Windows** **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. W tym katalogu Otwórz **Kosza**, a następnie kliknij **Debugowanie**. Powinien zostać wyświetlony pliki tekstowe, które zostały wyprodukowane podczas próby uruchomienia: sentences.txt, counter.txt i splitter.txt. Otwórz każdego pliku tekstowego, a następnie przeprowadź inspekcję danych.

    > [AZURE.NOTE] Ciąg danych jest zachowywane jako tablicę dziesiętnych w tych plików. Na przykład \[[97,103,111]] w **splitter.txt** plik znajduje się wyraz "i".

Mimo że testowanie podstawowe wyraz zliczanie lokalnie aplikacja jest dosyć prosty, liczba rzeczywista wartość pochodzi zawierających złożone topologii, która komunikuje się z zewnętrznymi źródłami danych lub wykonuje złożonych analiz danych. Podczas pracy nad projektem takie, może być konieczne ustawianie punktów kontrolnych i kroków kod w składniki do wyodrębnienia problemów.

> [AZURE.NOTE] Należy ustawić **Typ projektu** powrót do **Biblioteka klas** przed wdrożeniem dla Burza w klastrze HDInsight.

### <a name="log-information"></a>Informacje dziennika

Można łatwo rejestrowanie informacji z składniki topologii, za pomocą `Context.Logger`. Na przykład poniższa utworzy wpis dziennika informacyjnych:

    Context.Logger.Info("Component started");

Zarejestrowane informacje mogą być wyświetlane z **Dziennika usługi Hadoop**, który znajduje się w **Eksploratorze serwera**. Rozwiń pozycję dla swojego Burza w klastrze HDInsight, a następnie rozwiń węzeł **Dziennika usługi Hadoop**. Na koniec wybierz plik dziennika, aby wyświetlić.

> [AZURE.NOTE] Dzienniki są przechowywane na koncie magazyn Azure, jest używana przez klaster. W przypadku subskrypcji inny niż ten, który jest zalogowany do przy użyciu programu Visual Studio, musisz zalogować się do subskrypcji, która zawiera konto przestrzeni dyskowej, aby wyświetlić te informacje.

### <a name="view-error-information"></a>Wyświetlanie informacji o błędzie

Aby wyświetlić błędy, które wystąpiły w topologii uruchomionego, wykonaj następujące czynności:

1. Korzystając z **Eksploratora serwera**kliknij prawym przyciskiem myszy Burza w klastrze HDInsight, a następnie wybierz **Widok Burza topologii**.

2. **Spout** i **śrub**kolumny **Ostatni błąd** będzie zawierać informacje ostatni błąd, który wystąpił.

3. Wybierz **Spout identyfikator** lub **Identyfikator błyskawicy** dla składnika wystąpił błąd na liście. Na stronie Szczegóły wyświetlane dodatkowe informacje o błędzie znajdzie się w sekcji **błędy** w dolnej części strony.

4. Aby uzyskać więcej informacji, należy wybrać **Port** w sekcji **testamentu** stronie, aby wyświetlić dziennik pracownik Burza ostatniej kilka minut.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już opracowywanie i wdrażanie topologii Burza na karcie Narzędzia HDInsight programu Visual Studio, Dowiedz się, jak [zdarzenia procesu z koncentratora zdarzenia Azure za pomocą Burza na HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).

Na przykład topologii C#, który dzieli transmisji danych na wielu strumieni zobacz [przykład C# Burza](https://github.com/Blackmist/csharp-storm-example).

Aby odkryć więcej informacji o tworzeniu C# topologii, odwiedź stronę [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Inne sposoby pracy z usługi HDInsight i więcej Burza na próbkach HDinsight zobacz następujące artykuły:

**SCP.NET firmy Microsoft**

* [Podręcznik programowania połączenia](hdinsight-storm-scp-programming-guide.md)

**Burza Apache na HDInsight**

- [Wdrażanie i monitorowanie topologii z Burza Apache na HDInsight](hdinsight-storm-deploy-monitor-topology.md)

- [Przykład topologii dla Burza na HDInsight](hdinsight-storm-example-topology.md)

**Apache Hadoop na HDInsight**

- [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

- [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)

- [MapReduce za pomocą Hadoop na HDInsight](hdinsight-use-mapreduce.md)

**Apache HBase na HDInsight**

- [Wprowadzenie do programu HBase na HDInsight](hdinsight-hbase-tutorial-get-started.md)
