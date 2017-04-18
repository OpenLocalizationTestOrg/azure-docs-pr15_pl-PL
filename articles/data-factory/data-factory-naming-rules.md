<properties 
    pageTitle="Dane Factory - nazewnictwa reguły | Microsoft Azure" 
    description="W tym artykule opisano reguł nazewnictwa dla obiektów Factory danych." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---naming-rules"></a>Azure danych Factory - nazewnictwa reguł 
Poniższa tabela zawiera reguł nazewnictwa dla artefaktów Factory danych.



Nazwa | Unikatowość nazw | Sprawdzanie poprawności
:--- | :-------------- | :----------------
Factory danych | Unikatowe przez Microsoft Azure. Nazwy wielkość liter, oznacza to, że MyDF i mydf odwołują się do tej samej fabryki danych. |<ul><li>Każdy factory danych jest związany dokładnie jedną subskrypcję Azure.</li><li>Nazwy obiektów musi rozpoczynać się od litery lub liczby i może zawierać tylko litery, liczby i znakiem minus (-).</li><li>Każdy znak minus (-) należy natychmiast poprzedzane i następuje numer lub litery. Kolejne kreski nie są dozwolone w nazwach kontenerów.</li><li>Nazwa może być 3-63 znaków.</li></ul>
Połączone usług i tabele i procesy | Unikatowe z w factory danych. Nazwy wielkość liter. | <ul><li>Maksymalna liczba znaków w nazwie tabeli: 260.</li><li>Nazwy obiektów musi się zaczynać od numeru litery lub podkreślenia (_).</li><li>Po znaki są niedozwolone: ".", "+", "?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul>
Grupa zasobów | Unikatowe przez Microsoft Azure. Nazwy wielkość liter. | <ul><li>Maksymalna liczba znaków: 1000.</li><li>Nazwa może zawierać litery, cyfry i następujących znaków: "-", "_",","i".".</li></ul>