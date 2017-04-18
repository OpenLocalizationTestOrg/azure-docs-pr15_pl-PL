<properties
    pageTitle="Tworzenie obrazu Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się więcej o opcje dostępne w przypadku tworzenia obrazów na potrzeby Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="create-an-azure-remoteapp-image"></a>Tworzenie obrazu Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Funkcja RemoteApp Azure używa obrazów do przechowywania aplikacje, które udostępniasz innym użytkownikom. (Możemy wykonać obrazu i za jej pomocą tworzyć maszyny wirtualne — to dostępu użytkowników po zapisaniu do Azure RemoteApp) Aby utworzyć zbiór Azure RemoteApp z wybraną aplikacji, czy jest chmurze lub hybrydowymi połączeniami, rozpoczyna się od utworzenia obrazu z tych zainstalowane aplikacje. Następnie utwórz zbioru, która korzysta z tym obrazem, przypisywanie użytkowników do kolekcji i publikowanie aplikacji dla tych użytkowników.

Istnieje kilka opcji do tworzenia i używania obrazów. Podstawowe [wymagania](remoteapp-imagereqs.md) obrazu jest uruchamianie systemu Windows Server 2012 R2 i masz zainstalowaną rolą hosta zdalnej sesji pulpitu (RDSH). Jak uzyskać którego jest miejsce, w którym interesujący rzeczy.

Jeśli chodzi o obrazy są się następujące opcje:

- Można importować i użyć [obrazu według Azure maszyn wirtualnych](remoteapp-image-on-azurevm.md). Jest to dobre dla aplikacji z LOB, które wymagają ustawień niestandardowych. Obraz pracować aplikacji można dostosować.
- Możesz [tworzyć i Przekaż obraz niestandardowy](remoteapp-create-custom-image.md). To jest zalecana, jeśli masz już na obraz, którego możesz używać dla lokalnego wdrożenia usługi pulpitu zdalnego.
- Można użyć jednego z [obrazów szablonów](remoteapp-images.md) dostępnych w ramach subskrypcji RemoteApp. Te obrazy są tworzone i obsługiwane przez zespół RemoteApp i zawierają niektóre aplikacje standardowy (na przykład pakietu Office), które można udostępnić użytkownikom. Należy zauważyć, że tylko Office 365 Pro Plus obrazu może być używany w ustawieniach produkcji.

Niezależnie od tego, gdzie znaleźć obrazu lub sposób tworzenia warto przeprowadzić upewnij się, że wiesz, [wymagania dotyczące aplikacji](remoteapp-appreqs.md) , aby upewnić się, że aplikacji działa również w RemoteApp. Następnie następnym krokiem jest utworzenie w [chmurze](remoteapp-create-cloud-deployment.md) lub [hybrydowymi połączeniami](remoteapp-create-hybrid-deployment.md) zbiorze.
