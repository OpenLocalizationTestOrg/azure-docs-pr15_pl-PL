Strefy DNS służy do przechowywania rekordów DNS dla danej domeny. Aby rozpocząć, hostingu swojej domeny, musisz utworzyć strefy DNS. Rekord DNS utworzone dla konkretnej domeny będzie się wewnątrz strefy DNS dla domeny. 

Na przykład domeny "contoso.com" może zawierać wiele rekordów DNS, na przykład "mail.contoso.com" (dla serwera poczty) i "www.contoso.com" (w przypadku witryny sieci web). 


## <a name="names"></a>Informacje o nazwach strefy DNS
 
- Nazwa strefy musi być unikatowa w grupie zasobów, a strefy nie musi już istnieć. W przeciwnym razie operacja nie powiedzie się.

- Tej samej nazwy strefy można ponownie używać w różnych grupach zasobów lub innej subskrypcji Azure. 

- W przypadku, gdy kilka stref o tej samej nazwie, każde wystąpienie powoduje przypisanie mu inną nazwę adresów serwerów, a tylko jedno wystąpienie można przekazać z domeny nadrzędnej. Aby uzyskać więcej informacji zobacz [pełnomocnika domeny DNS Azure](../articles/dns/dns-domain-delegation.md).