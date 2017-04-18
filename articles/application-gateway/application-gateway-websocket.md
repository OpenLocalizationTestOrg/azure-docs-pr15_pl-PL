<properties
   pageTitle="Obsługa aplikacji bramy WebSocket | Microsoft Azure"
   description="Ta strona zawiera omówienie obsługi aplikacji WebSocket bramy."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>Obsługa aplikacji WebSocket bramy

Brama aplikacji obsługuje natywne WebSocket przez wszystkich rozmiarach bramy. Istnieje nie można konfigurować użytkownika ustawienie selektywne włączenie lub wyłączenie WebSocket pomocy technicznej. Możesz kontynuować korzystanie z standardowy HTTPListener na porcie 80 i 443, aby odbierać WebSocket danych. Ruch WebSocket następnie jest przekierowywany do serwera enabled wewnętrznej bazy danych WebSocket, zgodnie z regułami bramy aplikacji przy użyciu puli odpowiednie wewnętrznej bazy danych. Protokół WebSocket ustandaryzowane w [RFC6455](https://tools.ietf.org/html/rfc6455) umożliwia pełny dupleks komunikacji między klienta i serwera przez długotrwałych połączeń TCP. Ta funkcja umożliwia interakcje komunikacji między komputerami serwerów sieci web i klienta, który może być dwukierunkowy bez potrzeby ankieta jako wymagane w implementacji oparte na HTTP.  WebSocket małych jest obciążenie w przeciwieństwie do protokołu HTTP i można ponownie użyć tego samego połączenia TCP wielu żądania i odpowiedzi, uzyskując efektywniejsze wykorzystania zasobów. Protokoły WebSocket jest przeznaczona do pracy nad tradycyjnych HTTP porty 80 i 443.

Serwer wewnętrznej bazy danych musi odpowiadać na sondy bramy aplikacji opisane w sekcji [Omówienie sondy kondycji](application-gateway-probe-overview.md) . Sondy kondycji bramy aplikacji są tylko protokołu HTTP/HTTPS, oznacza to, że każdy serwer wewnętrznej bazy danych musi odpowiadać na sondy HTTP dla aplikacji bramy skierować ruch WebSocket na serwerze.

## <a name="listener-configuration-element"></a>Element konfiguracji odbiornika

Istniejące HTTPListener może służyć do obsługi WebSocket. Oto fragment elementu HttpListeners z przykładowego pliku szablonu. Może być konieczne detektory HTTP i HTTPS do obsługi WebSocket i zabezpieczanie ruchu WebSocket. Podobnie możesz używać [portalu](application-gateway-create-gateway-portal.md) lub [programu PowerShell](application-gateway-create-gateway-arm.md) na tworzenie bramy aplikacji detektory na porcie 80 i 443 do obsługi ruchu sieciowego WebSocket.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool, BackendHttpSetting i trasowanie konfiguracji reguły

BackendAddressPool należy zdefiniować puli wewnętrznej bazy danych z serwerami WebSocket włączone. Z wewnętrznej bazy danych należy zdefiniować BackendHttpSetting port 80 i 443. Właściwości koligacji systemem plików cookie i requestTimeouts nie są odpowiednie dla ruchu WebSocket. Istnieje zmiany nie są wymagane w regule routingu. Reguły routingu, że "Podstawowe" powinny być nadal stosowane powiązać odpowiednie odbiornika do odpowiedniej puli adresów wewnętrznej bazy danych. 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>WebSocket włączone wewnętrznej bazy danych

Do wewnętrznej bazy danych musi być serwerem sieci web protokołu HTTP/HTTPS na skonfigurowany port (zazwyczaj 80 i 443) dla WebSocket do pracy. To wymaganie dotyczy, ponieważ protokół WebSocket wymaga początkowe uzgadnianie być HTTP uaktualnienie protokołu WebSocket jako pola nagłówka.

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Innym powodem tego jest sonda zdrowia tej aplikacji bramy wewnętrznej bazy danych obsługuje tylko protokoły protokołu HTTP/HTTPS. Jeśli serwera wewnętrznej bazy danych nie odpowiada protokołu HTTP/HTTPS sondy, czy należy wziąć pod wykorzystać puli wewnętrznej bazy danych i nie żądania, w tym także żądania WebSocket, czy osiągnięcia tej wewnętrznej bazy danych.

## <a name="next-steps"></a>Następne kroki

Przejdź do [Tworzenie bramy aplikacji](application-gateway-create-gateway.md) , aby rozpocząć pracę z WebSocket włączane po szkoleniowe na temat pomocy technicznej WebSocket aplikacji sieci web.
