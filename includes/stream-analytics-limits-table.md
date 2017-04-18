<properties 
   pageTitle="Tabela limity analizy strumieniu"
   description="W tym artykule opisano ograniczenia systemu i zalecane rozmiarów składniki analizy strumieniu i połączenia."
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| Identyfikator limitu | Limit       | Komentarze |
|----------------- | ------------|--------- |
| Maksymalna liczba jednostek Streaming na subskrypcję w rozbiciu na regiony | 50 | Wniosek o zwiększenie przesyłanie strumieniowe jednostki dla subskrypcji ponad 50 będzie możliwe, kontaktując się z [Pomocą techniczną firmy Microsoft](https://support.microsoft.com/en-us). |
| Maksymalna przepustowość jednostki strumieniowego przesyłania | 1MB / s * | Maksymalna przepustowość na SU zależy od tego scenariusza. Rzeczywista wydajność może być mniejsza i zależy od złożoność kwerendy i podziału. Dodatkowe informacje można znaleźć w artykule [analizy strumieniu Azure skala zadania, aby zwiększyć produktywność](../articles/stream-analytics/stream-analytics-scale-jobs.md) . |
| Maksymalna liczba wartości wejściowych na zadanie | 60 | Istnieje górny limit 60 nakładów na zadanie analizy strumieniu. |
| Maksymalna liczba wyników na zadanie | 60 | Istnieje górny limit 60 wyjściowe na zadanie analizy strumieniu. |
| Maksymalna liczba funkcji na zadanie | 60 | Istnieje górny limit 60 funkcji na zadanie analizy strumieniu. |
| Maksymalna liczba zadań w rozbiciu na regiony | 1500 | Każdej subskrypcji może mieć maksymalnie 1500 zlecenia dla regionu geograficznego. |