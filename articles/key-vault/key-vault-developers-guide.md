<properties
   pageTitle="Klucz — Przewodnik programisty repozytorium | Microsoft Azure"
   description="Deweloperzy mogą używać magazyn kluczy Azure, aby zarządzać klucze kryptograficzne w środowisku Microsoft Azure. "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/03/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-developers-guide"></a>Podręcznik programisty magazyn kluczy Azure
Za pomocą magazynu kluczy, będzie można bezpieczny dostęp do poufnych informacji z aplikacji tak, aby:

- Klucze i hasła będą chronione bez konieczności samodzielnego programowania kodu i będzie łatwo można z nich korzystać z aplikacji.
- Będziesz mógł mieć własnych klientów i zarządzać własne klucze, więc możesz skoncentrować się na dostarczaniu podstawowe funkcje oprogramowania. W ten sposób aplikacje nie będzie właścicielem odpowiedzialności ani potencjalnych zobowiązań dla kluczy dzierżawy klientów i tajemnice.
- Aplikacja może używać klucze podpisywania i szyfrowania jeszcze utrzymuje zarządzania kluczami zewnętrznych z poziomu aplikacji takie, że rozwiązanie jest odpowiedni dla aplikacji, która jest rozproszone geograficznie.

- Wraz z wydaniem września 2016 magazynie kluczy, aplikacje mogą teraz korzystać z magazyn kluczy certyfikatów. Aby uzyskać więcej informacji, zobacz artykuł **o klucze, klucze tajne i certyfikaty** w [POZOSTAŁEJ odniesienia](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Aby uzyskać więcej ogólnych informacji o magazynie kluczy Azure zobacz [Co to jest magazyn kluczy](key-vault-whatis.md).

## <a name="videos"></a>Pliki wideo
Ten film pokazuje, jak utworzyć swój własny magazyn kluczy i jak z niego korzystać z przykładowej aplikacji "Hello magazyn kluczy".

[AZURE.VIDEO azure-key-vault-developer-quick-start]

Łącza do zasobów wymienionych w wideo:
- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Magazyn kluczy Azure przykładowy kod](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

Aby dowiedzieć się więcej można po [Klucz repozytorium blogu](http://aka.ms/kvblog) i uczestniczyć w [Forum Vault klucza](http://aka.ms/kvforum).

## <a name="creating-and-managing-key-vaults"></a>Tworzenie i zarządzanie magazynów kluczy

Przed rozpoczęciem pracy z magazyn kluczy Azure w kodzie, można tworzyć i zarządzać magazynów do odpoczynku, szablony Menedżera zasobów, środowiska PowerShell lub interfejsu wiersza polecenia, jak opisano w następujących artykułach:

- [Tworzenie i zarządzanie magazynów kluczy z resztą](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [Tworzenie i zarządzanie magazynów kluczy przy użyciu programu PowerShell](key-vault-get-started.md)
- [Tworzenie i zarządzanie magazynów kluczy z interfejsu wiersza polecenia](key-vault-manage-with-cli.md)
- [Utwórz magazyn kluczy i Dodaj klucz tajny za pomocą szablonu Menedżera zasobów](../resource-manager-template-keyvault.md)

>[AZURE.NOTE] Działania przeciwko magazynów kluczy są uwierzytelniane za pomocą AAD i autoryzowane za pośrednictwem klucza magazynu własne zasady dostępu, definiowanych dla repozytorium.

## <a name="coding-with-key-vault"></a>Kodowanie z magazyn kluczy

Magazyn kluczy system zarządzania dla programistów składa się z kilka interfejsów, z resztą jako Fundacji, [Klucz repozytorium REST API Reference](https://msdn.microsoft.com/library/azure/dn903609.aspx).

Użytkownik może zastrzeżeniem pomyślnego, wykonaj następujące czynności:

- Zarządzają kluczami szyfrowania przy użyciu [Utwórz](https://msdn.microsoft.com/library/azure/dn903634.aspx), [Import](https://msdn.microsoft.com/library/azure/dn903626.aspx), [aktualizację](https://msdn.microsoft.com/library/azure/dn903616.aspx), [Usuwanie](https://msdn.microsoft.com/library/azure/dn903611.aspx) i innych operacji

- Zarządzanie za pomocą [Get](https://msdn.microsoft.com/library/azure/dn903633.aspx), [aktualizację](https://msdn.microsoft.com/library/azure/dn986818.aspx), [Usuwanie](https://msdn.microsoft.com/library/azure/dn903613.aspx) i innych operacji tajemnice

- Klucze szyfrowania za pomocą [znaku](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[Sprawdź](https://msdn.microsoft.com/library/azure/dn878082.aspx), [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx) i [Szyfruj](https://msdn.microsoft.com/library/azure/dn878060.aspx)/operacji[odszyfrowywania](https://msdn.microsoft.com/library/azure/dn878097.aspx)

Następujące zestawy SDK są dostępne do pracy z magazynu kluczy:

|[![.NET](./media/key-vault-developers-guide/msft.netlogo_purple.png)](https://msdn.microsoft.com/library/mt765854.aspx)|[![Node.js](./media/key-vault-developers-guide/nodejs.png)](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
|[Dokumentację zestawu SDK platformy .NET](https://msdn.microsoft.com/library/mt765854.aspx)|[Dokumentacja pakietu SDK node.js](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)|
|[Pakiet SDK platformy .NET na Nuget](http://www.nuget.org/packages/Microsoft.Azure.KeyVault)|[Pakiet node.js SDK](https://www.npmjs.com/package/azure-keyvault)|

Aby uzyskać więcej informacji dotyczących zestawu SDK dla usługi .NET w wersji 2.x zobacz [informacje o wersji](key-vault-dotnet2api-release-notes.md).

## <a name="example-code"></a>Przykładowy kod
Za pomocą usługi Magazyn kluczy z aplikacjami kompletne przykłady zobacz:

- Przykład usługi Azure w sieci web i aplikacji przykładowej .NET *HelloKeyVault* . [Przykłady kodu magazyn kluczy Azure](http://www.microsoft.com/download/details.aspx?id=45343)
- Samouczek ułatwiające korzystanie z aplikacji sieci web w systemie Azure magazyn kluczy Azure. [Użyj magazyn kluczy Azure z aplikacji sieci Web](key-vault-use-from-web-application.md)

## <a name="how-tos"></a>Porady dotyczące

Następujące artykuły i scenariusze zawierają szczegółowe wytyczne zadań do pracy z magazyn kluczy Azure:

- [Zmień identyfikator dzierżawcy magazyn kluczy po przenoszenia subskrypcji](key-vault-subscription-move-fix.md) - po przeniesieniu usługi z lokatora A do dzierżawcy B, z istniejących magazynów kluczy są niedostępne przez podmioty (Użytkownicy i aplikacje) w dzierżawie poprawkę B. to korzystanie z przewodnika.
- [Uzyskiwanie dostępu do magazynu kluczy za zaporą](key-vault-access-behind-firewall.md) - Aby uzyskać dostęp magazyn kluczy, które Magazyn kluczy aplikacja klienta musi mieć możliwość dostępu do wielu punktów końcowych dla różnych funkcji.

- [Jak i Generuj klucze Transfer HSM-Protected dla magazyn kluczy Azure](key-vault-hsm-protected-keys.md) - to pomoże Ci planowanie, generowanie i następnie przenieść swój własny chronione HSM klawisze, aby użyć magazyn kluczy Azure.
- [Jak przekazać bezpieczne wartości (na przykład hasła) podczas wdrażania](../resource-manager-keyvault-parameter.md) - gdy trzeba przekazać wartość bezpieczne (np. hasło) jako parametr podczas wdrażania, można przechowywać tę wartość jako klucz tajny w magazynie kluczy Azure i odniesienia wartości w innych szablonach Menedżera zasobów.
- [Jak używać magazyn kluczy dla Rozszerzalne zarządzanie kluczami z programem SQL Server](https://msdn.microsoft.com/library/dn198405.aspx) - łącznik serwera SQL dla magazyn kluczy Azure umożliwia SQL Server i SQL w VM wykorzystać usługę Magazyn kluczy Azure jako dostawca rozszerzonego Key Management (EKM) do ochrony jego kluczy szyfrowania dla łącza aplikacji; Przezroczyste szyfrowanie danych, szyfrowania kopii zapasowych i szyfrowanie na poziomie kolumny.
- [Sposobu wdrażania certyfikatów do maszyn wirtualnych z magazynu kluczy](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - aplikacji chmury, działa na maszynie Wirtualnej Azure wymaga specjalnego certyfikatu. Jak uzyskać ten certyfikat do tej maszyny Wirtualnej na dziś?
- [Jak skonfigurować magazyn kluczy obrót klucza kompleksowe i audytowania](key-vault-key-rotation-log-monitoring.md) - to idzie poprzez jak skonfigurować inspekcję magazyn kluczy Azure i obrót klucza.

Aby uzyskać więcej wskazówek specyficznych zadań na integrowanie i magazynów kluczy za pomocą Azure zobacz [Przykłady szablonu Menedżera zasobów Ryan Jones magazyn kluczy](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

## <a name="integrated-with-key-vault"></a>Zintegrowany z magazyn kluczy

Artykuły te są o innych scenariuszach i usługi, które sprawiają, że jesteśmy z lub zintegrować z magazynu kluczy.

- [Szyfrowanie dysku Azure](../security/azure-security-disk-encryption.md) wykorzystuje przemysłu standardowych funkcji [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) systemu Windows i funkcji [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) systemu Linux, aby zapewnić szyfrowanie woluminu dla systemu operacyjnego i dysków danych. Roztwór jest zintegrowany z magazyn kluczy Azure, aby ułatwić sterowanie i zarządzanie kluczami szyfrowania dysku i tajemnice w ramach subskrypcji usługi Magazyn kluczy, przy jednoczesnym zapewnieniu, że wszystkie dane z dysków maszyny wirtualnej są szyfrowane w stanie spoczynku w magazynie Azure.


## <a name="supporting-libraries"></a>Biblioteki pomocnicze

- [Microsoft Azure klucz repozytorium Podstawowa biblioteka](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) zawiera `IKey` i `IKeyResolver` interfejsów do lokalizowania kluczy z identyfikatorów i wykonywania operacji z kluczami.

- [Rozszerzenia Vault klucz Microsoft Azure](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) zapewnia rozszerzone możliwości magazyn kluczy Azure.

## <a name="other-key-vault-resources"></a>Inne zasoby magazynu kluczy
- [Magazyn kluczy blogu](http://aka.ms/kvblog)
- [Forum magazyn kluczy](http://aka.ms/kvforum)
