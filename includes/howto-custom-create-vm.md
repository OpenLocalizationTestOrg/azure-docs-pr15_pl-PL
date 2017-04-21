#<a name="how-to-create-a-custom-virtual-machine"></a>Jak utworzyć niestandardowe maszyny wirtualnej

*Niestandardowe* maszyny wirtualnej odwołuje się do maszyny wirtualnej utworzone przy użyciu metody **Z galerii** , ponieważ umożliwia konfigurowanie opcji więcej niż metoda **Szybkiego tworzenia** . Te opcje obejmują:

- Więcej opcji obrazu do tworzenia maszyny wirtualnej (maszyn wirtualnych)
- Nawiązywanie połączenia wirtualnej sieci maszyn wirtualnych
- Dodawanie maszyn wirtualnych do istniejącej usługi w chmurze
- Dodawanie maszyn wirtualnych do zestawu dostępności

> [AZURE.IMPORTANT] Jeśli chcesz, aby komputer wirtualnych o pozwalający łączyć do niego bezpośrednio przez nazwa hosta lub Konfigurowanie połączenia między lokalnej wirtualną sieć, upewnij się, że podczas tworzenia maszyny wirtualnej określić wirtualnej sieci. Aby dołączyć do wirtualnej sieci tylko wtedy, gdy tworzysz maszyny wirtualnej można skonfigurować maszyny wirtualnej. Aby uzyskać więcej informacji na temat wirtualnych sieci zobacz [Omówienie sieci wirtualnej Azure](http://go.microsoft.com/fwlink/p/?LinkID=294063).

1. Zaloguj się do [portalu Azure](http://manage.windowsazure.com).

2. Na pasku poleceń kliknij przycisk **Nowy**.

3. Kliknij pozycję **obliczenia**, kliknij pozycję **maszyn wirtualnych**, a następnie kliknij **Z galerii**.

4. Wybierz obraz, którego chcesz użyć, a następnie kliknij strzałkę, aby kontynuować.

5. Jeśli wiele wersji obrazu są dostępne w **Wersji, Data wydania**, wybierz wersję, którą chcesz użyć.

6. W polu **Nazwa maszyn wirtualnych**wpisz nazwę, która ma być używany dla maszyny wirtualnej.

7. Aby wybrać odpowiedni rozmiar maszyny wirtualnej za pomocą **warstwowych** i **rozmiar** . Rozmiar, który można wybrać wpływa na maksymalną konfigurację maszyny wirtualnej, a także ceny. Aby uzyskać szczegóły konfiguracji zobacz [maszyn wirtualnych i rozmiarów usługi Cloud Azure](http://go.microsoft.com/fwlink/p/?LinkID=389844).

8. W polu **Nazwa nowego użytkownika**wpisz nazwę konta administracyjnego, które ma być używany do zarządzania na serwerze.

9. W polu **Nowe hasło**wpisz silnego hasła dla konta administracyjnego. W polu **Potwierdź hasło**ponownie wpisz to samo hasło.

10. Kliknij strzałkę, aby kontynuować.

11. W **Usłudze w chmurze**wykonaj jedną z następujących czynności:

    - Jeśli jest to pierwszy lub tylko maszyny wirtualnej w usłudze w chmurze, wybierz pozycję **Utwórz nowe usługi w chmurze**. Następnie w polu **Nazwa DNS usługi Cloud**wpisz nazwę, która używa pomiędzy 3 a 24 małe litery, jak i liczb. Ta nazwa staje się częścią identyfikator URI, który jest używany do skontaktuj się z maszyny wirtualnej, za pośrednictwem usługi w chmurze.
    - Jeśli ten maszyn wirtualnych jest dodawana do usługi w chmurze, wybierz go na liście.

    > [AZURE.NOTE] Aby uzyskać więcej informacji o umieszczenie maszyn wirtualnych w tym samym usługi w chmurze zobacz [jak połączyć maszyn wirtualnych w usłudze w chmurze](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/).

12. W **Region i koligacji grupy i wirtualnej sieci**wybierz region, grupa koligacji lub wirtualnej sieci, który ma być używany dla maszyny wirtualnej. Aby uzyskać więcej informacji o grupach koligacji zobacz [informacje o grup koligacji dla wirtualną sieć](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

13. **Konta miejsca do magazynowania**wybierz istniejące konto miejsca do magazynowania dla pliku wirtualnego dysku twardego lub za pomocą konta automatycznie wygenerowanego miejsca do magazynowania. Tylko z jednym klientem miejsca do magazynowania w rozbiciu na regiony jest tworzony automatycznie. Wszystkie inne maszyn wirtualnych utworzone za pomocą tego ustawienia znajdują się na tym koncie miejsca do magazynowania. Jest ograniczona do 20 kont miejsca do magazynowania.

14. Jeśli chcesz, aby maszyny wirtualnej, do której będzie należał zestaw dostępność, **Ustaw dostępność**, wybierz opcję **Utwórz dostępność zestawu** lub Dodaj go do istniejącego zestawu dostępności.

    **Uwaga**: maszyn wirtualnych w zestawie dostępności są rozmieszczane błędów innej domeny. Umieszczenie wiele maszyn wirtualnych w dostępność zestawu pomaga, upewnij się, że aplikacja jest dostępna podczas awarie sieci, awarii dysku sprzętu oraz wszelkie przestojów planowanych.

15.  W obszarze **punkty końcowe**Przejrzyj nowe punkty końcowe utworzone do zezwalania na połączenia do maszyny wirtualnej, takie za pośrednictwem pulpitu zdalnego lub klienta Secure Shell (SSH). Możesz również można teraz dodawać punkty końcowe lub utworzyć je później. Aby uzyskać instrukcje dotyczące tworzenia je później, zobacz [jak się punkty końcowe maszyn wirtualnych](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

16.  W obszarze **Agenta maszyn wirtualnych**zdecyduj, czy zainstalować agenta maszyn wirtualnych. Ten agent oferuje środowisko do zainstalowania rozszerzenia, które mogą ułatwić interakcję z komputerem wirtualnych. Aby uzyskać szczegółowe informacje zobacz [Zarządzanie rozszerzeń](http://go.microsoft.com/FWLink/p/?LinkID=390493).

17. Kliknij strzałkę w celu utworzenia maszyny wirtualnej.

    ![Tworzenie niestandardowego maszyn wirtualnych pomyślnie](./media/howto-custom-create-vm/VMSuccessWindows.png)

##<a name="next-steps"></a>Następne kroki##
Po utworzeniu maszyny wirtualnej jest uruchamiany automatycznie. Gdy portalu jest wyświetlany stan jako uruchomiony, możesz się zalogować maszyn wirtualnych. Aby uzyskać instrukcje zobacz jeden z następujących artykułów:

- [Jak zalogować się do maszyny wirtualnej systemem Linux](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)
- [Jak zalogować się do maszyny wirtualnej z systemem Windows Server](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md)

