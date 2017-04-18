<properties
    pageTitle="Tworzenie aplikacji sieci web w środowisku usługi aplikacji"
    description="Dowiedz się, jak tworzenie aplikacji sieci web i aplikacji planów usług w środowisku usługi aplikacji"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>Tworzenie aplikacji sieci web w środowisku usługi aplikacji

## <a name="overview"></a>Omówienie

Ten samouczek pokazano, jak utworzyć aplikacji sieci web i plany usługi aplikacji w [Środowisku usługi aplikacji](app-service-app-service-environment-intro.md) (ASE). 

> [AZURE.NOTE] Jeśli chcesz dowiedzieć się, jak utworzyć aplikację sieci web, ale nie musisz wykonywać go w środowisku usługi aplikacji, zobacz [Tworzenie aplikacji sieci web .NET](web-sites-dotnet-get-started.md) lub jeden z powiązanych samouczki dla innych języków i struktury.

## <a name="prerequisites"></a>Wymagania wstępne

Ten samouczek założono, że użytkownik utworzył środowisku usługi aplikacji. Jeśli użytkownik jeszcze nie, zobacz [Tworzenie środowisku usługi aplikacji](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci web

1. W [Azure Portal](https://portal.azure.com/)kliknij **Nowy > Web + Mobile > aplikacji sieci Web**. 

    ![][1]

2. Wybierz subskrypcję.  

    Jeśli masz wiele subskrypcji należy pamiętać, że do tworzenia aplikacji w środowisku usługi aplikacji, należy użyć tej samej subskrypcji, który został użyty podczas tworzenia środowiska. 

3. Wybierz lub Utwórz grupę zasobów.

    *Grupy zasobów* umożliwiają zarządzanie zasoby pokrewne Azure jako jednostki i są przydatne, tworząc reguły *Kontrola dostępu oparta na rolach* (RBAC) dla aplikacji. Aby uzyskać więcej informacji, zobacz [Zarządzanie Azure zasobów][ResourceGroups]. 

4. Wybierz lub Utwórz plan usług aplikacji.

    *Plany aplikacji usługi* są zarządzane zestawów aplikacji sieci web.  Zwykle należy wybrać cennik, pobierana jest stosowana do planu usługi aplikacji, a nie do poszczególnych aplikacji. W ASE płacisz wystąpień obliczeń przydzielono ASE zamiast co to jest informacja strona ASP.  Rozbudowy liczbę wystąpień aplikacji sieci web, którą rozbudowy wystąpienia usługi aplikacji planu i wpływa na wszystkie aplikacje sieci web w programie.  Niektóre funkcje, takie jak gniazdach witryny lub integracji VNET mieć także ograniczenia dotyczące ilości w planie.  Aby uzyskać więcej informacji zobacz [Omówienie planów Azure aplikacji usługi](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

    Należy określić plany aplikacji usługi w swojej ASE, sprawdzając lokalizację, do której należy zauważyć, w obszarze Nazwa planu.  

    ![][5]

    Jeśli chcesz użyć aplikacji usługi plan, który już istnieje w środowisku usługi aplikacji, wybierz ten plan. Jeśli chcesz utworzyć nowy plan usług aplikacji, zobacz następną sekcję tego samouczka, [Utwórz plan usług aplikacji w środowisku usługi aplikacji](#createplan).

5. Wprowadź nazwę dla aplikacji sieci web, a następnie kliknij przycisk **Utwórz**. 

    Jeśli usługi ASE korzysta z zewnętrznego VIP jest adres URL aplikacji w ASE: [*Nazwa witryny*]. [*Nazwa aplikacji środowiska usługi*]. p.azurewebsites.net zamiast [*Nazwa witryny*]. azurewebsites.net
    
    Jeśli usługi ASE korzysta wewnętrznych VIP, a następnie adres URL aplikacji, w tym jest ASE: [*Nazwa witryny*]. [*poddomeny określone podczas tworzenia ASE*]   
    Po wybraniu podczas tworzenia ASE poddomeny aktualizowanie pod **nazwą** zostanie wyświetlona strona ASP

## <a name="createplan"></a>Tworzenie planu aplikacji usługi

Podczas tworzenia planu usługi aplikacji w środowisku usługi aplikacji, pracownik dostępne są następujące opcje różnych są nie udostępnionego pracowników w ASE.  Pracowników, których należy użyć są tymi, które zostały przydzielone do ASE przez administratora.  Oznacza to, że aby utworzyć nowy plan, trzeba mieć więcej pracowników przydzielone do roboczych ASE niż całkowita liczba wystąpień wszystkich planów już w tej puli pracownika.  Jeśli nie masz za mało pracowników pracownik ASE puli tworzenie planu, musisz pracować z administratora ASE, aby dodać je uzyskać.

Inna różnica z planów aplikacji usług obsługiwanych przez środowisku usługi aplikacji jest Brak ceny zaznaczenia.  Jeśli masz środowisku usługi aplikacji dokonujesz płatności dla zasobów obliczeń używanych przez system i nie mają dodane opłaty dla planów w tym środowisku.  Podczas tworzenia planu usługi aplikacji zwykle Wybierz cennik plan, który określa rozliczenia.  Środowisko usługi aplikacji jest zasadniczo prywatne lokalizacji miejsce, w którym można utworzyć zawartość.  Płatność dla środowiska i nie chcesz, aby udostępnić zawartość.

Poniższe instrukcje pokazująca, jak utworzyć plan usług aplikacji, gdy tworzysz aplikację sieci web zgodnie z opisem w poprzedniej sekcji samouczka.

1. Kliknij przycisk **Utwórz nowy** plan zaznaczenie interfejsu użytkownika i podaj nazwę dla Twojego planu, tak jak zwykle poza ASE.

2. Wybierz pozycję ASE, który ma być używany z selektora Twojej lokalizacji.

    Ponieważ środowisku usługi aplikacji jest zasadniczo lokalizację prywatnego wdrożenia, jest wyświetlany w obszarze lokalizacja. 

    ![][2]

    Po zaznaczeniu ASE w selektorze lokalizacji Tworzenie planu aplikacji usługi interfejsu użytkownika aktualizacji.  Lokalizację teraz zawiera nazwę systemu ASE i regionu, znajduje się w, a cennik selektora plan jest zastępowany selektora puli pracownika.  

    ![][3]

### <a name="selecting-a-worker-pool"></a>Wybieranie zestawu pracownika

Zwykle w usłudze Azure aplikacji i spoza niej środowisku usługi aplikacji istnieje 3 rozmiary obliczeń, które są dostępne w przypadku zaznaczenia planu dedykowane ceny.  W podobny sposób dla ASE można zdefiniować maksymalnie 3 pul pracowników i określić rozmiar obliczeń, używanym do tej puli pracownika.  Co to znaczy dzierżaw ASE jest to zamiast tego wybrać cennik plan o rozmiarze obliczeń dla Twojego planu usługi aplikacji, wybierz pozycję tak zwany *roboczych*.  

Zaznaczenie puli pracownik interfejsu użytkownika pokazuje rozmiar obliczeń na potrzeby tej puli pracownika pod nazwą.  Ilości dostępnej odwołuje się do wyliczenia ile wystąpień są dostępne do użycia w tej puli.  Całkowity puli może w rzeczywistości obejmować wystąpień więcej niż, ale ta wartość odwołuje się do wystarczy ile nie są używane.  Jeśli chcesz dostosować środowiska usługi aplikacji, aby dodać więcej obliczyć zasobów zobacz [Konfigurowanie środowiska usługi aplikacji](app-service-web-configure-an-app-service-environment.md).

![][4]

W tym przykładzie widać dostępne tylko dwa pul pracownika. Jest tak, ponieważ ASE administrator przydzielonego hosts tylko w tych dwóch pracownik pule.  Trzecia będzie wyświetlany w przypadku maszyny wirtualne przydzielone do niego.  

## <a name="after-web-app-creation"></a>Po utworzeniu aplikacji sieci web

Istnieje kilka zagadnienia związane z uruchamiania aplikacji sieci web i zarządzanie planów aplikacji usług w ASE, które należy wziąć pod uwagę.  

Jak wspomniano wcześniej, właściciel ASE jest odpowiedzialny za rozmiar systemu i w wyniku również są odpowiedzialne za zapewnienie, że jest wystarczającej do obsługi odpowiednie plany aplikacji usługi. W przypadku nie dostępne pracowników nie można utworzyć planu obsługi aplikacji.  Jest to także wartość PRAWDA Skalowanie wewnętrzne aplikacji sieci web.  Jeśli potrzebujesz więcej wystąpień będzie mieć uzyskanie administratora środowisko usługi aplikacji, aby dodać więcej pracowników.

Po utworzeniu aplikacji sieci web, a plan usług aplikacji jest dobrym pomysłem jest jego rozbudowy.  W ASE możesz zawsze muszą mieć co najmniej 2 wystąpienia planu usług aplikacji, aby zapewnić odporność na uszkodzenia dla aplikacji.  Skalowanie plan usług aplikacji w ASE jest taka sama, jak normalne za pośrednictwem plan usług aplikacji interfejsu użytkownika.  Aby uzyskać więcej informacji na temat skalowania, [sposobu skalowania aplikacji sieci web w środowisku usługi aplikacji](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
