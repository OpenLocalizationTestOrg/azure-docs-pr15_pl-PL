<properties
   pageTitle="Przewodnik tworzenia szablonu rozwiązanie dla Marketplace | Microsoft Azure"
   description="Szczegółowe informacje, jak tworzyć, certyfikowanie i wdrażanie szablonu maszyn wirtualnych wielokrotne obraz rozwiązanie do zakupu na Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="07/27/2016"
      ms.author="hascipio; v-divte" />

# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Przewodnik, aby utworzyć szablon rozwiązania dla usługi Azure Marketplace
Po wykonaniu kroku 1, [Tworzenie konta i rejestracji][link-acct-creation], możemy wskazówki na temat tworzenia szablonu zgodnego z programem Azure rozwiązanie [techniczne wymagania wstępne dotyczące tworzenia szablonów rozwiązanie](marketplace-publishing-solution-template-creation-prerequisites.md). Teraz możemy przeprowadzi Cię przez etapy tworzenia szablonu rozwiązanie dla wielu maszyny wirtualne w [Portal publikowania] [ link-pubportal] dla usługi Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Tworzenie Twoją ofertę szablonu rozwiązanie w Portal publikowania
Przejdź do [https://publish.windowsazure.com](http://publish.windowsazure.com). Podczas logowania się po raz pierwszy do [Portal publikowania](https://publish.windowsazure.com/)za pomocą tego samego konta, z którym została zarejestrowana w firmie sprzedawcy profilu. Później możesz dodać dowolnego pracownika firmy jako administratorów współpracujących w portalu publikowania.

### <a name="1-select-solution-templates"></a>1. Wybierz pozycję "Szablony rozwiązanie"

  ![Rysunek][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Utwórz nowy szablon rozwiązanie

  ![Rysunek][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. początkowych topologii
Szablon rozwiązanie jest "parent" do wszystkich jej topologii. Wiele topologii można zdefiniować w jeden szablon oferty i rozwiązanie. Jeśli oferta zostanie przypisany do przemieszczania, zostanie przypisany ze wszystkimi jej topologii. Wykonaj poniższe czynności, aby zdefiniować Twoją ofertę:     

- Tworzenie topologii: "Identyfikator topologii" jest zazwyczaj nazwą topologii szablonu rozwiązanie. Identyfikator topologii jest używane w adresie URL, tak jak pokazano poniżej:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace}-{OfferIdentifier} {TopologyIdentifier}

  Azure Portal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}

- Dodawanie nowej wersji.

### <a name="4-get-your-topology-versions-certified"></a>4. przejść z wersji topologii certyfikowane
Przekazywanie pliku zip zawierający wszystkie pliki wymagane do obsługi administracyjnej tej określonej wersji topologii. Ten plik zip musi zawierać następujące czynności:

- plik *mainTemplate.json* i *createUiDefinition.json* w katalogu głównym.
- Połączone szablony i wszystkie wymagane skryptów.

  > [AZURE.TIP] Podczas pracy deweloperów na temat tworzenia rozwiązanie topologii szablonu i ich certyfikowane, jej działalności, marketingowych i/lub prawne działy firmy mogą pracować nad zawartością marketingowych i prawnych.

## <a name="next-steps"></a>Następne kroki
Teraz, gdy utworzony szablon rozwiązanie i przekazać pliku zip, sprawdź postępuj zgodnie z instrukcjami [Przewodnik po zawartości marketingowych Marketplace](marketplace-publishing-push-to-staging.md) , przed naciśnięcie ofertę do przemieszczania. Aby uzyskać pełny zestaw marketplace publikowanie artykułów, odwiedź stronę [Wprowadzenie: jak publikować ofertę Azure Marketplace](marketplace-publishing-getting-started.md).

Można również w te artykuły pokrewne:

- Obrazów maszyn wirtualnych: [O obrazów maszyn wirtualnych platformy Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
- Rozszerzenia maszyn wirtualnych: [Omówienie rozszerzenia maszyn wirtualnych i agenta maszyn wirtualnych](https://msdn.microsoft.com/library/azure/dn832621.aspx) i [rozszerzenia maszyn wirtualnych Azure i funkcje](https://msdn.microsoft.com/library/azure/dn606311.aspx)
- [Azure ARM szablonów do tworzenia](../resource-group-authoring-templates.md) i [ARM prosty szablon przykłady](https://github.com/rjmax/ArmExamples) Azure Menedżer zasobów:
- Ogranicza konta miejsca do magazynowania: [jak Monitor do ograniczania konta miejsca do magazynowania](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) i [Premium miejsca do magazynowania](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
