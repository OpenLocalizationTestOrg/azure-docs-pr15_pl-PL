<properties
    pageTitle="Zezwalaj aplikacjom hasła Azure stos klucza magazynu revtrieve | Microsoft Azure"
    description="Korzystanie z aplikacji przykładowych do pracy z magazynu klucza stos Azure"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="run-the-sample-application-for-key-vault"></a>Uruchom aplikację próbki dla magazynu klucza 

W tym przewodniku za pomocą aplikacji przykładowej pobieranie hasła i hasła z magazynu klucza.

## <a name="download-the-samples-and-prepare"></a>Pobieranie próbki i przygotowywanie

Pobierz próbki klienta Azure klucza magazynu z [magazynu klucza Azure strona próbki klienta](https://www.microsoft.com/en-us/download/details.aspx?id=45343).

Wyodrębnianie zawartości pliku ZIP na komputer lokalny.

Przeczytaj plik **README.md** (jest to plik tekstowy), a następnie postępuj zgodnie z instrukcjami.

## <a name="run-sample-1--hellokeyvault"></a>Uruchamianie przykładowych #1 — HelloKeyVault
HelloKeyVault jest aplikacji konsoli, który przeprowadzi przez kluczowe scenariusze, które są obsługiwane przez magazynu klucza:

  1. Tworzenie/importu klawisz (HSM lub oprogramowania)
  2. Szyfrowanie hasła, przy użyciu klucza zawartości
  3. Zawijanie klucz zawartości przy użyciu klucza magazynu klucza
  4. Cofnij zawijanie klucz zawartości
  5. Odszyfrowywanie hasło
  6. Ustawianie hasła

Ta aplikacja konsoli powinna działać bez żadnych zmian, z wyjątkiem, że ustawienia odpowiednią konfigurację w App.Config zostaną zaktualizowane zgodnie z następujących czynności:

1. Zaktualizuj ustawienia konfiguracji aplikacji w HelloKeyVault\App.config z magazynu adresu URL, identyfikator głównej aplikacji i hasło. Informacje mogą być generowane opcjonalnie za pomocą **scripts\GetAppConfigSettings.ps1**.
2. Zaktualizuj wartości obowiązkowe zmiennych w GetAppConfigSettings.ps1.
3. Uruchom okna programu Windows PowerShell.
4. Uruchom skrypt GetAppConfigSettings.ps1 w oknie programu PowerShell.
5. Kopiowanie wyników skrypt do pliku HelloKeyVault\App.config.


## <a name="next-steps"></a>Następne kroki

[Wdrażanie maszyn wirtualnych za pomocą hasła klucza magazynu](azure-stack-kv-deploy-vm-with-secret.md)

[Wdrażanie maszyn wirtualnych za pomocą certyfikatu klucza magazynu](azure-stack-kv-push-secret-into-vm.md)