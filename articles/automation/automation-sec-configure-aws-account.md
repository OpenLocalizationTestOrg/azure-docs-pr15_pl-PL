<properties
   pageTitle="Konfigurowanie uwierzytelniania z usługi sieci Web Amazon | Microsoft Azure"
   description="W tym artykule opisano, jak tworzyć i sprawdź poprawność AWS poświadczenia dla runbooks w automatyzacji Azure zarządzania zasobami AWS."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="Uwierzytelnianie awS Konfigurowanie aws"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>

# <a name="authenticate-runbooks-with-amazon-web-services"></a>Uwierzytelnianie Runbooks z usługami sieci Web Amazon
Automatyzowanie typowych zadań z zasobami w usługach sieci Web Amazon (AWS) można wykonywać przy użyciu runbooks automatyzacji platformy Azure.  Można zautomatyzować wiele zadań w AWS przy użyciu automatyzacji runbooks tak, jak można to zrobić zasobów w Azure.  Wszystkie, które są wymagane są dwie czynności:

* Subskrypcja AWS i zestaw poświadczeń.  W szczególności usługi AWS klawisz dostępu i klucz tajny.  Aby uzyskać więcej informacji zapoznaj się z tego artykułu [Przy użyciu poświadczeń AWS](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Azure subskrypcji, a konto automatyzacji.  Aby uzyskać więcej informacji o konfigurowaniu konto Azure automatyzacji Przejrzyj artykuł [Konfigurowanie Azure konta Uruchom jako](../automation/automation-sec-configure-azure-runas-account.md).  

Typ poświadczeń uwierzytelniania AWS, musisz określić zestaw poświadczeń AWS do uwierzytelnienia usługi runbooks uruchamiania z automatyzacji Azure. Jeśli masz już konto automatyzacji utworzone i chcesz użyć do uwierzytelniania AWS, możesz wykonać czynności opisane w następnej sekcji.  Jeśli chcesz dedykowane konta runbooks wskazuje AWS zasobów, należy najpierw utworzyć nowe [konta automatyzacji Uruchom jako](../automation/automation-sec-configure-azure-runas-account.md) (Pomiń opcję, aby utworzyć wystawcy usługi) i wykonaj następujące czynności.

## <a name="configure-automation-account"></a>Konfigurowanie konta automatyzacji
Azure automatyzacji można komunikować się z AWS zostanie najpierw należy pobrać poświadczenia AWS i przechowywać je jako środkami automatyzacji Azure.  Wykonaj następujące czynności opisane w dokumencie AWS [Zarządzanie klawiszy dostępu do konta AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) tworzenie klawisz dostępu i kopiowanie **Identyfikator klucza dostępu** i **Klawisz dostępu hasło** (opcjonalnie pobierania pliku klucza schować gdzieś bezpieczne).

Po utworzeniu i skopiowany kluczy zabezpieczeń AWS, może być konieczne Utwórz trwały poświadczeń przy użyciu konta automatyzacji Azure bezpiecznie ich przechowywania i ich wyszukiwanie przy użyciu usługi runbooks.  Postępuj zgodnie z instrukcjami w sekcji, **Aby utworzyć nowy poświadczeń** w artykule [poświadczeń środkami automatyzacji Azure](../automation/automation-certificates.md/###To create a new credential with the Azure portal) i wprowadź następujące informacje:

1. W polu **Nazwa** wprowadź odpowiednią wartość zgodnie z standardami nazewnictwa lub **AWScred** .  
2. W polu **Nazwa użytkownika** wpisz swój **Identyfikator programu Access** i **Klawisz dostępu hasło** w polu **hasło** i **Potwierdź hasło** .   

## <a name="next-steps"></a>Następne kroki

- Artykuł rozwiązanie [Automatyzowanie wdrażania maszyn wirtualnych w usługach sieci Web Amazon](../automation/automation-scenario-aws-deployment.md) , aby dowiedzieć się, jak utworzyć runbooks do automatyzowania zadań w AWS monitującymi.
