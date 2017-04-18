<properties 
    pageTitle="Jak za pomocą Inspektora interfejsu API do śledzenia połączeń w zarządzaniu interfejsu API platformy Azure" 
    description="Dowiedz się, jak śledzenie połączenia za pomocą Inspektora interfejsu API w zarządzaniu interfejsu API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Jak za pomocą Inspektora interfejsu API do śledzenia połączeń w zarządzaniu interfejsu API platformy Azure

Interfejs API zarządzania zawiera narzędzie Inspektor interfejsu API ułatwiające debugowania i rozwiązywania problemów z interfejsów API. Inspektor interfejsu API można używać programowo i można również bezpośrednio z poziomu portalu Deweloper. 

Oprócz operacji śledzenia interfejsu API Inspektor śledzi ocen [wyrażenie zasad](https://msdn.microsoft.com/library/azure/dn910913.aspx) . Aby wyświetlić pokaz, zobacz [177 odcinka obejmuje chmury: więcej funkcji zarządzania interfejsu API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) i szybkie przewijanie do przodu do 21:00.

Ten przewodnik zawiera omówienie korzystania z interfejsu API Inspektor.

>[AZURE.NOTE] Inspektor interfejsu API śledzenia są tylko generowane i dostępne dla żądań zawierających klawiszy subskrypcji, które należą do konta [administratora](api-management-howto-create-groups.md) .

## <a name="trace-call"></a> Używanie Inspektora interfejsu API do śledzenia połączenia

Korzystając z Inspektora interfejsu API, Dodaj **ocp-apim śledzenia: PRAWDA** żądanie nagłówka do połączenia operacji, następnie pobrać i Przeprowadź inspekcję śledzenia przy użyciu adresu URL wskazywany przez nagłówka odpowiedzi **ocp apim śledzenia lokalizacji** . Można przeprowadzić filmowych i można również bezpośrednio z poziomu portalu Deweloper.

Ten samouczek pokazano, jak korzystając z Inspektora interfejsu API do śledzenia działań korzystanie z podstawowych interfejsu API kalkulatora skonfigurowanym [Zarządzanie do pierwszego interfejsu API](api-management-get-started.md) uzyskiwania samouczka Wprowadzenie. Jeśli nie wykonano tego samouczka trwa tylko kilka chwil do zaimportowania podstawowe API kalkulatora lub można użyć innego interfejsu API wybierania takie jak interfejs API Echo. Każde wystąpienie usługi zarządzania interfejsu API jest wstępnie skonfigurowana z interfejs API Echo, który może służyć do eksperymentować i informacje o zarządzaniu interfejsu API. Interfejs API Echo zwraca ponownie, niezależnie od danych wejściowych jest wysyłana do niego. Używanie tej funkcji, można wywołania dowolnej zlecenie HTTP, a zwrócona wartość będzie po prostu, została wysłana. 



Aby rozpocząć pracę, kliknij opcję **dzięki portalowi deweloperów** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API. Operacje można wywołać bezpośrednio z portalem Deweloper, który zapewnia wygodny sposób, aby wyświetlić i przetestować operacje interfejsu API.

>Jeśli nie została jeszcze utworzona wystąpienie usługi zarządzania interfejsu API, zobacz [Tworzenie wystąpienia usługi zarządzania interfejsu API][] samouczka [Wprowadzenie do zarządzania interfejsu API Azure][] .

![Interfejs API zarządzania dzięki portalowi deweloperów][api-management-developer-portal-menu]

Kliknij pozycję **interfejsy API** w górnym menu, a następnie kliknij **Prostego kalkulatora**.

![Interfejs API echo][api-management-api]

Kliknij pozycję **Wypróbuj** spróbuj **Dodaj dwóch liczb całkowitych** .

![Wypróbuj][api-management-open-console]

Należy pozostawić domyślne wartości parametrów, a następnie wybierz subskrypcję klucz produktu, który ma być używany z **klucza subskrypcji** listy rozwijanej.

Domyślnie w portalu Deweloper **Ocp-Apim-śledzenia** nagłówek jest już ustawiona na **wartość true**. Nagłówek ten określa, czy śledzenie jest generowany.

![Wyślij][api-management-http-get]

Kliknij przycisk **Wyślij** do wywołania tej operacji.

![Wyślij][api-management-send-results]

W odpowiedzi nagłówki zostaną **ocp apim śledzenia lokalizacji** z wartością podobny do następującego przykładu.

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

Śledzenie można pobrać określonej lokalizacji i przejrzeć, jak pokazano w następnym kroku.

## <a name="inspect-trace"> </a>Przeprowadź inspekcję śledzenia

Aby zapoznać się z wartości w wynikach śledzenia, Pobierz plik śledzenia z adresu URL **ocp apim śledzenia lokalizacji** . Jest to plik tekstowy w formacie JSON, a zawiera wpisy podobny do następującego przykładu.

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"> </a>Następne kroki

-   Obejrzyj pokaz śledzenia zasad wyrażeń w [177 obejmuje odcinek chmury: więcej funkcji zarządzania interfejsu API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Przewinąć do 21:00, aby obejrzeć pokaz.

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Wprowadzenie do zarządzania interfejsu API platformy Azure]: api-management-get-started.md
[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 