<properties 
    pageTitle="Licencjonowania wygładzonymi klienta przesyłanie strumieniowe Microsoft® przenoszenia Kit" 
    description="Dowiedz się, jak do zasad licencjonowania Microsoft® wygładzonymi Streaming klienta przenoszenia Kit." 
    services="media-services" 
    documentationCenter="" 
    authors="xpouyat,vsood" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016"  
    ms.author="xpouyat"/>

#<a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Licencjonowania wygładzonymi klienta przesyłanie strumieniowe Microsoft® przenoszenia Kit

##<a name="overview"></a>Omówienie

Microsoft wygładzonymi Streaming klienta przenoszenia Kit (**SSPK** dla krótkiej) jest implementacją klienta wygładzonymi Streaming zoptymalizowane pod kątem producenci sprzętu osadzony, kabla i Operatorzy sieci komórkowych, zawartości usługodawców, słuchawki producentów, niezależnych dostawców oprogramowania (ISV) i programistom tworzenie produkty i usługi przesyłania strumieniowego adaptacyjne zawartości strumieniowych w formacie wygładzonymi Streaming. SSPK jest urządzenia i platformy implementacji niezależnych wygładzonymi Streaming klienta, który można przenieść przez licencjobiorcę do dowolnego urządzenia i platformy. 

Dołączana poniżej jest architektury wysokiego poziomu i usług IIS wygładzonymi Streaming przenoszenia Kit pole jest implementacji wygładzonymi klienta Streaming dostarczane przez firmę Microsoft i zawiera wszystkie logikę core do odtwarzania wygładzonymi strumieniowych. Jest to następnie przenieść przez partnerów dla konkretnego urządzenia lub platformy z zastosowaniem odpowiednich interfejsów. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

##<a name="description"></a>Opis

SSPK jest licencjonowany na warunkach, które oferują doskonały biznesu. Licencja SSPK zawiera branżowe z:

- Gładkie źródła strumieniowego przesyłania zestawu przenoszenia w języku C++ 
  - zawiera funkcję płynnego Streaming klienta
  - Dodaje formatowanie analizowania heurystykę, buforowanie logiczny itp.
- Aplikacja odtwarzacza interfejsy API 
  - interfejsy programowania interakcji z aplikacja odtwarzacza multimedialnego
- Interfejs warstwy (PAL) abstrakcji platformy 
  - interfejsy programowania interakcji z systemem operacyjnym (wątków, sockets)
- Interfejs Layer (HAL) abstrakcji sprzętu 
  - interfejsy interakcji ze sprzętem A / V dekoderów (dekodowanie, renderowania)
- Interfejs zarządzania (prawami cyfrowymi DRM) prawami cyfrowymi 
  - interfejsy programowania obsługi DRM za pośrednictwem warstwy abstrakcji DRM (DAL)
  - Microsoft PlayReady przenoszenia Kit dostarczany oddzielnie, ale można zintegrować przez ten interfejs. Aby uzyskać więcej informacji dotyczących licencjonowania firmy Microsoft PlayReady Device, kliknij [tutaj](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
- Przykłady implementacji 
  - Przykładowe zastosowanie PAL Linux
  - Przykładowe zastosowanie HAL dla GStreamer

##<a name="licensing-options"></a>Opcji licencjonowania

Microsoft wygładzonymi Streaming klienta przenoszenia Kit jest udostępniana licencjobiorców w obszarze dwóch umów odrębnych licencji: jedną do tworzenia wygładzonymi produktów tymczasowy klienta Streaming i innego do dystrybucji wygładzonymi Streaming klienta produktów końcowych do użytkowników końcowych.
 
- Producenci chipset, integratorów lub niezależnych dostawców oprogramowania (ISV) wymagających kod źródłowy przenoszenia kit opracowywanie produktów tymczasowy Microsoft wygładzonymi Streaming klienta przenoszenia Kit **Licencji produktu tymczasowy** powinna zostać wykonana.
- Producenci sprzętu lub niezależnych dostawców oprogramowania, którzy potrzebują prawami dystrybucji wygładzonymi Streaming klienta produktów końcowych do użytkowników końcowych należy wykonać Microsoft wygładzonymi Streaming klienta przenoszenia Kit **Ostateczne licencji produktu** .

###<a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Wygładzanie Microsoft Streaming klienta przenoszenia licencji produktu tymczasowy Kit

W obszarze tej licencji Microsoft oferuje wygładzonymi Streaming klienta przenoszenia zestaw i praw własności intelektualnej niezbędne do opracowanie i rozpowszechnianie wygładzonymi produktów tymczasowy klienta Streaming innym licencjobiorcom urządzenia wygładzonymi Streaming klienta przenoszenia Kit, które wygładzonymi Streaming klienta produktów końcowych.

####<a name="fee-structure"></a>Struktura opłat

Opłata jednorazowej licencji 50 000 zł USA zapewnia dostęp do płynnego Streaming klienta przenoszenia zestawu. 

###<a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Wygładzanie Microsoft Streaming klienta przenoszenia licencji produktu Kit wersji ostatecznej

W obszarze tej licencji firma Microsoft udostępnia wszystkie prawa własności intelektualnej niezbędne do odbierania wygładzonymi produktów tymczasowy klienta Streaming z innym licencjobiorcom wygładzonymi Streaming klienta przenoszenia Kit i rozpowszechnianie marki firmy wygładzonymi Streaming klienta produktów końcowych do użytkowników końcowych.

####<a name="fee-structure"></a>Struktura opłat

Gładkie Streaming klienta produktu ostatecznego jest dostępne w obszarze modelu royalty jako w obszarze:

- 0,10 $ na urządzeniu wdrożenia dołączona do
- Royalty jest ograniczona do 50 000 zł każdego roku
- Nie royalty pierwszych 10 000 implementacji urządzenia każdego roku 

##<a name="licensing-procedure-and-sspk-access"></a>Procedurę licencjonowania i SSPK dostępu

Wyślij wiadomość e-mail [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) dla wszystkich Licencjonowanie kwerendy.

[Portal rozkładu SSPK](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) jest dostępny dla zarejestrowanych licencjobiorców tymczasowego.

Tymczasowe i ostateczne SSPK licencjobiorców można zadawać pytania techniczne na [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).

##<a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Wygładzanie Microsoft Streaming licencjobiorców Umowa dotycząca produktu pośredniego klienta

- Adroit rozwiązań biznesowych, Inc.
- Zaawansowane SA emisji cyfrowe
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis technologii Ltd.
- Firma Alticast
- Cyfrowe Amazon usług, Inc.
- Oprogramowanie Multimedia AVC Company, Ltd.
- Cavium, Inc.
- EchoStar Corporation zakupu
- Enseo, Inc.
- Fluendo SA
- BroadInfoCom HANDAN Company, Ltd.
- Infomir GMBH
- Irdeto USA Inc.
- Wolności globalnych usług BV
- MediaTek Inc.
- MStar Co, Ltd.
- Nintendo Company, Ltd.
- OpenTV, Inc.
- Ograniczone cyfrowe szafran
- Elektryczne Changhong prowincji Syczuan Co., Ltd.
- SoftAtHome
- Sony Corporation
- Tatung technologii Inc.
- Electronics Technoly (Huizhou) TCL Company, Ltd.
- Zapisz Vestel Elektronik Sanayi Ticaret A.S.
- VisualOn, Inc.
- Firma ZTE

##<a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Wygładzanie Microsoft Streaming licencjobiorców Umowa dotycząca produktu ostatecznego klienta

- Zaawansowane SA emisji cyfrowe
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis technologii Ltd.
- Cyfrowe Amazon usług, Inc.
- Technologia AmTRAN Company, Ltd.
- Arcadyan Technology Corporation
- ATMACA ELEKTRONİK SAN. ZAPISZ TİC. A.Ş
- Ograniczona emisja Sky brytyjskich
- CastPal technologii Inc., Shenzhen
- Compal Electronics, Inc.
- Dongguan cyfrowe AV technologii Corp., Ltd.
- EchoStar Corporation zakupu
- Enseo, Inc.
- Ograniczone filmy Filmflex
- Fluendo SA
- Ograniczone innowacje Gibson
- Haier informacji Applicantion S.R.L
- BroadInfoCom HANDAN Company, Ltd.
- Homecast Co., Ltd.
- HON Hai dokładności branżowe Company, Ltd.
- Infomir GMBH
- Kaonmedia Company, Ltd.
- KDDI Corporation
- Nintendo Company, Ltd.
- Pomarańczowy SA
- Ograniczone cyfrowe szafran
- Połączenie szerokopasmowe Sagemcom skojarzenia zabezpieczeń
- Shenzhen Coship Electronics CO., LTD
- Elektryczne Jiuzhou Shenzhen Co., Ltd.
- Shenzhen Skyworth cyfrowe technologii Co., Ltd.
- Elektryczne Changhong prowincji Syczuan Company, Ltd.
- Skardin przemysłowych Corp.
- Sky Deutschland Fernsehen GmbH & Co. KG
- SmarDTV SA
- SoftAtHome
- Sony Corporation
- Ograniczone TCL zamorskich Marketing (Offshore komercyjnych Makau SAR)
- Technicolor dostarczania technologii, skojarzenia zabezpieczeń
- Tongfang Ltd. globalny
- Styl życia Toshiba produktów i usług Corporation
- S.r.o. /Slovakia/ uniwersalnej Corporation multimediów
- VIZIO, Inc.
- Firma Wistron
- Firma ZTE

##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
