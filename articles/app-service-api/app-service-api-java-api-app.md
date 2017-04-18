<properties
    pageTitle="Tworzenie i wdrażanie aplikacji interfejsu API języka Java Azure aplikacji usługi"
    description="Dowiedz się, jak utworzyć pakiet aplikacji interfejsu API języka Java i Wdroż Azure aplikacji usługi."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Tworzenie i wdrażanie aplikacji interfejsu API języka Java Azure aplikacji usługi

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Ten samouczek pokazano, jak utworzyć aplikację Java i Wdroż interfejsu API usługi aplikacji Azure za pomocą [cyfra]. Z instrukcjami podanymi w tym samouczku mogą występować we wszystkich systemach operacyjnych, który jest możliwe uruchamianie Java. Kod w ramach tego samouczka jest tworzona przy użyciu [środowiska Maven]. [JAX-r] służy do tworzenia RESTful usługi i jest generowany na podstawie przy użyciu [Edytora Swagger]specyfikacji [Swagger] metadanych.

## <a name="prerequisites"></a>Wymagania wstępne

1. [Java Developer Kit 8] \(lub nowsza)
1. [Środowiska maven] zainstalowana na tym komputerze opracowywania
1. [Cyfra] zainstalowana na tym komputerze opracowywania
1. Subskrypcję zapłaconej lub [bezpłatną wersję próbną] usługi [Microsoft Azure]
1. Aplikację test HTTP, taką jak [Postman]

## <a name="scaffold-the-api-using-swaggerio"></a>Scaffold API przy użyciu Swagger.IO

Przy użyciu edytora online swagger.io, można wprowadzić kod Swagger JSON lub YAML odwzorować strukturę z interfejsu API. Po umieszczeniu powierzchni interfejsu API przeznaczona, możesz wyeksportować kodu dla różnych platform i struktury. W następnej sekcji scaffolded kod zostaną zmodyfikowane, aby zasymulować funkcjami. 

Ta rozpocznie się o układzie Swagger JSON, w której chcesz wkleić do edytora swagger.io, która następnie będzie używana do generowania kodu wykorzystania JAX RS Aby uzyskać dostęp do punktu końcowego interfejsu API usługi REST. Następnie można będzie edytować scaffolded kod zwrócenie danych zasymulować symulowanie interfejsu API usługi REST wbudowany nad mechanizmu utrzymywanie danych.  

1. Skopiuj poniższy kod Swagger JSON do Schowka:

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Przejdź do [trybu Online Swagger edytora]. Kliknij jeden raz, element menu **Plik -> Wklej JSON** .

    ![Element menu Wklej JSON][paste-json]

1. Wklej kontakty listy interfejsu API Swagger JSON skopiowany wcześniej. 

    ![Wklejanie kodu JSON w Swagger][pasted-swagger]

1. Wyświetlanie stron dokumentacji i podsumowanie interfejsu API renderowane w edytorze. 

    ![Widok Swagger wygenerowane dokumenty][view-swagger-generated-docs]

1. Wybierz opcję menu **JAX-r-> Generuj serwera** , aby scaffold kodu po stronie serwera, będzie edytować później dodać zasymulować implementacji. 

    ![Generowanie kodu element Menu][generate-code-menu-item]

    Wygenerowany kod będzie znajdować się pliku ZIP do pobrania. Ten plik zawiera kod scaffolded przez generator kodu Swagger i wszystkie skojarzone tworzenia skryptów. Rozpakuj plik całą bibliotekę z katalogu w miejscu pracy rozwoju. 

## <a name="edit-the-code-to-add-api-implementation"></a>Edytować kod, aby dodać implementacji interfejsu API

W tej sekcji kodu wygenerowane Swagger po stronie serwera implementacji będzie Zamień niestandardowy kod. Nowy kod zwróci podmioty ArrayList kontaktu do klienta wywołującego. 

1. Otwórz plik modelu *Contact.java* , który znajduje się w folderze *src Generator java i EI swagger modelu* przy użyciu [Programu Visual Studio kod] lub edytora tekstu Ulubione. 

    ![Otwórz plik kontaktów][open-contact-model-file]

1. Dodaj następujące konstruktora klasy **kontaktu** . 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. Otwórz plik implementacji usługi *ContactsApiServiceImpl.java* , który znajduje się w folderze *src główne java i EI swagger-interfejsu api-impl* przy użyciu [Programu Visual Studio kod] lub edytora tekstu Ulubione.

    ![Otwieranie kontaktów usługi pliku kodu][open-contact-service-code-file]

1. Zastąp kod w pliku ten nowy kod, aby dodać zasymulować implementacji kod usługi. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Otwórz wiersz polecenia i zmień katalog do folderu głównego aplikacji.

1. Wykonaj następujące polecenie środowiska Maven, aby utworzyć kod, a następnie uruchom go przy użyciu serwera aplikacji molo lokalnie. 

        mvn package jetty:run

1. Powinien zostać wyświetlony okna odzwierciedlenia molo został uruchomiony kodu na porcie 8080. 

    ![Otwieranie kontaktów usługi pliku kodu][run-jetty-war]

1. Aby wprowadzić żądanie "Pobierz wszystkie kontakty" metoda interfejsu API na 8080/interfejsu api i kontaktów za pomocą [Postman] .

    ![Nawiązywać połączenia z kontaktami interfejsu API][calling-contacts-api]

1. Za pomocą [Postman] złożyć wniosek do znajdującego się w 8080/interfejsu api kontakty/2 metody interfejsu API "Pobierz z określonym kontaktem".

    ![Nawiązywać połączenia z kontaktami interfejsu API][calling-specific-contact-api]

1. Ponadto można utworzyć plik też Java (archiwum sieci Web), wykonując następujące polecenie środowiska Maven w konsoli. 

        mvn package war:war

1. Po skonstruowaniu go też będą znajdować się do tego folderu **docelowego** . Przejdź do folderu **docelowego** i Zmień nazwę pliku też na **ROOT.war**. (Upewnij się, że w tym formacie są zgodne wielkości liter).

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Na koniec wykonaj następujące polecenia w folderze głównym, aby utworzyć folder **Wdrażanie** Aby wdrożyć go też Azure za pomocą aplikacji. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>Publikowanie danych wyjściowych Azure aplikacji usługi

W tej sekcji będzie Dowiedz się, jak tworzyć nowej aplikacji interfejsu API przy użyciu Azure Portal, przygotowywanie, aplikacja interfejsu API do obsługi aplikacji Java i wdrażanie nowo utworzony plik też Azure aplikacji usługi Uruchamianie nowej aplikacji interfejsu API. 

1. Tworzenie nowej aplikacji interfejsu API w [Azure portal], klikając element menu **Nowy -> Web + Mobile -> aplikacji interfejsu API** , wprowadzanie informacji aplikacji i klikając polecenie **Utwórz**.

    ![Tworzenie nowej aplikacji interfejsu API][create-api-app]

1. Po utworzeniu aplikacji interfejsu API Otwórz karta **Ustawienia** Twojej aplikacji, a następnie kliknij element menu **Ustawienia aplikacji** . Wybierz pozycję najnowsze wersje Java z dostępnych opcji, a następnie wybierz najnowszą Tomcat z menu **kontener sieci Web** , a następnie kliknij **Zapisz**.

    ![Konfigurowanie Java w karta interfejsu API aplikacji][set-up-java]

1. Kliknij element menu ustawienia **wdrażania poświadczeń** i podaj nazwę użytkownika i hasło, którego chcesz użyć do publikowania plików aplikacji interfejsu API. 

    ![Ustaw poświadczenia wdrażania][deployment-credentials]

1. Kliknij element menu ustawienia **Źródło rozmieszczania** . Jeden raz, kliknij przycisk **Wybierz źródło** , wybierz opcję **Lokalnego repozytorium cyfra** , a następnie kliknij **przycisk OK**. Spowoduje to utworzenie repozytorium cyfra działa Azure, która jest skojarzona z aplikacji interfejsu API. Za każdym razem zatwierdzić kod *wzorca* gałęzi repozytorium cyfra, kod zostanie opublikowany na żywo uruchomionego wystąpienia interfejsu API aplikacji. 

    ![Konfigurowanie nowego lokalnego repozytorium cyfra][select-git-repo]

1. Skopiuj adres URL nowego repozytorium cyfra do Schowka. Zapisz to jako ważne za chwilę. 

    ![Konfigurowanie nowego repozytorium cyfra aplikacji][copy-git-repo-url]

1. Cyfra przekazać plik też do repozytorium online. Aby to zrobić, przejdź do folderu **Wdrażanie** utworzony wcześniej tak, aby łatwo można przekazać kodu do repozytorium działa w aplikacji usługi. Gdy jesteś w oknie konsoli i nawigację do folderu, w którym znajduje się folder używanie, problemów następujące polecenia cyfra, aby uruchomić proces i uruchomienie poza wdrożeniu. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Gdy problem żądanie **wypychania** zostanie wyświetlony monit o podanie hasła dla poświadczenia wdrożenia wcześniej utworzoną. Po wprowadzeniu poświadczeń powinna być widoczna portalem wyświetlanie wdrożonej aktualizacji.

1. Jeśli używasz ponownie Postman trafienie interfejsu API nowo wdrożony aplikacji działa usługa Azure aplikacji, zobaczysz zachowanie jest spójne i że teraz go zwraca dane kontaktowe zgodnie z oczekiwaniami, a za pomocą kodu proste zmiany Swagger.io scaffolded kodu języka Java. 

    ![Korzystanie z interfejsu API usługi REST kontaktów Java live platformy Azure][postman-calling-azure-contacts]

## <a name="next-steps"></a>Następne kroki

W tym artykule udało Ci się zaczynać pliku Swagger JSON, a niektóre scaffolded kodu języka Java uzyskanego z edytora Swagger.io. W tym miejscu proste zmiany i cyfra wdrażanie proces spowodowało po funkcjonalnych aplikacji interfejsu API napisany w języku Java. Następnej pokazano samouczka jak [korzystać z interfejsu API aplikacji z klientami JavaScript, przy użyciu CORS][App Service API CORS]. Samouczki później w serii pokazująca, jak zaimplementować uwierzytelniania i autoryzacji.

Aby utworzyć, na przykład, możesz więcej informacji o [SDK miejsca do magazynowania dla języka Java] pozostać blob JSON. Lub można użyć [Dokumentu DB Java SDK] zapisanie danych kontaktów do Azure DB dokumentu. 

Aby uzyskać więcej informacji o używaniu języka Java w Azure zobacz [Centrum deweloperów języka Java].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure portal]: https://portal.azure.com/
[Dokument DB Java SDK]: ../documentdb/documentdb-java-application.md
[Bezpłatna wersja próbna]: https://azure.microsoft.com/pricing/free-trial/
[Cyfra]: http://www.git-scm.com/
[Centrum deweloperów języka Java]: /develop/java/
[Java Developer Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[JAX r]: https://jax-rs-spec.java.net/
[Środowiska maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Edytor online Swagger]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[Magazyn SDK dla języka Java]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Edytor swagger]: http://editor.swagger.io/
[Kod Visual Studio]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
