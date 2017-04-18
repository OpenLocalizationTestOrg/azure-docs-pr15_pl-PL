<properties
    pageTitle="Usługa Azure punkty końcowe"
    description="W tym artykule opisano ustawienia punktu końcowego usługi Azure w zestaw narzędzi Azure dla Zaćmienie."
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
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Usługa Azure punkty końcowe #

Punkty końcowe usługi Azure Określ, czy aplikacja jest używany i zarządzane przez globalnej platformy Azure, Azure obsługiwana przez firmę 21Vianet w Chinach lub prywatną platformy Azure. Okno dialogowe **Punkty końcowe usługi** umożliwia Określ, które usługa punkty końcowe, którego chcesz użyć. Aby otworzyć okno dialogowe **Usługi punkty końcowe** w Zaćmienie, kliknij menu **okno**, kliknij polecenie **Preferencje**, rozwiń **Azure**, a następnie kliknij **Punkty końcowe usługi**. Ustawienie **Ustaw aktywne** pole określa, które punkty końcowe usługi Azure będzie używana dla Azure projektów w bieżącym obszaru roboczego.

Poniżej przedstawiono okno dialogowe **Punkty końcowe usługi** .

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>Aby ustawić punkty końcowe usługi ##

W oknie dialogowym **Usługi punkty końcowe** wykonaj jedną z następujących czynności:

* Jeśli chcesz używać globalnej platformy Azure, z listy rozwijanej **Ustaw aktywne** zaznacz **windowsazure.com** , a następnie kliknij **przycisk OK**.
* Jeśli chcesz używać Azure obsługiwana przez firmę 21Vianet w Chinach, z listy rozwijanej **Ustaw aktywne** zaznacz **windowsazure.cn (Chiny)** , a następnie kliknij **przycisk OK**.
* Jeśli chcesz użyć prywatnej platformy Azure:
    1. Kliknij przycisk **Edytuj**.
    2. Zostanie otwarte okno dialogowe, informujące, że zostanie zamknięte okno dialogowe **Usługi punkty końcowe** , a zostanie otwarty plik zestawy preferencji. Kliknij **przycisk OK**.
    3. W pliku preferencesets.xml, Utwórz nową `preferenceset` element. Dla tego nowego elementu, Utwórz `name`, `blob`, `management`, `portalURL` i `publishsettings` atrybuty i dodawanie wartości dla nich, które odpowiadają do posiadanej platformy Azure prywatne. Można użyć wartości dla istniejących `preferenceset` elementy jako szablonów. **Uwaga**: wartość służąca do `blob` atrybutu musi zawierać tekst "blob" w adresie URL.
    4. Zapisz i zamknij preferencesets.xml.
    5. Ponownie otwórz okno dialogowe **Punkty końcowe usługi** .
    6. Z listy rozwijanej **Ustaw aktywnej** wybierz zestaw aktywnej utworzone przez Ciebie, a następnie kliknij **przycisk OK**.
    7. Po utworzeniu prywatne platformy Azure `preferenceset` element, można zmienić wartości do niego przypisana, klikając przycisk **Edytuj** w oknie dialogowym **Punktu końcowego usługi** . Możesz również utworzyć wiele prywatnych platformy Azure `preferenceset` elementów, jeśli jest to wymagane.

## <a name="see-also"></a>Zobacz też ##

[Azure zestaw narzędzi dla programu Eclipse][]

[Instalowanie narzędzi Azure dla programu Eclipse][] 

[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie][]

Aby uzyskać więcej informacji na temat Azure za pomocą języka Java zobacz [Centrum deweloperów języka Java Azure][].

<!-- URL List -->

[Centrum deweloperów języka Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure zestaw narzędzi dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalowanie narzędzi Azure dla programu Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png
