<properties
    pageTitle="Zaloguj się dodatki z wielu obszarów geograficznych"
    description="Raport, który wskazuje użytkownikom na miejsce, w którym dwa podpisywanie dodatki pojawiły się pochodzi z różnych regionów i czasu między znak, który dodatki uniemożliwia użytkownikowi mają przebytej między regionach."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="gchander"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-from-multiple-geographies"></a>Dodatki logowania z wielu obszarów geograficznych

Ten raport zawiera powiedzie rejestrowaniu użytkownikowi miejsce, w którym dwa rejestrowaniu pojawiła się pochodzi z różnych regionów i czasu między dodatki logowania uniemożliwia użytkownikowi mają w czasie podróży między regionach. Możliwe przyczyny są:

- Udostępnia swoje hasło użytkownika z innymi użytkownikami

- Użytkownik korzysta z pulpitu zdalnego do uruchamianie przeglądarki sieci web dla logowania

- Haker zalogował się do konta użytkownika z innego kraju

- Użytkownik korzysta z sieci VPN lub serwera proxy

- Użytkownik jest zalogowany na wielu urządzeniach w tym samym czasie, takich jak komputer stacjonarny i telefon komórkowy, a adres IP telefon komórkowy jest rzadko używana.

Wyniki z tego raportu zostanie wyświetlona pomyślnego logowania zdarzenia, razem z czasu między dodatki logowania, gdzie dodatki logowania okazało się pochodzą z regionów i podczas podróży szacowany między regionach. Podczas podróży wyświetlana jest tylko szacowana i mogą różnić się od czasu rzeczywistego podróży między lokalizacjami.


![Zaloguj się dodatki z wielu obszarów geograficznych](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)
