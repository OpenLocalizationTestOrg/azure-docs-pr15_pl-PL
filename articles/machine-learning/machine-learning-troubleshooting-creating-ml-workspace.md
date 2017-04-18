<properties
    pageTitle="Rozwiązywanie problemów z: Tworzenie i łączenie do obszaru roboczego maszynowego uczenia | Microsoft Azure"
    description="Rozwiązania typowych problemów dotyczących tworzenia i łączenia się z obszarem roboczym nauki komputera Azure"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="garye"/>


# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Przewodnik rozwiązywania problemów: tworzenie i łączenie się z obszarem roboczym nauki komputera

Ten przewodnik zawiera rozwiązań dla niektórych często napotkał problemy podczas konfigurowania obszarów roboczych Azure maszynowego uczenia.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Właściciel obszaru roboczego

Po utworzeniu nowego obszaru roboczego maszynowego uczenia identyfikator wprowadź w polu właściciel obszaru roboczego musi być prawidłowym kontem Microsoft (wcześniej Windows Live ID), na przykład john-contoso@live.com lub john-contoso@hotmail.com. Nie może być konta firmy Microsoft, takiego jak firmowej poczty e-mail. Aby utworzyć bezpłatne konto Microsoft, przejdź do [www.live.com](http://www.live.com).

Należy zauważyć, że konto, którego użyto do zalogowania się do portalu Azure do tworzenie obszaru roboczego nie automatycznie uprawnień do *otwarcia* tego obszaru roboczego, o ile nie wybierzesz tego konta jako właściciel. Aby otworzyć obszar roboczy w Studio nauki komputera, możesz należy się zalogować do Account Microsoft, które zostało zdefiniowane jako właściciel obszaru roboczego lub musisz otrzymać zaproszenie do obszaru roboczego od właściciela. Z poziomu portalu klasyczny Azure można jednak *Zarządzanie* obszaru roboczego, który umożliwia zmienianie właściciela i konfigurowanie dostępu.

Aby uzyskać więcej informacji o zarządzaniu obszaru roboczego zobacz [Zarządzanie z obszarem roboczym Azure maszynowego uczenia].

[Zarządzanie Azure maszynowego uczenia obszaru roboczego]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Dozwolone regionów

Nauka maszynowego jest obecnie dostępna w ograniczoną liczbą regionów. Jeśli Twoja subskrypcja nie zawiera jedną z następujących regionów, może zostać wyświetlony komunikat o błędzie, "Jest nie w regionach dozwolone."

Aby zażądać od dodania regionu do subskrypcji, wybierz **Kontakt z pomocą techniczną firmy Microsoft** z portalem klasyczny Azure, wybierz pozycję **rozliczenia** jako typ problemu i postępuj zgodnie z instrukcjami, aby przesłać żądanie.

![Kontakt z pomocą techniczną firmy Microsoft][screen1]

## <a name="storage-account"></a>Konto miejsca do magazynowania

Usługa maszynowego uczenia wymaga konta miejsca do magazynowania do przechowywania danych. Można użyć istniejącego konta miejsca do magazynowania lub można utworzyć nowe konto miejsca do magazynowania, podczas tworzenia nowego obszaru roboczego nauki komputera (Jeśli masz przydziału, aby utworzyć nowe konto miejsca do magazynowania).

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Tworzenie obszaru roboczego][screen2]

Po utworzeniu nowego obszaru roboczego nauki komputera, możesz się zalogować do komputera nauki Studio za pomocą konta Microsoft, którego określony jako właściciel obszaru roboczego. Jeśli wyświetlony komunikat o błędzie "Obszar roboczy nie znaleziono" (podobne do następujących zrzut ekranu), aby usunąć pliki cookie przeglądarki wykonaj następujące czynności.

![Nie można odnaleźć obszaru roboczego][screen3]

**Aby usunąć pliki cookie przeglądarki**

Jeśli używasz programu Internet Explorer, kliknij przycisk **Narzędzia** w prawym górnym rogu i wybierz polecenie **Opcje internetowe**.  

![Opcje internetowe][screen4]

Na karcie **Ogólne** kliknij przycisk **Usuń...**

![Na karcie Ogólne][screen5]

W oknie dialogowym **Usuwanie historii przeglądania** upewnij się, że jest zaznaczona pozycja **pliki cookie i dane witryn sieci Web** , a następnie kliknij przycisk **Usuń**.

![Usuń pliki cookie][screen6]

Po usunięciu plików cookie, uruchom ponownie przeglądarkę, a następnie przejdź do strony [Microsoft Azure maszynowego uczenia](https://studio.azureml.net) . Gdy zostanie wyświetlony monit o podanie nazwy użytkownika i hasła, wprowadź tego samego konta Microsoft, podanych jako właściciel obszaru roboczego.

Naszej sieci jest zapewnienie szkoleniowej komputera jako Bezproblemowa, jak to możliwe. Opublikuj problemami i komentarze, na [forum Azure maszynowego uczenia](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) ułatwi będą one lepiej.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
