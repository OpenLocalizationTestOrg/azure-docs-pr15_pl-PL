**Cel C**: 

1. Na komputerze Mac Otwórz _QSTodoListViewController.m_ w Xcode i dodaj następujące metody. Zmienianie _google_ _microsoftaccount_, _serwisu twitter_, _facebook_i _windowsazureactivedirectory_ , jeśli nie korzystasz z usługi Google jako dostawcy tożsamości. Jeśli korzystasz z serwisu Facebook, [będzie konieczne listy sprawdzonej Facebook domen w aplikacji](https://developers.facebook.com/docs/ios/ios9#whitelist).

            - (void) loginAndGetData
            {
                MSClient *client = self.todoService.client;
                if (client.currentUser != nil) {
                    return;
                }
            
                [client loginWithProvider:@"google" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                }];
            }


2. Zamienianie `[self refresh]` w `viewDidLoad` w _QSTodoListViewController.m_ z następujących czynności:

            [self loginAndGetData];

3. Naciśnij klawisz, _Uruchom_ , aby uruchomić aplikację, a następnie zaloguj się. Gdy logujesz się powinny być widoczne na liście zadania ani uaktualniać.

**Swift**:

1. Na komputerze Mac Otwórz _ToDoTableViewController.swift_ w Xcode i dodaj następujące metody. Zmienianie _google_ _microsoftaccount_, _serwisu twitter_, _facebook_i _windowsazureactivedirectory_ , jeśli nie korzystasz z usługi Google jako dostawcy tożsamości. Jeśli korzystasz z serwisu Facebook, [będzie konieczne listy sprawdzonej Facebook domen w aplikacji](https://developers.facebook.com/docs/ios/ios9#whitelist).
        
            func loginAndGetData() {
                
                guard let client = self.table?.client where client.currentUser == nil else {
                    return
                }
                
                client.loginWithProvider("google", controller: self, animated: true) { (user, error) in
                    self.refreshControl?.beginRefreshing()
                    self.onRefresh(self.refreshControl)
                }
            }


2. Usuwanie linii `self.refreshControl?.beginRefreshing()` i `self.onRefresh(self.refreshControl)` na końcu `viewDidLoad()` w _ToDoTableViewController.swift_. Dodawanie połączenia do `loginAndGetData()` w jego miejscu:

            loginAndGetData()

3. Naciśnij klawisz, _Uruchom_ , aby uruchomić aplikację, a następnie zaloguj się. Gdy logujesz się powinny być widoczne na liście zadania ani uaktualniać.
