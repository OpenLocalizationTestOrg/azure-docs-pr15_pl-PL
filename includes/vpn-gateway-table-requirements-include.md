Poniższa tabela zawiera listę wymagań dotyczących PolicyBased i RouteBased sieci VPN bramy. Ta tabela odnosi się do Menedżera zasobów i modeli wdrażania klasyczny. Dla modelu Klasyczny bram PolicyBased VPN taki sam, jak statyczny bram i oparte na rozsyłania bram jest taka sama, jak dynamiczne bram.


|   | **Brama VPN PolicyBased Basic** | **Brama VPN RouteBased Basic** | **Brama VPN RouteBased standardowe**   | **Brama VPN RouteBased wysokiej wydajności** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
|    **Aby witryna łączności (S2S)**  | Konfiguracja sieci VPN PolicyBased        | Konfiguracja sieci VPN RouteBased  | Konfiguracja sieci VPN RouteBased     | Konfiguracja sieci VPN RouteBased    |
| Łączność punktu witryny **(P2S**)      | Brak obsługi   | Obsługiwane (można współistnienie S2S)  | Obsługiwane (można współistnienie S2S)  | Obsługiwane (można współistnienie S2S) |
| **Metody uwierzytelniania**                 |    Klucz wstępny  | Certyfikaty klucza wstępnego łączności S2S P2S połączeń | Certyfikaty klucza wstępnego łączności S2S P2S połączeń | Certyfikaty klucza wstępnego łączności S2S P2S połączeń |
| **Maksymalna liczba połączeń S2S**       | 1                              | 10                                                                    | 10                                | 30                               |
| **Maksymalna liczba połączeń P2S**       | Brak obsługi                  | 128                                                                   | 128                               | 128                              |
|**Aktywne rozsyłania (BGP)**           | Brak obsługi                  | Brak obsługi                                                         | Obsługiwane                     | Obsługiwane                   |
 
