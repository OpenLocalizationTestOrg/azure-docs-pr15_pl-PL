<properties
    pageTitle="Praca z domen niestandardowych w serwera Proxy aplikacji Azure AD | Microsoft Azure"
    description="Obejmuje jak działają z domen niestandardowych w Azure AD serwera Proxy aplikacji."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Praca z domen niestandardowych w Azure AD serwera Proxy aplikacji

Za pomocą domenę domyślną umożliwia ustawianie tego samego adresu URL jako adres URL wewnętrznych i zewnętrznych do uzyskiwania dostępu do aplikacji, tak aby użytkownicy mają tylko jeden adres URL do Pamiętaj, aby uzyskać dostęp do aplikacji, niezależnie od tego, gdzie uzyskują dostęp do z. Pozwala to także utworzyć skrót pojedynczy w panelu dostępu dla aplikacji. Jeśli używasz domenę domyślną dostarczony przez serwer Proxy aplikacji Azure AD istnieje żadne dodatkowe czynności konfiguracyjne, musisz włączyć domeny. W przypadku, gdy korzystasz z domeny niestandardowej, istnieje kilka rzeczy, które należy wykonać, aby upewnić się, że serwer Proxy aplikacji rozpoznaje domeny oraz weryfikowanych przez jego certyfikaty.

## <a name="selecting-your-custom-domain"></a>Wybieranie domeny niestandardowej

1. Publikowanie aplikacji zgodnie z instrukcjami podanymi w [aplikacjach Publikuj z serwera Proxy aplikacji](active-directory-application-proxy-publish.md).
2. Gdy aplikacja pojawi się na liście aplikacji, zaznacz go i kliknij przycisk **Konfiguruj**.
3. W obszarze **Zewnętrzny adres URL**wprowadź domeny niestandardowej.
4. Jeśli adres URL zewnętrznego jest https, pojawi przekazać certyfikatu, aby tego Azure może sprawdzić adres URL aplikacji. Możesz również przekazać certyfikat symboli wieloznacznych, który pasuje do zewnętrznego adresu URL aplikacji. Tej domeny musi być na liście usługi [Azure zweryfikowanych domen](https://msdn.microsoft.com/library/azure/jj151788.aspx). Azure musi mieć certyfikat dla domeny adres URL aplikacji lub certyfikatu symboli wieloznacznych, która jest zgodna z zewnętrznego adresu URL aplikacji.
5. Upewnij się dodać rekord DNS, który przekierowuje wewnętrzny adres URL do aplikacji, która umożliwia mieć ten sam adres URL dla dostępu do wewnętrznych i zewnętrznych i pojedynczy skrót do aplikacji na liście aplikacji użytkownika.

## <a name="frequently-asked-questions-about-working-with-custom-domains"></a>Często zadawane pytania dotyczące pracy z domen niestandardowych

P: czy mogę wybrać certyfikat już przekazane bez przekazania go ponownie?  
O: wcześniej przekazane certyfikaty są automatycznie powiązane z aplikacji, a jest dokładnie jedną certyfikatu odpowiadającego nazwa hosta aplikacji.  

P: jak dodać certyfikat i format należy eksportowany certyfikat można przekazać w?  
O: na stronie Konfiguracja aplikacji należy przesłać certyfikatu. Certyfikat powinny być plik PFX.  

P: certyfikaty ECC można używać?  
O: nie jest bez ograniczeń jawne metod podpisu.  

P: certyfikaty SAN można używać?  
O: tak.  

P: czy można używać symboli wieloznacznych certyfikaty  
O: tak.  

P: czy certyfikat różnych ma być używany dla każdej aplikacji?  
O: tak, chyba że obiema aplikacjami udostępnianie tego samego hosta zewnętrznych.  

P: czy zarejestrować nową domenę, można użyć tej domeny?  
O: tak, podawania listy domen z listy zweryfikowaną domeną dzierżawy.  

P: co się dzieje po wygaśnięciu certyfikatu?  
Odpowiedzi ostrzeżenie zostanie wyświetlony w sekcji certyfikatu na stronie Konfiguracja aplikacji. Gdy użytkownik próbuje uzyskać dostęp do aplikacji, ostrzeżenie o zabezpieczeniach będzie wyświetlana.  

P: co zrobić, aby zamienić certyfikat dla danej aplikacji?  
O: Przekaż nowy certyfikat na stronie Konfiguracja aplikacji.  

P: czy mogę usunąć certyfikatu i zastąpić ją?  
O: w przypadku przekazywania nowego certyfikatu, jeśli stary certyfikat nie jest używany przez inną aplikację zostaną automatycznie usunięte.  

P: co się dzieje, gdy certyfikat został odwołany?  
O: sprawdzanie odwołań nie są wykonywane certyfikatów. Gdy użytkownik próbuje uzyskać dostęp do aplikacji, w zależności od przeglądarki, może zostać wyświetlony ostrzeżenie o zabezpieczeniach.  

P: czy mogę używać certyfikatu z podpisem własnym  
O: tak wolno certyfikatów z podpisem własnym. Należy zauważyć, że korzystasz z urzędu certyfikacji prywatne, punkty dystrybucji (punkt dystrybucji punktu odwołań certyfikatów) certyfikatu należy publicznej.  

P: czy istnieje miejscu, aby wyświetlić wszystkie certyfikaty dla mojej dzierżawie?  
O: to nie jest obsługiwane w bieżącej wersji.  


## <a name="see-also"></a>Zobacz też

- [Publikowanie aplikacji za pomocą serwera Proxy aplikacji](active-directory-application-proxy-publish.md)
- [Włączanie rejestracji jednokrotnej](active-directory-application-proxy-sso-using-kcd.md)
- [Włączanie dostępu warunkowego](active-directory-application-proxy-conditional-access.md)
- [Dodawanie niestandardowej nazwy domeny do Azure AD](active-directory-add-domain.md)

Aby uzyskać najnowsze informacje i aktualizacje zapoznaj się z [serwera Proxy aplikacji blogu](http://blogs.technet.com/b/applicationproxyblog/)
