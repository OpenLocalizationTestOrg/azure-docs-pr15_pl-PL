<properties
 pageTitle="Opis i usuwanie błędów WebHCat na HDInsight"
 description="Dowiedz się, jak do około typowe błędy zwrócone przez WebHCat na HDInsight oraz sposób ich rozwiązania."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
 tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="09/27/2016"
 ms.author="larryfr"/>

#<a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>Opis i naprawianie błędów otrzymywanych od WebHCat (Templeton), na HDInsight

Podczas pracy z usługi HDInsight, przy użyciu WebHCat (wcześniej nazywanego Templeton,) może się pojawić błędy. Ten dokument zawiera wskazówki dotyczące typowych błędów — Dlaczego występują i co można zrobić, aby je rozwiązać.

##<a name="what-is-webhcat"></a>Co to jest WebHCat?

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) jest interfejsu API usługi REST warstwy zarządzania [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tabelę i miejsca do magazynowania dla Hadoop. WebHCat jest domyślnie włączona w klastrów HDInsight i jest używany przez różne narzędzia do przesyłania zadań, Pobierz stan zadania itp bez logowania się z klastrem.

##<a name="modifying-configuration"></a>Modyfikowanie konfiguracji

> [AZURE.IMPORTANT] Występuje wiele błędów wymienionych w tym dokumencie, ponieważ przekroczono skonfigurowane maksimum. Gdy kroku rozwiązywania wzmianki, można zmienić wartość, należy użyć jedną z następujących Aby dokonać zmiany:

* W przypadku klastrów **systemu Windows** : za pomocą akcji skrypt można skonfigurować wartość podczas tworzenia klaster. Aby uzyskać więcej informacji zobacz [opracowanie skryptu akcje](hdinsight-hadoop-script-actions.md).

* W przypadku klastrów **Linux** : używanie Ambari (sieć web lub interfejsu API usługi REST), aby zmodyfikować wartość. Aby uzyskać więcej informacji zobacz [Zarządzanie HDInsight przy użyciu Ambari](hdinsight-hadoop-manage-ambari.md)

###<a name="default-configuration"></a>Domyślna konfiguracja

Poniżej przedstawiono wartości konfiguracji domyślnych, które mogą mieć wpływ na wydajność WebHCat lub powodować błędy, jeśli te wartości:

| Ustawienie | Działanie | Wartość domyślna |
| ------- | ------------ | ------------- |
| [yarn.Scheduler.Capacity.Maximum aplikacji][maximum-applications] | Maksymalna liczba zadań, które mogą być wykonywane jednocześnie (oczekiwanie lub nie działa) | 10 000 |
| [templeton.EXEC.max-procs][max-procs] | Maksymalna liczba żądań, które mogą być obsługiwane jednocześnie | 20 |
| [mapreduce.jobhistory.max wieku ms][max-age-ms] | Liczba dni, które zostaną zachowane historię zatrudnienia | 7 dni |

##<a name="too-many-requests"></a>Zbyt wiele żądań

**Kod stanu HTTP**: 429

| Przyczyna | Rozdzielczość |
| ----- | ---------- |
| Został przekroczony maksymalny równoczesne żądania obsługiwane przez WebHCat na minutę (domyślnie 20) | Zmniejszanie usługi Obciążenie pracą, aby upewnić się, że nie przesyłać więcej niż maksymalną liczbę żądań lub zwiększyć limit równoczesne żądania zmieniając `templeton.exec.max-procs`. Aby uzyskać więcej informacji, zobacz [Modyfikowanie Konfiguracja](#modifying-configuration) |

##<a name="server-unavailable"></a>Serwer niedostępny

**Kod stanu HTTP**: 503

| Przyczyna | Rozdzielczość |
| ---------------- | ------------------- |
| Zazwyczaj dzieje podczas pracy awaryjnej między HeadNode głównego i pomocniczego dla klaster | Zaczekaj dwie minuty, a następnie ponowić próbę |

##<a name="bad-request-content-could-not-find-job"></a>Nieprawidłowe żądanie zawartości: nie można odnaleźć zadania

**Kod stanu HTTP**: 400

| Przyczyna | Rozdzielczość |
| ---------------- | ------------------- |
| Szczegóły zadania zostały wyczyszczone w górę, historię zatrudnienia filtra | Domyślny okres przechowywania historii zadań jest 7 dni. Można go zmienić, modyfikując `mapreduce.jobhistory.max-age-ms`. Aby uzyskać więcej informacji, zobacz [Modyfikowanie Konfiguracja](#modifying-configuration) |
| Zadania został zatrzymany z powodu trybie awaryjnym | Spróbuj ponownie przesyłania zadania dla dwóch minut |
| Użyto nieprawidłowy identyfikator zadania | Sprawdzanie poprawności identyfikatora zadania |

##<a name="bad-gateway"></a>Nieprawidłowa brama

**Kod stanu HTTP**: 502

| Przyczyna | Rozdzielczość |
| ---------------- | ------------------- |
| Wewnętrzna śmieci występuje w ramach procesu WebHCat | Czekaj na śmieci do zakończenia lub ponowne uruchamianie usługi WebHCat |
| Oczekiwanie na odpowiedź z usługi ResourceManager limit czasu. Taka sytuacja może wystąpić, gdy liczba aktywnych aplikacji skonfigurowane maksimum (domyślnie 10 000) | Poczekaj, aż obecnie uruchomione zadań do wykonania lub zwiększyć limit równoczesne zadania zmieniając `yarn.scheduler.capacity.maximum-applications`. Aby uzyskać więcej informacji, zobacz [Modyfikowanie Konfiguracja](#modifying-configuration)  |
| Podczas próby pobrania wszystkich zadań za pomocą wywołania [Uzyskiwanie /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) podczas `Fields` jest ustawiona na`*` | Nie pobrać *Wszystkie* szczegóły zadania, użyj zamiast tego `jobid` do pobierania szczegóły dotyczące zadań tylko większa niż niektórych identyfikator zadania. Nie używaj`Fields` |
| Usługa WebHCat nie działa podczas pracy awaryjnej HeadNode | Czekaj na dwóch minut i spróbuj ponownie operacji |
| Istnieje więcej niż 500 oczekujących zadań przesłane przez WebHCat | Poczekaj na ukończenie zadania obecnie Oczekiwanie przed przesłaniem więcej zadań |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
 
