<properties 
    pageTitle="Sposobu skalowania aplikacji w środowisku usługi aplikacji" 
    description="Skalowanie aplikacji w środowisku usługi aplikacji" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="ccompy"/>

# <a name="scaling-apps-in-an-app-service-environment"></a>Skalowanie aplikacji w środowisku usługi aplikacji #

W usłudze Azure aplikacji istnieją zwykle trzy czynności, które można skalować:

- Cennik planu
- rozmiar pracownika 
- Liczba wystąpień.

W ASE istnieje bez konieczności wybierz lub zmień cennik planu.  Za pomocą funkcji jest już na Premium ceny poziom możliwości.  

W odniesieniu do rozmiarów pracownik administrator ASE można przypisać rozmiar zasobu obliczeń może być używany dla każdej puli pracownika.  Oznacza to, że masz pracownik puli 1 z zasobami obliczeń P4 i pracownik puli 2 z P1 obliczyć zasobów, jeśli to konieczne.  Nie mają kolejność rozmiar.  Dla szczegóły wokół rozmiary i ich cennik zobacz dokument tutaj [Azure aplikacji usługi ceny][AppServicePricing].  To pozostawia opcji skalowania dla aplikacji sieci web i plany usługi aplikacji w środowisku usługi aplikacji, należy:

- Zaznaczenie puli pracownika
- Liczba wystąpień

Zmienianie oba elementy odbywa się odpowiednie elementy interfejsu użytkownika wyświetlane dla swojego ASE hostowanej plany usługi aplikacji.  

![][1]

Strona ASP poza liczby zasobów dostępnych obliczeń w puli pracownika, która jest strona ASP nie rozbudowy.  Jeśli potrzebujesz obliczyć zasoby z tej puli pracownik jest potrzebne do uzyskania z administratorem ASE, aby je dodać.  Dla informacji dotyczących ponowne konfigurowanie programu ASE odczytywanie informacji o tutaj: [jak skonfigurować środowisko aplikacji usługi][HowtoConfigureASE].  Można także korzystać z funkcji autoscale ASE, aby dodać zdolności na podstawie harmonogramu lub metryki.  Aby uzyskać więcej informacji na temat konfigurowania autoscale dla ASE środowisko sam Dowiedz się, [jak skonfigurować autoscale w środowisku usługi aplikacji][ASEAutoscale].

Możesz utworzyć aplikację wiele planów usługi przy użyciu obliczeń zasoby z zestawów innego pracownika lub można użyć tej samej puli pracownika.  Na przykład jeśli masz (10) zasobów dostępnych obliczeń w pracownik puli 1, można utworzyć jeden plan usług aplikacji przy użyciu zasobów obliczeń (6) i planowanie drugiej aplikacji usługi używającej (4) obliczyć zasobów.

### <a name="scaling-the-number-of-instances"></a>Skalowanie liczbę wystąpień ###

Podczas tworzenia aplikacji sieci web w środowisku usługi aplikacji zaczyna się od 1 wystąpienie.  Można następnie skalować się dodatkowe wystąpienia dostarczać obliczeń dodatkowe zasoby dla aplikacji.   

Jeśli do ASE ma za mało wydajność pracy jest bardzo proste.  Przejdź do aplikacji usługi planu zawierający witryny, które chcesz rozbudowy i wybierz skalę.  Spowoduje to otwarcie interfejsu użytkownika, w którym można ręcznie ustaw odpowiednią skalę dla strona ASP lub skonfigurować reguły autoscale strona ASP.  Ręczne skali aplikacji po prostu ustaw ***Skalowanie przez*** ******liczy wystąpienia, które można wprowadzić ręcznie.  W tym miejscu przeciągnij suwak, aby wymagana ilość lub wprowadzić ją w polu obok suwaka.  

![][2] 

Zasady autoscale ASP w ASE działa tak samo, jak zwykle.  Można wybrać ***Wartość procentową Procesora*** w obszarze ***Skala przez*** i utworzyć reguły autoscale dla strona ASP na podstawie procentu Procesora lub można tworzyć bardziej złożone reguły przy użyciu ***reguł harmonogram i wydajności***.  Aby wyświetlić dokładniejszy szczegółowe informacje na temat konfigurowania autoscale za pomocą przewodnika tutaj [skalowanie aplikacji w usłudze Azure aplikacji][AppScale]. 


### <a name="worker-pool-selection"></a>Zaznaczenie puli pracownika ###

Jak wspomniano wcześniej, zaznaczenie puli pracownik jest dostępny ASP interfejsu użytkownika.  Otwieranie karta dla ASP, który chcesz skalować i wybierz pozycję roboczych.  Zostanie wyświetlona wszystkie pule pracownika, które zostały skonfigurowane w środowisku usługi aplikacji.  Jeśli masz tylko jedną roboczych następnie wyświetlany tylko jednej puli na liście.  Aby zmienić puli pracownik, strona ASP znajduje się w, po prostu wybierz roboczych ma planu o usługi aplikacji przejdź do.  

![][3]

Przed przeniesieniem strona ASP z puli jednego pracownika do innego należy upewnij się, że użytkownik ma odpowiednie możliwości strona ASP.  Na liście pul pracownik nie tylko znajduje się nazwa puli pracownika, ale można też wyświetlić liczbę pracowników są dostępne w tej puli pracownika.  Upewnij się, że są za mało wystąpienia dostępne zawierać Planowanie aplikacji usługi.  Jeśli potrzebujesz więcej obliczyć zasobów w puli pracownik, który chcesz przenieść, następnie przejść z administratorem ASE, aby je dodać.  

> [AZURE.NOTE] Przeniesienie ASP z jednego pracownika puli spowoduje, że zimnej powoduje uruchomienie aplikacji w ASP.  Może to powodować żądania uruchomienia powoli, jak aplikacja jest zimnej uruchomieniem nowych zasobów obliczeń.  Rozpoczęcie zimnej można uniknąć przy użyciu [aplikacji ciepłych się funkcja] [ AppWarmup] w usłudze Azure aplikacji.  Moduł inicjowania aplikacji opisane w artykule działa także w przypadku uruchomienia zimnej ponieważ proces inicjowania również jest wywoływana, gdy aplikacje są zimnej uruchomieniem nowych zasobów obliczeń. 

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć pracę ze środowiskami usługi aplikacji, zobacz [Jak do tworzenia aplikacji środowisko usługi][HowtoCreateASE]

Aby uzyskać więcej informacji na temat platformy Azure aplikacji usługi zobacz [Azure aplikacji usługi][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
