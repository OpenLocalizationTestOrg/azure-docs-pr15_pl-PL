<properties
    pageTitle="Co należy zrobić w przypadku Azure usługi zakłócenia w pracy, którego dotyczy Azure klucza magazynu | Microsoft Azure"
    description="Dowiedz się, co należy zrobić w przypadku awarii usługi Azure, którego dotyczy Azure klucza magazynu."
    services="key-vault"
    documentationCenter=""
    authors="adamglick"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="key-vault"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="sumedhb;aglick"/>


# <a name="azure-key-vault-availability-and-redundancy"></a>Azure dostępności klucza magazynu i nadmiarowości

Azure magazynu klucza funkcji wielokrotnej nadmiarowości, aby upewnić się, że klucze i hasła pozostają dostępne w aplikacji nawet w przypadku pojedynczych składników usługi kończą się niepowodzeniem.

Zawartość z magazynu kluczy są replikowane w regionie i pomocniczą region co najmniej 150 mila z dala od komputera, ale w tym samym geograficznych. To zachowuje wysokiej ważności klucze i hasła usługi.

Jeśli nie pojedynczych składników w usłudze klucza magazynu, alternatywny składniki w obszarze krok mogła obsługiwać Twojego żądania, aby upewnić się, że jest nie spadek funkcjonalności. Nie musisz podejmować żadnych działań wyzwalać to. Odbywa się automatycznie, a ma być przezroczysty dla Ciebie.

W rzadkich niedostępności całego regionu Azure żądania, wprowadzone z magazynu klucza Azure w danym regionie są automatycznie routingu (*nie powiodło się nad*) do obszaru pomocniczej. Gdy podstawowy obszar się ponownie, żądania są kierowane Wstecz (*nie można ponownie*) do obszaru podstawowego. Ponownie nie musisz podejmować żadnych działań, ponieważ dzieje się automatycznie.

Istnieje kilka ostrzeżenia, o których warto pamiętać:

* W przypadku awaryjnego przeniesienia region może potrwać kilka minut na awarię przez usługi. Żądania, które zostały wprowadzone w tym czasie może zakończyć się niepowodzeniem, dopóki nie zakończy się tym przełączeniu.
* Po zakończeniu trybie awaryjnym usługi klucza magazynu jest w trybie tylko do odczytu. Żądania, które są obsługiwane w tym trybie są:
 * Lista magazynami najważniejszych
 * Pobierz właściwości magazynami najważniejszych
 * Lista haseł
 * Pobieranie hasła
 * Lista kluczy
 * Uzyskiwanie kluczy (właściwości)
 * Szyfrowanie
 * Odszyfrowywanie
 * Zawijanie
 * Cofnij zawijanie
 * Sprawdź
 * Znak
 * Wykonywanie kopii zapasowych
* Po trybie awaryjnym to nie powiodło się ponownie, wszystkie typy żądania (w tym odczytu *i* zapisu żądania) są dostępne.
