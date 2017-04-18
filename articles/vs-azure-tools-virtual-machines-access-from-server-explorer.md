<properties
   pageTitle="Uzyskiwanie dostępu do Azure maszyn wirtualnych Eksploratora serwera | Microsoft Azure"
   description="Uzyskiwanie omówienie sposobu wyświetlania tworzenie i zarządzanie nimi Azure wirtualnych maszyn w Eksploratorze serwera w programie Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Uzyskiwanie dostępu do Azure maszyn wirtualnych Eksploratora serwera

Za pomocą Eksploratora serwera w programie Visual Studio, można wyświetlić informacje o obsługiwanych przez Azure maszyn wirtualnych.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Uzyskiwanie dostępu do maszyn wirtualnych w Eksploratorze serwera

Jeśli masz maszyn wirtualnych obsługiwanych przez Azure, możesz uzyskiwać do nich dostęp w Eksploratorze serwera. Musisz najpierw zalogować się do subskrypcji usługi Azure wyświetlanie usługi urządzeń przenośnych. Aby się zalogować, otwórz menu skrótów dla węzła Azure w Eksploratorze serwera i wybierz pozycję **Połącz z programem Microsoft Azure**.

### <a name="to-get-information-about-your-virtual-machines"></a>Aby uzyskać informacje o środowisku maszyn wirtualnych systemu

1. W Eksploratorze serwera wybierz maszyny wirtualnej, a następnie wybierz klawisz F4, aby wyświetlić jego okno właściwości.

    W poniższej tabeli przedstawiono, jakie właściwości są dostępne, ale są one wszystkie tylko do odczytu. Aby je zmienić, użyj [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).

  	|Właściwość|Opis|
  	|---|---|
  	|Nazwa DNS|Adres URL z adres internetowy maszyny wirtualnej.|
  	|Środowisko|Maszyny wirtualnej wartość tej właściwości jest zawsze produkcji.|
  	|Nazwa|Nazwa maszyny wirtualnej.|
  	|Rozmiar|Rozmiar maszyny wirtualnej, co odzwierciedla ilość miejsca pamięci i na dysku, który jest dostępny. Aby uzyskać więcej informacji, zobacz jak: Konfigurowanie rozmiarów maszyn wirtualnych.|
  	|Stan|Wartości to początkowy, wprowadzenie zatrzymania, zatrzymania i pobierania stanu. Jeśli pojawi się pobieranie stanu, bieżący stan jest nieznany. Wartości tej właściwości różnią się od wartości, które są używane w [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885).|
  	|SubscriptionID|Identyfikator subskrypcji dla Twojego konta Azure. Możesz wyświetlić te informacje w [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885) , wyświetlając właściwości dla subskrypcji.|

1. Wybierz węzeł punktu końcowego, a następnie Wyświetl okno **Właściwości** .

1. W poniższej tabeli opisano dostępne właściwości punktów końcowych, ale są tylko do odczytu. Aby dodać lub edytować punkty końcowe dla maszyny wirtualnej, użyj [portal Azure klasyczny](http://go.microsoft.com/fwlink/?LinkID=213885). 

  	|Właściwość|Opis|
  	|---|---|
  	|Nazwa|Identyfikator punktu końcowego.|
  	|Port prywatny|Port dla dostępu do sieci wewnętrznej do aplikacji.|
  	|Protocol (protokół)|Protokół, który korzysta z warstwy transportu dla tego punktu końcowego, port TCP i UDP.|
  	|Port publiczny|Port używaną w dostępie do aplikacji.|

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o korzystaniu z platformy Azure ról w programie Visual Studio, zobacz [Za pomocą pulpitu zdalnego z rolami Azure](vs-azure-tools-remote-desktop-roles.md).
