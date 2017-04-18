<properties
pageTitle="Jak zaktualizować usługi w chmurze | Microsoft Azure"
description="Dowiedz się, jak zaktualizować usług w chmurze platformy Azure. Dowiedz się, jak aktualizacji usługi w chmurze przechodzi do zapewnienia dostępności."
services="cloud-services"
documentationCenter=""
authors="Thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/10/2016"
ms.author="adegeo"/>

# <a name="how-to-update-a-cloud-service"></a>Jak zaktualizować usługi w chmurze

## <a name="overview"></a>Omówienie
Na 10 000 stóp aktualizacji usługi w chmurze, takie jak zarówno ról i gościa, za pomocą systemu operacyjnego, jest procesem trzech fazach. Najpierw należy przekazać pliki binarne i pliki konfiguracyjne dla nowej usługi w chmurze lub wersja systemu operacyjnego. Następnie Azure zastrzega sobie zasobów sieci i obliczeń dla usług w chmurze na podstawie wymagań dotyczących nowej wersji usługi w chmurze. Na koniec Azure wykonuje uaktualnienia stopniowego stopniowo aktualizacja dzierżawy do nowej wersji lub Gość OS przy zachowaniu swojej dostępności. W tym artykule omówiono szczegóły ten ostatni krok — uaktualnienie stopniowe.

## <a name="update-an-azure-service"></a>Aktualizowanie usługi Azure

Azure są rozmieszczone wystąpienia do roli do grupy logiczne o nazwie uaktualnienia domen (UD). Uaktualnianie domeny (UD) są logiczne zestawy wystąpień roli, które zostały zaktualizowane jako grupy.  Azure zaktualizuje chmury usługi UD jeden w danym momencie, dzięki czemu wystąpienia w innych UDs, aby kontynuować obsługujące ruch.

Domyślny numer uaktualnienia domen jest 5. Możesz określić inną liczbę uaktualnienia domen, dołączając atrybut upgradeDomainCount w pliku definicji usługi (.csdef). Aby uzyskać więcej informacji na temat atrybutu upgradeDomainCount zobacz [WebRole schematu](https://msdn.microsoft.com/library/azure/gg557553.aspx) lub [Schematu WorkerRole](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Po wykonaniu aktualizacji w miejscu jednego lub większej liczby ról w usłudze Azure aktualizacji zestawy wystąpień roli według uaktualnienia domeny, do której należą. Azure aktualizacje, które wszystkich wystąpień w danej domenie uaktualnienia — zatrzymanie je zaktualizować je w sposób kopii online — następnie powoduje przejście do następnego domeny. Przez zatrzymywanie tylko wystąpienia uruchomiony w bieżącej domenie uaktualnienia Azure służy do sprawdzenia, nastąpi aktualizacja efektownych najmniejsze do uruchomionej usługi. Aby uzyskać więcej informacji zobacz [jak przechodzi aktualizacji](#howanupgradeproceeds) w dalszej części tego artykułu.

> [AZURE.NOTE] Gdy warunki, **Aktualizowanie** i **uaktualniania** znaczeniu różni w kontekście Azure, można ich używać zamiennie dla procesów i opisy funkcji w tym dokumencie.

Usługi należy zdefiniować co najmniej dwa wystąpienia roli dla danej roli zostaną zaktualizowane w miejscu bez przestojów. Jeśli usługa obejmuje tylko jedno wystąpienie jedna rola, usługi będą niedostępne do momentu zakończenia aktualizacji w miejscu.

W tym temacie opisano następujące informacje o aktualizacjach Azure:

-   [Dozwolone zmiany usługi podczas aktualizacji](#AllowedChanges)
-   [Jak przechodzi uaktualnienia](#howanupgradeproceeds)
-   [Wycofywanie aktualizacji](#RollbackofanUpdate)
-   [Inicjowanie wielu mutating operacji na bieżąco wdrażania](#multiplemutatingoperations)
-   [Rozkład ról domen uaktualnienia](#distributiondfroles)

<a name="AllowedChanges"></a>
## <a name="allowed-service-changes-during-an-update"></a>Dozwolone zmiany usługi podczas aktualizacji
W poniższej tabeli przedstawiono dozwolone zmiany z usługą podczas aktualizacji:

|Zmiany niedozwolonych hostingu, usług i ról|Aktualizacja w miejscu|Etapowej (VIP wymiany)|Usuwanie i ponowne wdrażanie|
|---|---|---|---|
|Wersja systemu operacyjnego|Tak|Tak|Tak
|Poziom zaufania .NET|Tak|Tak|Tak|
|Rozmiar maszyn wirtualnych<sup>1</sup>|Tak<sup>2</sup>|Tak|Tak|
|Ustawienia lokalne przechowywanie|Zwiększanie<sup>2</sup>|Tak|Tak|
|Dodawanie lub usuwanie ról w usłudze|Tak|Tak|Tak|
|Liczba wystąpień określonej roli|Tak|Tak|Tak|
|Liczba lub typ punkty końcowe usługi|Tak,<sup>2</sup>|Brak|Tak|
|Nazwy i wartości ustawienia konfiguracji|Tak|Tak|Tak|
|Wartości (ale nie nazwy) ustawienia konfiguracji|Tak|Tak|Tak|
|Dodawanie nowych certyfikatów|Tak|Tak|Tak|
|Zmienianie istniejącego certyfikatów|Tak|Tak|Tak|
|Wdrażanie nowy kod|Tak|Tak|Tak|
<sup>1</sup> Zmiana rozmiaru ograniczona do podzestaw rozmiarów dostępna dla usług w chmurze.

<sup>2</sup> Wymaga 1,5 SDK Azure lub nowszy.

> [AZURE.WARNING] Zmienianie rozmiaru maszyn wirtualnych będzie destroy danych lokalnych.


Podczas aktualizacji nie są obsługiwane następujące elementy:

-   Zmiana nazwy roli. Usuń, a następnie dodaj rolę z nową nazwą.
-   Zmiana liczba uaktualnianie domen.
-   Zmniejszanie rozmiaru zasobów lokalnych.

Jeśli tworzysz inne aktualizacje do definicji tej usługi, takie jak zmniejszyć rozmiar zasobów lokalnych, należy wykonać aktualizację wymiany VIP zamiast tego. Aby uzyskać więcej informacji zobacz [Wdrożenia Zamień](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>
## <a name="how-an-upgrade-proceeds"></a>Jak przechodzi uaktualnienia
Możesz zdecydować, czy chcesz zaktualizować wszystkie ról w usłudze lub pojedynczej roli w tej usłudze. W obu przypadkach wszystkie wystąpienia poszczególnych ról jest uaktualniany, które należą do pierwszego uaktualnienia domeny są zatrzymane, uaktualnione i powrócić do trybu online. Po powrocie do trybu online są, wystąpienia w drugiej domenie uaktualnienia są zatrzymane, uaktualnione i powrócić do trybu online. Usługa w chmurze może zawierać najwyżej jedno uaktualnienie aktywne w danej chwili. Uaktualnianie jest zawsze przeprowadzane w najnowszej wersji usługi w chmurze.

Na poniższym diagramie przedstawiono, jak uaktualnienie przechodzi w przypadku uaktualniania wszystkich ról w usłudze:

![Uaktualnienie usługi] (media/cloud-services-update-azure-service/IC345879.png "Uaktualnienie usługi")

Ten następnego diagramie przedstawiono, jak aktualizacji postępu uaktualniania tylko jedną rolę.

![Uaktualnianie ról] (media/cloud-services-update-azure-service/IC345880.png "Uaktualnianie ról")  

> [AZURE.NOTE] Podczas uaktualniania usługi z pojedynczego wystąpienia do wielu wystąpień tej usługi zostaną ulec awarii podczas uaktualniania odbywa się ze względu na sposób usługi Azure uaktualnienia. Dostępność usługi Umowa dotycząca poziomu gwarantujących usługi dotyczy tylko usług, które są wdrożone z więcej niż jednego wystąpienia. Na poniższej liście opisano, jak dane na każdym dysku dotyczy każdego scenariusza uaktualniania usługi Azure:
>
>Uruchom ponownie komputer maszyn wirtualnych:
>
-   C: zachowywane
-   D: zachowywane
-   E: zachowywane
>
>Portal Uruchom ponownie komputer:
>
-   C: zachowywane
-   D: zachowywane
-   E: zniszczone
>
>Reimage portalu:
>
-   C: zachowywane
-   D: zniszczone
-   E: zniszczone

>Uaktualnienie w miejscu:
>
-   C: zachowywane
-   D: zachowywane
-   E: zniszczone
>
>Węzeł migracji:
>
-   C: zniszczone
-   D: zniszczone
-   E: zniszczone

>Należy pamiętać, że na powyższej liście, dysk E: reprezentuje dysku głównym roli, nie powinny być stałe. Użyj zamiast tego zmienna % RoleRoot % środowiska reprezentować dysk.

>Aby zminimalizować przestoje podczas uaktualniania usługi jednego wystąpienia, należy wdrożyć nową usługę wielu wystąpień na serwerze i wykonaj wymiany VIP.

Podczas aktualizacji automatycznej kontroler tkaninie Azure okresowo oblicza kondycji usługi w chmurze do określenia, gdy jest to bezpieczne przeprowadził następnego UD. Ocena kondycji jest wykonywane na podstawie na rolach i są uwzględnione tylko wystąpienia w ramach najnowszej wersji (to znaczy wystąpienia z UDs, które już zostały udał). Weryfikuje, czy minimalna liczba wystąpień rolę, dla każdej roli osiągnęły zadowalający stan końcowych.

### <a name="role-instance-start-timeout"></a>Limit czasu rozpoczęcia wystąpienia roli
Kontroler tkaninie będzie czekać 30 minut dla każdego wystąpienia roli do osiągnięcia stan uruchomiony. Jeśli upływa limit czasu, kontroler tkaninie będzie nadal analizowanie do kolejnej roli.

<a name="RollbackofanUpdate"></a>
## <a name="rollback-of-an-update"></a>Wycofywanie aktualizacji
Azure udostępnia elastyczne zarządzanie usługami podczas aktualizacji, umożliwiając zainicjować dodatkowych operacji na usługi, po zaakceptowaniu żądania aktualizacji początkowej przez administratora tkaninie Azure. Wycofywania mogą być wykonywane tylko po aktualizacji (zmiana konfiguracji) lub uaktualnienie jest w stanie **w toku** w ramach wdrożenia. Aktualizacja lub uaktualnianie jest uważany za w trakcie wykonywania, ile istnieje co najmniej jedno wystąpienie usługi, która nie została jeszcze zaktualizowana do nowej wersji. Aby sprawdzić, czy jest dozwolona wycofywania, sprawdź wartość flagę RollbackAllowed zwrócone przez operacje [Uzyskiwanie wdrożenia](https://msdn.microsoft.com/library/azure/ee460804.aspx) i [Uzyskaj właściwości usługi Cloud](https://msdn.microsoft.com/library/azure/ee460806.aspx) , jest ustawiona na PRAWDA.

> [AZURE.NOTE] Tylko warto połączenia wycofywania aktualizację **w miejscu** lub uaktualnić, ponieważ uaktualnienia wymiany VIP obejmują zmieniania jednego całego uruchomionego wystąpienia tej usługi na inny.

Wycofywanie aktualizacji w trakcie wykonywania występują w ramach wdrożenia następujących efektów:

-   Wszystkie wystąpienia roli, które zostały jeszcze nie zostały zaktualizowane lub uaktualnione do nowej wersji nie są aktualizowane lub uaktualnione, ponieważ te wystąpienia uruchomionym wersji docelowej usługi.
-   Wszystkie wystąpienia roli, które zostały już zostały zaktualizowane lub uaktualnienie do nowej wersji pakietu z (\*.cspkg) pliku lub konfigurację usługi (\*.cscfg) pliku (lub oba pliki) zostaną anulowane przed uaktualnieniem wersji tych plików.

To funkcjonalny jest dostarczany przez następujące funkcje:

-   [Wycofywanie aktualizacji lub uaktualnić](https://msdn.microsoft.com/library/azure/hh403977.aspx) operacji, która może być wywoływana dla aktualizacji konfiguracji (wywołane połączeń [Konfiguracja wdrożenia zmiany](https://msdn.microsoft.com/library/azure/ee460809.aspx)) lub uaktualnienia (wywołane połączeń [Uaktualnienie wdrożenia](https://msdn.microsoft.com/library/azure/ee460793.aspx)), jak usługa, która nie została jeszcze zaktualizowana do nowej wersji jest co najmniej jedno wystąpienie.
-   Element zablokowane i elementu RollbackAllowed, które są zwracane jako część treść odpowiedzi operacji [Uzyskiwanie wdrożenia](https://msdn.microsoft.com/library/azure/ee460804.aspx) i [Uzyskaj właściwości usługi Cloud](https://msdn.microsoft.com/library/azure/ee460806.aspx) :
    1.  Element zablokowane umożliwia wykrycie operacji mutating może być wywoływana danego wdrożenia.
    2.  RollbackAllowed element umożliwia wykrycie operacji [Wycofywania aktualizacji lub uaktualnić](https://msdn.microsoft.com/library/azure/hh403977.aspx) może być wywoływana dla danego wdrożenia.

    Aby cofnąć, nie musisz Sprawdź zarówno zablokowane i elementy RollbackAllowed. Wystarczy upewnij się, że RollbackAllowed jest ustawiona na PRAWDA. Te elementy są zwracane tylko, jeśli te metody są wywoływane za pomocą nagłówka żądania, ustaw "x-ms wersja: 2011-10-01" lub nowszym. Aby uzyskać więcej informacji o nagłówkach przechowywania wersji zobacz [Usługa zarządzania wersji](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Istnieje kilka sytuacji, gdy wycofywania aktualizacji lub uaktualnienie nie jest obsługiwane, są następujące:

-   Zmniejszenie zasobów lokalnych — Jeśli aktualizacja zwiększa zasobów lokalnych dla roli platformie Azure nie zezwala na wycofywanie. 
-   Ograniczeń przydziału — Jeśli aktualizacja została skalę szczegółów operacji, które może być już masz wystarczające przydziału obliczeń, aby zakończyć operację wycofywania. Każdy Azure Subskrypcja obejmuje przydziału skojarzona określa maksymalną liczbę rdzenie, które może być używana przez wszystkich hostowanej usług, które należą do tej subskrypcji. Jeśli wykonywanie wycofywania danej aktualizacji czy umieść subskrypcji na przydziału, a następnie który wycofywania nie zostaną włączone.
-   Warunek wyścigowe — jeśli początkowe aktualizacja została ukończona, wycofywania nie jest możliwe.

Przykład podczas wycofywania aktualizacji mogą być przydatne jest, jeśli używasz operacji [Wdrażania uaktualnienie](https://msdn.microsoft.com/library/azure/ee460793.aspx) w trybie ręcznym do kontrolki, który jest wdrażania stawek, co ważne uaktualnienie w miejscu usługi Azure hostowana usługa.

Podczas fazy uaktualniania połączeń [Wdrożenia uaktualnienie](https://msdn.microsoft.com/library/azure/ee460793.aspx) w trybie ręcznym i zacząć przeprowadził uaktualnienia domen. Jeśli w pewnym momencie podczas uaktualniania można monitorować, możesz należy zauważyć, że w niektórych przypadkach ról w pierwszym uaktualnienia domen, które możesz sprawdzić stały nie odpowiada, możesz zadzwonić operacji [Wycofywania aktualizacji lub uaktualnić](https://msdn.microsoft.com/library/azure/hh403977.aspx) wdrożenia Pozostaw bez zmian, wystąpienia, które nie zostały jeszcze uaktualnione i przywracanie wystąpienia, które miały została uaktualniona do poprzedniego pakietu usług i konfiguracji.

<a name="multiplemutatingoperations"></a>
## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Inicjowanie wielu mutating operacji na bieżąco wdrażania
W niektórych przypadkach może być zainicjować wielu jednoczesnego operacji mutating na bieżąco wdrożenia. Na przykład może wykonywać aktualizacji usługi i podczas tej aktualizacji jest rozwijane przez usługi, należy niektóre zmiany, np. do użycia aktualizacji powrotem, zastosować inną aktualizację lub nawet usunąć wdrożenia. Przypadek, w którym to może być konieczne jest, jeśli uaktualniania usługi zawiera buggy kod, który powoduje wystąpienie roli uaktualnione wielokrotnie awarię. W tym przypadku kontroler tkaninie Azure nie będzie można dokonać postępu w stosowaniu, który uaktualnić, ponieważ liczba wystąpień w uaktualnionej domenie jest prawidłowy. Ten stan jest określane jako *zablokowany wdrożenia*. Czy przylegał rozmieszczenia, wycofywanie aktualizacji lub stosując świeży aktualizacji w górnej części którego nie można wykonać jedną.

Po otrzymaniu żądania początkowego zaktualizować lub uaktualnienie usługi Azure kontroler tkaninie można rozpocząć kolejne operacje mutacja. Oznacza to, że nie masz czekać na zakończenie przed rozpoczęciem innej operacji mutating początkowej operacji.

Inicjowanie drugiego operacji aktualizacji, gdy trwa pierwszej aktualizacji wykona podobne do wycofywanie operacji. Druga aktualizacja znajduje się w tryb automatycznego, pierwsza domena uaktualnienia zostanie uaktualniony od razu, prawdopodobnie prowadzące do wystąpienia z wielu domen uaktualnienia jest niedostępny w tym samym punkcie w czasie.

Operacje mutating są następujące: [Konfiguracja wdrożenia zmiany](https://msdn.microsoft.com/library/azure/ee460809.aspx), [Uaktualnianie wdrożenia](https://msdn.microsoft.com/library/azure/ee460793.aspx) [Stan wdrożenia aktualizacji](https://msdn.microsoft.com/library/azure/ee460808.aspx), [Usuń wdrożenia](https://msdn.microsoft.com/library/azure/ee460815.aspx)i [Wycofywania aktualizacji lub uaktualnić](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Dwie operacje [Uzyskiwanie wdrożenia](https://msdn.microsoft.com/library/azure/ee460804.aspx) i [Uzyskaj właściwości usługi Cloud](https://msdn.microsoft.com/library/azure/ee460806.aspx), zwracana flagę zablokowane, który może sprawdzić do określenia, czy operacji mutating może być wywoływana danego wdrożenia.

Aby nawiązać połączenie wersji tych metod, która zwraca flagę zablokowane, należy ustawić nagłówek "x-ms wersja: 2011-10-01" lub nowszym. Aby uzyskać więcej informacji o nagłówkach przechowywania wersji zobacz [Usługa zarządzania wersji](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>
## <a name="distribution-of-roles-across-upgrade-domains"></a>Rozkład ról domen uaktualnienia
Azure rozdziela wystąpienia roli równomiernie w ustalona liczba uaktualnienia domen, które można konfigurować w ramach usługi plik definicji (.csdef). Maksymalna liczba uaktualnienia domeny to 20 a wartością domyślną jest 5. Aby uzyskać więcej informacji o sposobie modyfikowania pliku definicji usługi zobacz [Schematu definicji usługi Azure (.csdef pliku)](cloud-services-model-and-package.md#csdef).

Na przykład jeśli Twoja rola ma dziesięć wystąpień, domyślnie każdej uaktualnienia domeny zawiera dwa wystąpienia. Jeśli Twoja rola ma wystąpień 14, cztery uaktualnienia domen zawiera trzy wystąpienia i piątym domeny zawiera dwa.

Uaktualnianie domeny są identyfikowane z indeksu: pierwszego uaktualnienia domeny ma identyfikator 0, a drugi uaktualnienia domeny ma identyfikator 1 i tak dalej.

Na poniższym diagramie przedstawiono, jak usługa nie zawiera dwie role są rozmieszczane przy usługę definiuje dwie domeny uaktualnienia. Usługa działa wystąpienia osiem ról w sieci web i dziewięciu wystąpienia roli pracownika.

![Rozkład uaktualnienia domen] (media/cloud-services-update-azure-service/IC345533.png "Rozkład uaktualnienia domen")

> [AZURE.NOTE] Należy zauważyć, że Azure steruje sposobu przydzielania wystąpienia domen uaktualnienia. Nie jest możliwe określenie, które wystąpienia zostało przydzielonych do której domeny.

## <a name="next-steps"></a>Następne kroki
[Jak zarządzać usługami w chmurze](cloud-services-how-to-manage.md)  
[Monitorowanie usług w chmurze](cloud-services-how-to-monitor.md)  
[Jak skonfigurować usług w chmurze](cloud-services-how-to-configure.md)  
