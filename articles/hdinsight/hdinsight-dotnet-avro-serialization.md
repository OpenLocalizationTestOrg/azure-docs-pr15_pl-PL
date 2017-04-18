<properties
    pageTitle="Szeregować danych za pomocą Microsoft Avro Library | Microsoft Azure"
    description="Dowiedz się, jak usługa Azure HDInsight używa Avro do szeregować duży danych."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Szeregować danych w Hadoop z Microsoft Avro Library

W tym temacie przedstawiono, jak za pomocą <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Biblioteki Avro Microsoft</a> szeregować obiektów i inne struktury danych do strumieni było utrzymują je do pamięci, bazy danych lub plik, a także jak deserializować je odzyskać oryginalnych obiektów.

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##<a name="apache-avro"></a>Apache Avro
<a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Biblioteki Avro Microsoft</a> zawiera system szeregowania danych Apache Avro dla środowiska Microsoft.NET. Apache Avro przewiduje format wymiany danych binarnych niewielkie szeregowania. Aby zdefiniować schematu niezależne od języka, w którym ubezpiecza współdziałanie języków używa <a href="http://www.json.org" target="_blank">JSON</a> . W drugim można odczytywać danych seryjnych w jednym języku. C, C++, C#, Java, PHP, Python i dopiskiem są obecnie obsługiwane. Szczegółowe informacje na temat formatu można znaleźć w <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Specyfikacji Avro Apache</a>. Należy zauważyć, że bieżącą wersję pakietu Microsoft Avro Library nie obsługuje zdalnego wywołania część tej specyfikacji.

Seryjnych reprezentacją obiektu w systemie Avro składa się z dwóch części: schematu i rzeczywista wartość. Schemat Avro zawiera opis modelu danych niezależny od języka seryjne danych za pomocą JSON. Jest Prezentuj boki postać binarna danych. O schemacie oddzielone od postać binarna pozwala zapisany przy użyciu nie koszty ogólne na wartość, ułatwiając szeregowania szybkie i reprezentacją małe wszystkich obiektów.

##<a name="the-hadoop-scenario"></a>Scenariusz Hadoop
Format szeregowania Apache Avro powszechnie jest używany w innych środowiskach Apache Hadoop i Azure HDInsight. Avro zapewnia wygodny sposób reprezentować złożone struktury danych w ramach zadania Hadoop MapReduce. Formatu plików Avro (plik kontenera obiektu Avro) zaprojektowano do obsługi rozłożone MapReduce model programowania. Funkcja klucza, która umożliwia rozkład są "masowej" w znaczeniu, że jeden szukanie dowolnego miejsca w pliku i rozpocząć odczyt z określonego bloku pliki.

##<a name="serialization-in-avro-library"></a>Szeregowania w bibliotece Avro
Biblioteki .NET Avro obsługuje szeregowania obiektów na dwa sposoby:

- **odbicie** - schematu JSON dla typów automatycznie składa się z danych atrybuty umowy typów .NET szeregowania.
- **rodzajowy rekordu** - JSON schematu jest jawnie wymienione w rekordzie reprezentowany przez klasy [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) , gdy nie typy .NET są obecne w celu opisania schemat danych można szeregować.

Gdy schemat danych jest znany writer i czytnika strumienia, dane mogą zostać wysłane bez jego schematu. W przypadku użycia pliku Avro obiektu kontenera schematu znajduje się w pliku. Można określić inne parametry, takie jak koder-dekoder używane do kompresji danych. Następujące scenariusze są opisane szczegółowo i przedstawione w poniższych przykładach kodu.


## <a name="install-avro-library"></a>Instalowanie Avro biblioteki

Poniżej przedstawiono wymagane przed zainstalowaniem biblioteki:

- <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Program Microsoft .NET Framework 4</a>
- <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 lub nowsza)

Należy zauważyć, że zależność Newtonsoft.Json.dll są pobierane automatycznie z instalacją pakietu Microsoft Avro Library. Procedura ta znajduje się w następnej sekcji.


Microsoft Avro Library jest rozpowszechniane jako pakiet NuGet, którą można zainstalować z programu Visual Studio przy użyciu następującej procedury:

1. Wybierz kartę **Projekt** -> **Zarządzanie NuGet pakietów...**
2. Wyszukaj "Microsoft.Hadoop.Avro" w polu **Wyszukiwania w trybie Online** .
3. Kliknij przycisk **Zainstaluj** obok pozycji **Microsoft Azure HDInsight Avro Biblioteka**.

Należy zauważyć, że Newtonsoft.Json.dll (> = 6.0.4) współzależności również są pobierane automatycznie z Microsoft Avro Library.

Być może zechcesz odwiedź <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">stronę główną programu Microsoft Avro biblioteki</a> , aby przeczytać informacje o wersji.


Kod źródłowy Microsoft Avro Library jest dostępna na <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">stronę główną programu Microsoft Avro biblioteki</a>.

##<a name="compile-schemas-using-avro-library"></a>Można skompilować schematy — za pomocą biblioteki Avro

Microsoft Avro Library zawiera narzędzie generowanie kodu, które umożliwia tworzenie C# typów automatycznie na podstawie uprzednio zdefiniowanej JSON schematu. Narzędzie do generowania kodu nie jest rozpowszechniane jako binarne plik wykonywalny, ale można łatwo można utworzyć za pomocą następującej procedury:

1. Pobierz plik zip z najnowszą wersją kodu źródłowego HDInsight SDK z <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">programu Microsoft .NET SDK dla Hadoop</a>. (Kliknij ikonę **pobierania** , nie na karcie **pliki do pobrania** ).

2. Wyodrębnianie SDK HDInsight do katalogu na komputerze przy użyciu programu .NET Framework 4 zainstalowane i połączenie z Internetem pobierania pakietów NuGet niezbędne zależności. Poniżej firma Microsoft będzie przyjęto założenie, że kod źródłowy jest wyodrębnione do C:\SDK.

3. Przejdź do folderu C:\SDK\src\Microsoft.Hadoop.Avro.Tools i uruchom build.bat. (Plik nawiąże połączenie MSBuild z rozkładem 32-bitowej programu .NET Framework. Jeśli chcesz korzystać z wersji 64-bitowej, Edycja build.bat po komentarze w pliku). Upewnij się, że kompilacja się pomyślnie. (W niektórych systemach MSBuild może powodować ostrzeżeń. Ostrzeżenia te nie wpływają na narzędzie ile istnieje już błędy kompilacji.)

4. Skompilowany narzędzie znajduje się w C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.


Aby zapoznać się z składni wiersza polecenia, wykonaj następujące polecenie z folderu, w którym znajduje się narzędzie generowanie kodu:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

Aby przetestować narzędzie, istnieje możliwość wygenerowania klasy C# z dostarczoną z kodu źródłowego pliku schematu przykładowe JSON. Wykonaj następujące polecenie:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

To ma tworzenia plików dwóch C# w bieżącym katalogu: SensorData.cs i Location.cs.

Aby zrozumieć logiki, która narzędziu generowanie kodu jest używany podczas konwertowania schematu JSON na typach C#, zobacz plik, który GenerationVerification.feature znajduje się w C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.

Pamiętaj, że przestrzenie nazw są wyodrębniane ze schematu JSON, za pomocą logiki opisany w pliku wymienionych w poprzednim akapicie. Przestrzenie nazw wyodrębnionych z schematu pierwszeństwo niezależnie od znajduje się przy użyciu parametru /n w wierszu polecenia narzędzia. Jeśli chcesz zastąpić nazw znajdujących się w schemacie, należy użyć parametru /nf. Na przykład aby zmienić wszystkie przestrzenie nazw z SampleJSONSchema.avsc my.own.nspace, należy wykonać następujące polecenie:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="samples"></a>Przykłady
Sześć przykładach opisane w tym temacie przedstawiono różnych scenariuszach obsługiwanych przez Microsoft Avro Library. Microsoft Avro Library jest przeznaczona do pracy ze strumieniem. W tych przykładach danych jest manipulować za pośrednictwem strumienie pamięci zamiast strumieni plików lub baz danych dla uproszczenia i spójność. Podejście w środowisku produkcyjnym zależy wymagania dokładnie scenariusza, źródła danych i głośność, ograniczeń wydajności oraz innych czynników.

Dwa pierwsze przykładach pokazano, jak szeregować i deserializacji danych do buforów strumienia pamięci za pomocą odbicie i ogólnego rekordów. Współużytkowanie czytniki i autorzy przyjmowana jest schematem w tych dwóch przypadkach w nowym oknie grupy.

Trzeci i czwarty przykładach pokazano, jak szeregować i deserializacji danych przy użyciu plików kontenera obiektu Avro. Jego schematu dane są przechowywane w pliku kontenera Avro, jest zawsze przechowywane z nim ponieważ schematu musi być udostępniana do deserializacji.

Próbki zawierającej pierwsze cztery przykłady można pobrać z witryny <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-86055923" target="_blank">Przykłady kodu Azure</a> .

W piątym przykładzie pokazano sposób jak za pomocą niestandardowych kompresji kodera-dekodera Avro obiektu kontenera plików. Próbki zawierającego kod w tym przykładzie można pobrać z witryny <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111" target="_blank">Przykłady kodu Azure</a> .

Przykładowe szóstym pokazano, jak za pomocą Avro szeregowania przekazywanie danych z magazynem obiektów Blob platformy Azure i analizować je przy użyciu gałęzi klaster HDInsight (Hadoop). Czy można pobrać z witryny <a href="https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Przykłady kodu Azure</a> .

Poniżej wymieniono sześć próbek omawiane w tym temacie:

 * <a href="#Scenario1">**Szeregowania z odbiciem**</a> - schematu JSON typów szeregowania automatycznie składa się z danych atrybuty umowy.
 * <a href="#Scenario2">**Szeregowania z rekordem ogólnego**</a> - JSON schematu jest jawnie wymienione w rekordzie, gdy żaden typ .NET jest dostępna dla odbicie.
 * <a href="#Scenario3">**Szeregowania przy użyciu obiektu kontenera plików z odbiciem**</a> - JSON schematu jest automatycznie wbudowane i udostępniane wraz z seryjnych danych za pośrednictwem pliku Avro obiektu kontener.
 * <a href="#Scenario4">**Szeregowania przy użyciu obiektu kontenera plików z rekordem ogólnego**</a> - JSON schematu jest jawnie określony przed szeregowania i udostępniane wraz z danych za pośrednictwem pliku Avro obiektu kontener.
 * <a href="#Scenario5">**Szeregowania przy użyciu obiektu kontenera plików za pomocą niestandardowych kompresji kodera-dekodera**</a> — przykład przedstawiono sposób tworzenia pliku kontenera obiektu Avro przy użyciu niestandardowych implementacji .NET kompresji danych Deflate kodera-dekodera.
 * <a href="#Scenario6">**Za pomocą Avro do przekazania danych dla usługi Microsoft Azure HDInsight**</a> — przykład przedstawia interakcji szeregowania Avro z usługą HDInsight. Aktywną subskrypcję Azure i dostęp do klastrów Azure HDInsight są potrzebne do uruchamiania w tym przykładzie.

###<a name="Scenario1"></a>Przykład 1: Szeregowania z odbiciem

Schemat JSON typów mogą być automatycznie wbudowane przez Microsoft Library Avro za pośrednictwem odbicie danych umowy atrybutów obiektów C# szeregowania. Tworzy Microsoft Avro Library [**IAvroSeralizer<T> **](http://msdn.microsoft.com/library/dn627341.aspx) do identyfikowania pola, które mają być seryjnych.

W tym przykładzie obiektów (klasa **SensorData** o strukturze **lokalizacji** członka) są szeregować do strumienia pamięci, a z kolei jest rozszeregować tym strumieniu. Wynikiem jest porównywane do początkowej wystąpienia, aby potwierdzić, że obiekt **SensorData** odzyskać jest identyczny z oryginalną.

Schemat w tym przykładzie zakłada się, że udostępniane między czytniki i autorzy, więc format kontenera obiektu Avro nie jest wymagane. Na przykład jak szeregować i deserializacji danych do buforów pamięci przy użyciu odbicie format kontenera obiektu podczas schematu musi być udostępniane dane zobacz <a href="#Scenario3">szeregowania przy użyciu obiektu kontenera plików z odbiciem</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-2-serialization-with-a-generic-record"></a>Przykład 2: Szeregowania z rekordem ogólne

Schemat JSON można jawnie wymienione w rekordzie ogólnego po nie można użyć odbicie, ponieważ nie można przedstawić dane za pośrednictwem klas .NET z umową danych. Ta metoda jest zazwyczaj jest mniejsza niż przy użyciu odbicia. W takich przypadkach w schemacie dla danych mogą też być dynamiczne, to znaczy jest nieznany w czasie kompilacji. Dane przedstawione w formacie wartości rozdzielanych przecinkami (CSV), którego schemat jest nieznany, aż przekształcania do formatu Avro w czasie wykonywania są przykładem tego rodzaju scenariusz dynamiczne.

W tym przykładzie przedstawiono sposób tworzenia i za pomocą [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) jawnie określ schemat JSON, jak wypełnić go z danymi, a następnie jak szeregować i deserializacji go. Wynikiem jest porównywane do początkowej wystąpienia, aby potwierdzić, że rekord odzyskać jest identyczny z oryginalną.

Schemat w tym przykładzie zakłada się, że udostępniane między czytelnicy i autorzy, więc format kontenera obiektu Avro nie jest wymagane. Na przykład jak szeregować i deserializacji danych do buforów pamięci przy użyciu rekordu ogólny format kontenera obiektu podczas schematu musi zostać dołączony seryjnych danych Zobacz przykład <a href="#Scenario4">szeregowania przy użyciu obiektu kontenera plików z rekordem ogólne</a> .


    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn will be then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Przykład 3: Szeregowania przy użyciu obiektu kontenera plików i szeregowania z odbiciem

W tym przykładzie jest podobny do tego scenariusza w <a href="#Scenario1">pierwszym przykładzie</a>miejsce, w którym niejawnie określono schematu z odbiciem. Różnica jest tutaj, schemat jest nie przyjmuje się, że określane z czytnika, który deserializes go. Obiekty **SensorData** szeregowania i ich niejawnie określonym schematem są przechowywane w pliku kontenera obiektu Avro reprezentowany przez klasę [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) .

Dane jest seryjny w tym przykładzie z [**SequentialWriter<SensorData> **](http://msdn.microsoft.com/library/dn627340.aspx) i rozszeregowanym z [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Wynik jest następnie porównywane początkowej wystąpieniach zapewnienie tożsamości.

Dane w pliku kontenera obiektu jest skompresowany za pomocą domyślnego [**Deflate**] [ deflate-100] koder-dekoder kompresji z .NET Framework 4. Zobacz <a href="#Scenario5">przykład piątym</a> w tym temacie opisano sposób do korzystania z nowszej i najwyższej wersji [**Deflate**] [ deflate-110] koder-dekoder kompresji jest dostępna w .NET Framework 4,5.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will be compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Próbka 4: Za pomocą obiektu kontenera plików i szeregowania z rekordem ogólnego szeregowania

W tym przykładzie jest podobny do tego scenariusza w <a href="#Scenario2">drugim przykładzie</a>miejsce, w którym schematu jest jawnie wymienione z JSON. Różnica jest tutaj, schemat jest nie przyjmuje się, że określane z czytnika, który deserializes go.

Test zestawu danych jest zbierane w postaci listy obiektów [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) za pośrednictwem jawnie zdefiniowane schematu JSON i przechowywać w pliku kontenera obiektu reprezentowany przez klasę [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) . Ten plik kontenera tworzy writer, który służy do szeregować danych, nieskompresowane ze strumieniem pamięci, który jest następnie zapisane w pliku. Parametr [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) służy do tworzenia czytnik określa te dane nie zostaną skompresowane.

Dane są następnie odczytu z pliku i rozszeregowany do kolekcji obiektów. Kolekcji jest porównywane wstępny wykaz Avro rekordów w celu potwierdzenia, że są identyczne.


    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will not be compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




###<a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Próbka 5: Szeregowania korzystanie z kodera-dekodera niestandardowe kompresji plików kontenera obiektu

W piątym przykładzie pokazano sposób jak za pomocą niestandardowych kompresji kodera-dekodera Avro obiektu kontenera plików. Próbki zawierającego kod w tym przykładzie można pobrać z witryny [Przykłady kodu Azure](http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111) .

[Specyfikacja Avro](http://avro.apache.org/docs/current/spec.html#Required+Codecs) umożliwia zastosowania kodera-dekodera kompresja (oprócz **wartości Null** i **Deflate** ustawienia domyślne). W tym przykładzie nie jest implementacji całkowicie nowy koder-dekoder takich jak Snappy (wymienionych jako obsługiwany codec opcjonalnie w [Specyfikacji Avro](http://avro.apache.org/docs/current/spec.html#snappy)). Pokazano, jak używać programu .NET Framework 4,5 stosowania [**Deflate**] [ deflate-110] koder-dekoder, która zapewnia lepsze algorytmu kompresji oparte na bibliotece kompresji [zlib](http://zlib.net/) niż domyślnej wersji .NET Framework 4.


    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It will catch the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it will rely on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

###<a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Przykład 6: Za pomocą Avro do przekazania danych dla usługi Microsoft Azure HDInsight

Przykład szóstym ilustruje technik programowania związane z interakcja z usługą HDInsight platformy Azure. Próbki zawierającego kod w tym przykładzie można pobrać z witryny [Przykłady kodu Azure](https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3) .

Próbki wykonuje następujące czynności:

* Nawiązuje połączenie z istniejącym klastrem usługi HDInsight.
* Serializes kilka plików CSV i wysyła wynik z magazynem obiektów Blob platformy Azure. (Z plików CSV są rozdzielane razem z próbki i reprezentują wyciąg z danych historycznych giełdowy załączniku rozdzielonych [Infochimps](http://www.infochimps.com/) okres 1970 2010. Próbka odczytuje dane pliku w formacie CSV, konwertuje rekordy wystąpienia klasy **giełdowy** i serializes je przy użyciu odbicia. Definicja typu giełdowy jest tworzona z przez narzędzie Microsoft Avro Library kod generowanie schematu JSON.
* Powoduje utworzenie nowej tabeli zewnętrznej zwanych **zasobów** w gałęzi i łącza do danych przekazane w poprzednim kroku.
* Wykonuje kwerendy przy użyciu gałęzi przez tabeli **zasobów** .

Ponadto próbki wykonuje procedurę oczyszczania przed i po wykonaniu operacji głównych. Podczas oczyszczania zostaną usunięte wszystkie powiązane dane obiektów Blob platformy Azure i folderów, a tabela gałąź jest usuwana. Można również wywołania procedury oczyszczania z poziomu wiersza polecenia próbki.

Próbka ma następujące wymagania:

* Aktywną subskrypcję Microsoft Azure i jego identyfikator subskrypcji.
* Certyfikat zarządzania subskrypcji z odpowiedni klucz prywatny. Należy zainstalować certyfikat w magazynie bieżącego użytkownika prywatne na komputerze używanych do uruchamiania próbki.
* Aktywne klaster HDInsight.
* Klaster HDInsight połączone konta magazynu platformy Azure z poprzedniego wymagania wstępne razem z odpowiedni klucz podstawowy i pomocniczy programu access.

Wszystkie informacje z wymagania wstępne należy wprowadzić do przykładowy plik konfiguracyjny przed próbki. Istnieją dwa sposoby to zrobić:

* Edytowanie pliku app.config w katalogu głównym próbki, a następnie skonstruuj próbki
* Najpierw skompilować przykład, a następnie edytuj AvroHDISample.exe.config w katalogu kompilacji

W obu przypadkach należy wykonywać wszystkie zmiany w **<appSettings>** w sekcji Ustawienia. Wykonaj komentarze w pliku.
Próbka jest uruchamiany z poziomu wiersza polecenia, wykonując następujące polecenie (w przeciwnym razie miejsce, w którym plik zip z próbki został przyjmuje się, że wyodrębnione do C:\AvroHDISample; Jeśli Użyj ścieżkę odpowiedniego pliku):

    AvroHDISample run C:\AvroHDISample\Data

Aby wyczyścić klaster, uruchom następujące polecenie:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
