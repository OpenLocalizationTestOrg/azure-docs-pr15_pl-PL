<properties
   pageTitle="Szyfrowanie dysków maszyny Linux | Microsoft Azure"
   description="Szyfrowanie dysków maszyny Linux przy użyciu interfejsu wiersza polecenia Azure i model wdrożenia Menedżera zasobów"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/11/2016"
   ms.author="iainfou"/>

# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli"></a>Szyfrowanie dysków maszyny Linux za pomocą interfejsu wiersza polecenia Azure
Dla rozszerzonym maszyn wirtualnych (maszyn wirtualnych) zabezpieczenia i zgodność można zaszyfrować dyski wirtualne platformy Azure spoczynku. Dyski są szyfrowane przy użyciu klucze szyfrowania zabezpieczonych w magazynu klucza Azure. Możesz kontrolować następujące klucze szyfrowania i można przeprowadzać inspekcję ich stosowania. W tym artykule opisano szyfrowanie dysków wirtualnych w maszyny Linux przy użyciu interfejsu wiersza polecenia Azure i model wdrożenia Menedżera zasobów.


## <a name="quick-commands"></a>Szybkie polecenia
Jeśli chcesz szybko wykonywać zadania, na następujące szczegóły sekcji podstawy polecenia szyfrowanie dysków wirtualnych w swojej maszyn wirtualnych. Pozostała część dokumentu, [Uruchamianie w tym miejscu](#overview-of-disk-encryption)można znaleźć bardziej szczegółowych informacji i kontekst dla każdego kroku.

Potrzebujesz [Najnowszych polecenie Azure](../xplat-cli-install.md) zainstalowane i zarejestrowane w trybie Menedżera zasobów w następujący sposób:

```
azure config mode arm
```

W poniższych przykładach należy zastąpić przykładowe nazwy parametrów własne wartości. Nazwy parametrów przykład obejmują `myResourceGroup`, `myKeyVault`, i `myVM`.

Najpierw należy włączyć dostawcy Azure klucza magazynu w ramach subskrypcji usługi Azure i utworzyć grupę zasobów. Poniższy przykład tworzy nazwę grupy zasobów `myResourceGroup` w `WestUS` lokalizacji:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Tworzenie Azure magazynu kluczy. Poniższy przykład tworzy magazynu klucza o nazwie `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Tworzenie klucza szyfrowania w swojej magazynu klucza i włącz go do szyfrowania dysku. W poniższym przykładzie tworzony klucz o nazwie `myKey`:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Tworzenie punktu końcowego do obsługi uwierzytelniania i wymiany kluczy szyfrowania z magazynu klucza przy użyciu usługi Azure Active Directory. `--home-page` i `--identifier-uris` nie muszą być rzeczywisty adres routingu. Na najwyższym poziomie zabezpieczeń hasła klienta powinien być używany zamiast hasła. Polecenie Azure obecnie nie można wygenerować hasła klienta. Hasła klienta mogą być generowane tylko w Azure portal. Poniższy przykład tworzy punktu końcowego usługi Azure Active Directory o nazwie `myAADApp` i używa hasła `myPassword`. Określ swoje hasło w następujący sposób:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Uwaga `applicationId` wyświetlana w danych wyjściowych z poprzedniego polecenia. Ten identyfikator aplikacji jest używany w następującej procedurze:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Dodawanie dysku danych do istniejącej maszyn wirtualnych. W poniższym przykładzie dodawane dysku danych do maszyny o nazwie `myVM`:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Przejrzyj szczegóły swojego magazynu klucza i klawisz, który został utworzony. Potrzebujesz identyfikator magazynu klucza, identyfikator URI i klucza adresu URL w ostatnim kroku. Poniższy przykład przegląda szczegóły dla magazynu klucza o nazwie `myKeyVault` i klucz o nazwie `myKey`:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Szyfrowanie dysków następująco, wprowadzając własne nazwy parametrów w całym:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Polecenie Azure nie zapewnia pełne błędów podczas procesu szyfrowania. Aby uzyskać dodatkowe informacje dotyczące rozwiązywania problemów, przejrzyj `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Podczas wykonywania poprzedniego polecenia ma wiele zmiennych i można uzyskać wiele informuje o tym, dlaczego proces kończy się niepowodzeniem, przykład wszystkie polecenia będzie w następujący sposób:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Na koniec sprawdź stan szyfrowania ponownie, aby potwierdzić, że dyski wirtualne mają teraz szyfrowane. W poniższym przykładzie sprawdzana stanu maszyny o nazwie `myVM` w `myResourceGroup` grupa zasobów:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Omówienie szyfrowania dysku
Dysków wirtualnych w maszyny wirtualne Linux są szyfrowane na pozostałych przy użyciu [kryptograficznego dm](https://wikipedia.org/wiki/Dm-crypt). Jest bezpłatna do szyfrowania dysków wirtualnych w Azure. Klucze szyfrowania są przechowywane w magazynu klucza Azure za pomocą oprogramowania ochrony lub można importować lub wygenerowania kluczy w modułach wartościowego sprzętu (HSM) certyfikat FIPS 140-2 standardy poziomu 2. Zachować kontrolę nad klucze szyfrowania i można przeprowadzać inspekcję ich stosowania. Klucze szyfrowania są używane do szyfrowania i odszyfrowywania dysków wirtualnych dołączone do Twojej maszyn wirtualnych. Punktu końcowego usługi Azure Active Directory udostępnia bezpieczny mechanizm wydawania klucze szyfrowania, jak pośrednictwem SMS są włączone, włączanie i wyłączanie funkcji.

Proces szyfrowania maszyny jest w następujący sposób:

1. Tworzenie klucza szyfrowania w magazynu klucza Azure.
2. Konfigurowanie klucz kryptograficzny może być używany do szyfrowania dysków.
3. Aby odczytać klucz kryptograficzny z magazynu klucza Azure, tworzenie punktu końcowego za pomocą usługi Azure Active Directory z odpowiednimi uprawnieniami.
4. Wydaj polecenie Szyfrowanie dysków wirtualnych określającą punktu końcowego usługi Azure Active Directory i odpowiednie klucza szyfrowania ma być używany.
5. Punkt końcowy usługi Azure Active Directory żąda wymaganego klucza szyfrowania z magazynu klucza Azure.
6. Dyski wirtualne są szyfrowane za pomocą dostarczonych klucz kryptograficzny.


## <a name="supporting-services-and-encryption-process"></a>Obsługa usług i proces szyfrowania
Szyfrowanie dysków zależy od następujące dodatkowe składniki:

- **Azure klucza magazynu** - używany do zabezpieczenia klucze szyfrowania i hasła na potrzeby procesu Szyfrowanie/odszyfrowywanie dysków. 
  - Jeśli istnieje, możesz użyć istniejącego magazynu klucza Azure. Nie masz przeznaczoną magazynu klucz do szyfrowania dysków.
  - Aby rozdzielić granice administracyjne i widoczność klucza, możesz utworzyć dedykowane magazynu klucza.
- **Usługi azure Active Directory** — obsługuje bezpiecznego wymiany wymagane klucze szyfrowania i uwierzytelniania dla żądanej akcji. 
  - Zazwyczaj za pomocą istniejącego wystąpienia usługi Azure Active Directory do przechowywania aplikacji. 
  - Aplikacja jest więcej punktu końcowego usługi klucza magazynu i maszyn wirtualnych do żądania i uzyskać wydane odpowiednie klucze szyfrowania. Nie tworzysz aplikację rzeczywista, która jest zintegrowany z usługi Azure Active Directory.


## <a name="requirements-and-limitations"></a>Wymagania i ograniczenia
Scenariusze obsługiwane i wymagania szyfrowania dysku:

- Następujące serwer Linux SKU - Ubuntu, CentOS SUSE i SUSE Linux Enterprise Server (SLES) i Red funkcję Enterprise Linux.
- Wszystkie zasoby (na przykład magazynu klucz konta miejsca do magazynowania i maszyn wirtualnych) muszą być w tym samym regionie Azure i subskrypcji.
- Standardowy A, D, DS, G i GS seria maszyny wirtualne.

Szyfrowanie dysków nie jest obecnie obsługiwane w następujących sytuacjach:

- Podstawowe warstwa maszyny wirtualne.
- Maszyny wirtualne utworzony przy użyciu modelu wdrożenia klasyczny.
- Wyłączanie szyfrowania dysku systemu operacyjnego maszyny wirtualne Linux.
- Aktualizowanie klucze szyfrowania na już zaszyfrowane maszyn wirtualnych Linux.


## <a name="create-the-azure-key-vault-and-keys"></a>Tworzenie magazynu klucza Azure i klawiszy
Do wykonania w dalszej części tego podręcznika, potrzebujesz [Najnowszych polecenie Azure](../xplat-cli-install.md) zainstalowane i zarejestrowane w trybie Menedżera zasobów w następujący sposób:

```
azure config mode arm
```

W przykładach polecenie Zamień wszystkie parametry przykład własnej nazwy, lokalizację i wartości klucza. W poniższych przykładach użyto Konwencji `myResourceGroup`, `myKeyVault`, `myAADApp`itp.

Pierwszym krokiem jest utworzenie Azure klucza magazynu do przechowywania klucze szyfrowania. Azure magazynu klucza mogą zawierać klawiszy, hasła lub hasła, które umożliwiają bezpieczne wykonania w swojej aplikacji i usług. Do szyfrowania dysku wirtualnego klucza magazynu jest służy do przechowywania klucz kryptograficzny używany do szyfrowania lub odszyfrowywania dysków wirtualnych. 

Włączanie dostawcy Azure klucza magazynu w ramach subskrypcji Azure, a następnie utworzyć grupę zasobów. Poniższy przykład tworzy grupę zasobów o nazwie `myResourceGroup` w `WestUS` lokalizacji:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Magazyn klucza Azure zawierające klucze szyfrowania i zasobów skojarzone obliczeń, takich jak miejsca do magazynowania i maszyn wirtualnych sam musi znajdować się w tym samym regionie. Poniższy przykład tworzy Azure magazynu klucza o nazwie `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Mogą zawierać klucze szyfrowania za pomocą oprogramowania lub sprzętu Model zabezpieczeń (HSM) ochrony. Używanie HSM wymaga premium klucza magazynu. Istnieje kosztu dodatkowego tworzenia premium magazynu klucza zamiast standardowego magazynu klucza przechowującego oprogramowanie chronione klawiszy. Aby utworzyć premium klucza magazynu, w poprzednim kroku Dodaj `--sku Premium` do polecenia. W poniższym przykładzie użyto oprogramowanie chronione klawiszy, ponieważ utworzonych standardowego magazynu klucza. 

W przypadku obu modeli ochrony platformie Azure musi mieć przyznany dostęp do żądania klucze szyfrowania, gdy maszyn wirtualnych uruchomi się wirtualne dyski odszyfrowywanie. Utworzenie klucza szyfrowania w ramach usługi magazynu klawisz, a następnie włącz go do użytku z dysku wirtualnego szyfrowania. W poniższym przykładzie tworzony klucz o nazwie `myKey` i włączenie szyfrowania dysku:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Tworzenie aplikacji usługi Azure Active Directory
Gdy dyski wirtualne są szyfrowane lub odszyfrowywane, program punktu końcowego do obsługi uwierzytelniania i wymiany kluczy szyfrowania z magazynu klucza. Aplikacja usługi Azure Active Directory tego punktu końcowego umożliwia platformie Azure żądania odpowiednie klucze szyfrowania imieniu maszyn wirtualnych. W przypadku wystąpienia domyślnego usługi Azure Active Directory jest dostępny w subskrypcji, chociaż wiele organizacji dedykowany katalogów usługi Azure Active Directory.

Jak nie tworzysz aplikację usługi Azure Active Directory pełny `--home-page` i `--identifier-uris` parametrów w poniższym przykładzie nie muszą być rzeczywisty adres routingu. W poniższym przykładzie określa również tajne oparte na hasłach zamiast generowania kluczy z poziomu portalu Azure. Teraz generowania kluczy nie są dostępne w polecenie Azure. 

Tworzenie aplikacji usługi Azure Active Directory. Poniższy przykład tworzy aplikacji o nazwie `myAADApp` i używa hasła `myPassword`. Określ swoje hasło w następujący sposób:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Zanotuj `applicationId` którego zostanie zwrócony w wyniku poprzedniego polecenia. Ten identyfikator aplikacji jest używany w niektórych pozostałe kroki. Następnie utwórz główną nazwę usługi (SPN) tak, aby aplikacja jest dostępna w środowisku. Aby pomyślnie szyfrowanie lub odszyfrowywanie dysków wirtualnych, być równa uprawnienia do klucza szyfrowania przechowywanych w klucza magazynu zezwolić aplikacji usługi Azure Active Directory w celu odczytu kluczy. 

Tworzenie nazwy SPN i ustaw odpowiednie uprawnienia w następujący sposób:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Dodawanie dysku wirtualnego i przeglądanie stanu szyfrowania
Faktycznie szyfrowanie niektórych dysków wirtualnych umożliwia dodanie dysku do istniejącego maszyn wirtualnych. Dodawanie dysku 5Gb danych do istniejącej maszyn wirtualnych w następujący sposób:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Dyski wirtualne nie są obecnie szyfrowane. Przejrzyj bieżący stan szyfrowania z maszyn wirtualnych w następujący sposób:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Szyfrowanie dysków wirtualnych
Aby teraz zaszyfrować dyski wirtualne, możesz umożliwiają powiązanie poprzedniego składniki:

1. Określ aplikacji usługi Azure Active Directory i hasło.
2. Umożliwia określenie magazyn klucz do przechowywania metadanych dla zaszyfrowanych dysków.
3. Określ cryptographic klawisze służące do pracy rzeczywistej szyfrowania i odszyfrowywania.
4. Określ, czy chcesz zaszyfrować dysku systemu operacyjnego, dyski danych lub wszystkie.

Umożliwia, przejrzyj szczegóły dla usługi Azure klucza magazynu i klawisz, który został utworzony, potrzebny identyfikator magazynu klucza, identyfikator URI, a następnie klawisz adresu URL w ostatnim kroku:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Szyfrowanie dysków wirtualnych za pomocą wynikiem `azure keyvault show` i `azure keyvault key show` polecenia w następujący sposób:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Jak poprzedniego polecenia ma wiele zmiennych, poniższy przykład to polecenie ukończone dla odwołania:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Polecenie Azure nie zapewnia pełne błędów podczas procesu szyfrowania. Aby uzyskać dodatkowe informacje dotyczące rozwiązywania problemów, przejrzyj `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` na maszyn wirtualnych, są szyfrowania.

Na koniec umożliwia przeglądanie stanu szyfrowania ponownie, aby potwierdzić, że dyski wirtualne mają teraz szyfrowane:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Dodawanie dysków dodatkowych danych
Po zaszyfrowanych dysków danych, możesz później dodać dodatkowe dyski wirtualne do swojego maszyn wirtualnych i również zaszyfrować je. Po uruchomieniu `azure vm enable-disk-encryption` polecenia, Zwiększ przy użyciu wersji sekwencji `--sequence-version` parametru. Ten parametr wersji sekwencji umożliwia wykonywanie powtarzających się operacjach na tym samym maszyn wirtualnych.

Na przykład umożliwia dodawanie dysku wirtualnego drugiego do swojej maszyn wirtualnych w następujący sposób:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Uruchom polecenie Szyfrowanie dysków wirtualnych, dodając ten czas `--sequence-version` parametru oraz zwiększanie wartość z naszych pierwszego uruchomienia w następujący sposób:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji o zarządzaniu Azure klucza magazynu, w tym usuwanie klucze szyfrowania i magazynów zobacz [Zarządzanie klucza magazynu przy użyciu interfejsu wiersza polecenia](../key-vault/key-vault-manage-with-cli.md).
- Aby uzyskać więcej informacji na temat szyfrowania dysku, takich jak przygotowywanie zaszyfrowanych niestandardowe maszyn wirtualnych przekazywanie do Azure zobacz [Szyfrowanie dysków Azure](../security/azure-security-disk-encryption.md).