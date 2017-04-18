<properties 
    pageTitle="Znane sieci | Microsoft Azure" 
    description="Konfigurowanie sieci znane, można uniknąć konieczności adresów IP, które są własnością organizacji zawarte w dodatki logowania z wielu obszarów geograficznych i dodatki logowania z adresów IP przy użyciu raportów podejrzane działania." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"  
    editor=""/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markvi"/>

# <a name="known-networks"></a>Znane sieci


Dostęp do usługi Azure Active Directory i raporty użycia umożliwia uzyskanie wgląd integralności i bezpieczeństwa katalogu organizacji. Przy użyciu tych informacji administratora usługi katalogowej lepiej określić miejsce, w którym może znajdować się możliwe ryzyko związane z zabezpieczeniami, tak aby były odpowiednio można zaplanować ich eliminowania.

Może się zdarzyć, że raporty "*Dodatki logowania z wielu obszarów geograficznych*" i "*Dodatki logowania z adresów IP przy użyciu podejrzane działania*" Flaga niepoprawnie adresów IP, które są w rzeczywistości własnością Twojej organizacji. 

Dzieje się tak na przykład, gdy: 

- Użytkownik usługi office zalogował się zdalnie do centrum danych w łódź Boston wyzwalane raport "Logowanie dodatki z wielu obszarów geograficznych" 

- Organizacji próbie logowania jednokrotnego z wyzwalaczami niepoprawnego hasła raport "Logujesz dodatki z adresów IP podejrzane działania" 

Aby uniknąć tych przypadkach generowania raportów mylące zabezpieczeń, możesz dodać znane zakresy adresów IP, na liście publiczny adres IP Twojej organizacji.    


###<a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Aby dodać publicznej zakresy adresów IP w Twojej organizacji, wykonaj następujące czynności: 

1.  Zaloguj się w [portalu zarządzania Azure](https://manage.windowsazure.com).

2.  W okienku po lewej stronie kliknij **Usługi Active Directory**. <br><br>![Jak działa odnajdowania aplikacji chmury](./media/active-directory-known-networks/known-netwoks-01.png)

3.  Na karcie **katalogu** wybierz pozycję katalogu.

4.  W menu u góry kliknij przycisk **Konfiguruj**. <br><br>![Jak działa odnajdowania aplikacji chmury](./media/active-directory-known-networks/known-netwoks-02.png)

5.  Na karcie Konfigurowanie przejdź do **zakresów adresów IP publicznej Twojej organizacji** <br><br>![Jak działa odnajdowania aplikacji chmury](./media/active-directory-known-networks/known-netwoks-03.png)

6.  Kliknij przycisk **Dodaj zakresy adresów IP znane**.

7.  Dodawanie do zakresów adresów w oknie dialogowym, które zostanie wyświetlone, a następnie kliknij przycisk Sprawdź po wykonaniu tych czynności. <br><br>![Jak działa odnajdowania aplikacji chmury](./media/active-directory-known-networks/known-netwoks-04.png)


**Dodatkowe zasoby**


* [Wyświetlanie raportów dostępu i użytkowania](active-directory-view-access-usage-reports.md)
* [Dodatki logowania z adresów IP przy użyciu podejrzane działania](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Zaloguj się dodatki z wielu obszarów geograficznych](active-directory-reporting-sign-ins-from-multiple-geographies.md)


