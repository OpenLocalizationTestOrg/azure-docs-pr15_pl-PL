<properties
   pageTitle="Aby skonfigurować o nazwie poświadczeń uwierzytelniania | Microsoft Azure"
   description="Dowiedz się jak do o podanie poświadczeń tego programu Visual Studio za pomocą uwierzytelniania żądań Azure, aby opublikować aplikację Azure z programu Visual Studio lub monitorowanie istniejące usługi w chmurze. "
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="setting-up-named-authentication-credentials"></a>Definiowanie nazwanych poświadczeń

Aby opublikować aplikację Azure z programu Visual Studio lub monitorowanie istniejące usługi w chmurze, musisz podać poświadczeń używanych przez Visual Studio do uwierzytelniania żądań Azure. Istnieje kilka miejsc w programie Visual Studio, na której możesz się zalogować podania tych poświadczeń. Na przykład Eksploratora serwera można otworzyć menu skrótów dla węzła **Azure** i wybierz polecenie **Połącz Azure**. Po zalogowaniu się informacje o subskrypcji skojarzonego z kontem Azure jest dostępna w programie Visual Studio, a nie ma więcej, należy wykonać.

Narzędzia Azure obsługuje także starsze metodą dostarczania poświadczeń, za pomocą subskrypcji (plik .publishsettings). W tym temacie opisano metodę nadal jest obsługiwana w Azure SDK 2.2.

Następujące elementy są wymagane do uwierzytelniania Azure.

- Identyfikator subskrypcji

- Ważny certyfikat X.509 v3

>[AZURE.NOTE] Długość klucza certyfikatu X.509 v3 musi wynosić co najmniej 2048 bitów. Azure odrzuci dowolny certyfikat, który nie spełnia to wymaganie lub nie jest prawidłowy.

Program Visual Studio używa identyfikator subskrypcji wraz z danych certyfikatu jako poświadczenia. Odpowiednie poświadczenia są zawarte w pliku subskrypcji (plik .publishsettings), który zawiera klucz publiczny certyfikatu. Plik subskrypcji może zawierać poświadczenia dla więcej niż jedną subskrypcję.

Można edytować informacje o subskrypcji w oknie dialogowym **Nowy/edytowanie subskrypcji** , zgodnie z opisem w dalszej części tego tematu.

Jeśli chcesz samodzielnie utworzyć certyfikat, możesz zapoznać się z instrukcjami w [Tworzenie i przekazywanie certyfikatu zarządzania Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) , a następnie ręcznie przekazać certyfikatu do [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.NOTE] Te poświadczenia, które Visual Studio wymaga, aby zarządzać usługami w chmurze nie są te same poświadczenia, które są wymagane do uwierzytelnienia żądania przed usług Azure magazynu.

## <a name="modify-or-export-authentication-credentials-in-visual-studio"></a>Modyfikowanie lub eksportowanie poświadczeń uwierzytelniania w programie Visual Studio

Możesz również skonfigurować, modyfikowanie lub eksportowanie poświadczeń uwierzytelniania w oknie dialogowym **Nowej subskrypcji** , która jest wyświetlana, jeśli wykonaj jedną z następujących czynności:

- W **Eksploratorze serwera**Otwórz menu skrótów dla węzła **Azure** , wybierz pozycję **Zarządzaj subskrypcjami**, wybierz kartę **Certyfikaty** i wybierz przycisk **Nowy** lub **Edytuj** .

- Podczas publikowania usługi w chmurze Azure z kreatora **Publikowania aplikacji Azure** , wybierz pozycję **Zarządzaj** na liście **Wybierz subskrypcję** , a następnie wybierz kartę certyfikaty, a następnie wybierz przycisk **Nowy** lub **edytować** .

W poniższej procedurze przyjęto, że jest otwarte okno dialogowe **Nowej subskrypcji** .

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Aby skonfigurować poświadczeń uwierzytelniania w programie Visual Studio

1. **Wybierz istniejący certyfikat** dla listy uwierzytelniania wybierz certyfikat.

1. Wybierz przycisk **skopiować pełną ścieżkę** . Ścieżka certyfikatu (plik cer) jest kopiowana do Schowka.

    >[AZURE.IMPORTANT] Aby opublikować Azure aplikacji z programu Visual Studio, możesz przekazać ten certyfikat do [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Aby przekazać certyfikatu do [Azure klasyczny portalu](http://go.microsoft.com/fwlink/?LinkID=213885):

    1. Wybierz link Azure Portal.

         Zostanie otwarte okno [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885) .

    1. Zaloguj się do [portalu klasyczny Azure](http://go.microsoft.com/fwlink/?LinkID=213885), a następnie wybierz przycisk **Usług w chmurze** .

    1. Wybierz pozycję Usługa w chmurze, interesującą Cię.

        Zostanie wyświetlona strona usługi.

    1. Na karcie **Certyfikaty** wybierz przycisk **Przekaż** .

    1. Wklej pełną ścieżkę pliku cer, która została właśnie utworzona, a następnie wprowadź hasło, które zostanie określone.
