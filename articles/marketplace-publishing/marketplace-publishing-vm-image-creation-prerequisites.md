<properties
   pageTitle="Techniczne wymagania wstępne dotyczące tworzenia obrazu maszyn wirtualnych dla usługi Azure Marketplace | Microsoft Azure"
   description="Opis wymagań dotyczących tworzenia i wdrażania obrazu maszyn wirtualnych usługi Azure Marketplace przez inne osoby do zakupu."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="04/29/2016"
  ms.author="hascipio; v-divte"/>

# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Techniczne wymagania wstępne dotyczące tworzenia obrazu maszyn wirtualnych dla usługi Azure Marketplace
Przeczytaj proces dokładnie przed rozpoczęciem i opis, gdzie i dlaczego jest wykonywane każdego kroku. Ile to możliwe, możesz należy przygotowywanie informacji o firmie i innych danych, Pobierz niezbędne narzędzia i/lub tworzenia składników techniczne przed rozpoczęciem procesu tworzenia oferty. Te elementy powinny być Wyczyść z przeglądania w tym artykule.  

## <a name="download-needed-tools--applications"></a>Pobierz narzędzia potrzebne & aplikacji
Należy dysponować następującymi elementami gotowe przed rozpoczęciem procesu:

- W zależności od systemu operacyjnego kierowanie, należy zainstalować [poleceń cmdlet środowiska PowerShell Azure](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) lub [Linux interfejs wiersza polecenia narzędzia](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) ze strony [Pobierania Azure](https://azure.microsoft.com/downloads/) .
- Zainstaluj Eksplorator magazynu Azure w witrynie CodePlex.
- Pobieranie i instalowanie narzędzia Test certyfikacji dla Azure certyfikowane:
  - [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Potrzebujesz komputerze z systemem Windows, aby uruchomić narzędzie certyfikacji. Jeśli nie masz komputerze z systemem Windows dostępne, może zostać uruchomiony za pomocą platformy Azure maszyn wirtualnych systemu Windows.

## <a name="platforms-supported"></a>Platformy obsługiwane
Można tworzyć oparte na platformie Azure maszyny wirtualne w systemach Windows i Linux. Niektóre elementy procesu publikowania — takie jak tworzenie zgodnego z programem Azure wirtualnego dysku twardego (wirtualnego dysku twardego) — używanie różnych narzędzi i czynności w zależności od systemu operacyjnego używasz:  

- Jeśli używasz Linux można znaleźć w sekcji "Tworzenie wirtualnego dysku twardego zgodnego z programem Azure (z systemem Linux)" [Podręcznik publikowania maszyn wirtualnych obrazu](marketplace-publishing-vm-image-creation.md).
- Jeśli korzystasz z systemu Windows, można znaleźć w sekcji "Tworzenie wirtualnego dysku twardego zgodnego z programem Azure (opartych na systemie Windows)" [Podręcznik publikowania maszyn wirtualnych obrazu](marketplace-publishing-vm-image-creation.md).

> [AZURE.NOTE] Potrzebny jest dostęp do komputera systemu Windows w celu:
- Uruchom narzędzie do sprawdzania poprawności certyfikacji.
- Tworzenie udostępnionych wirtualnego dysku twardego dostępu podpisu adresu URL przesyłania certyfikacji wirtualnego dysku twardego.

## <a name="develop-your-vhd"></a>Można opracowywać do wirtualnego dysku twardego
Istnieje możliwość utworzenia Azure wirtualnych dysków twardych w chmurze lub lokalnego:

- Oparte na chmurze rozwoju oznacza, że wszystkie kroki rozwoju są wykonywane zdalnie na pobytu Azure wirtualnego dysku twardego.
- Rozwoju lokalnego wymaga pobierane wirtualnego dysku twardego i opracowywania go przy użyciu infrastruktury lokalnego. Mimo że jest to możliwe, zaleca się jego. Należy zauważyć, że opracowywanie dla systemu Windows lub SQL lokalnego wymaga odpowiednich lokalnego kluczy licencji. Nie można dołączyć lub zainstalować programu SQL Server po utworzeniu maszyny. Należy również utworzyć Twoją ofertę na obrazie zatwierdzonych SQL z portalu Azure. Jeśli zdecydujesz się na opracowywanie lokalnych, należy wykonać kilka czynności, inaczej niż jeśli zostały opracowywania w chmurze. Informacje można znaleźć w sekcji [Tworzenie obrazu maszyn wirtualnych lokalnego](marketplace-publishing-vm-image-creation-on-premise.md).

## <a name="next-steps"></a>Następne kroki
Teraz, gdy przejrzeć wymagania wstępne i niezbędne zadania wykonane jako ukończone, można przechodzić do przodu do tworzenia Twoją ofertę obraz maszyn wirtualnych wyszczególnione w [publikacji Przewodnik obraz maszyn wirtualnych](marketplace-publishing-vm-image-creation.md).

## <a name="see-also"></a>Zobacz też
- [Wprowadzenie: jak publikować ofertę Azure Marketplace](marketplace-publishing-getting-started.md)
- [Tworzenie wirtualnych komputera z systemem Windows w portal Azure preview](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
