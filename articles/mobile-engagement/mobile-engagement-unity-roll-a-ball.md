<properties
    pageTitle="Rzut jedności samouczek "mina""
    description="Procedura tworzenia klasyczny jedności użycia nożna "mina", która jest wstępnie wymagane dla wszystkich samouczków jedności zaangażowania Mobile"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a id="unity-roll-a-ball"></a>Tworzenie użycia jedności piłka nożna

Ten samouczek opisano główne kroki dla nieco zmodyfikowanym [rzut jedności samouczek "mina"](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Ten przykładowy gry składa się z obiektu okrągłe player, który jest kontrolowane przez użytkownika aplikacji i celem gry jest "zbieranie" kolekcjonowanych obiektów przez kolizji obiektu odtwarzacza z tymi obiektami kolekcjonowanych. Podstawowe znajomości środowiska edytora jedności przy założeniu. Jeśli napotkasz jakiekolwiek problemy należy zajrzyj do pełnego samouczka. 

### <a name="setting-up-the-game"></a>Aby skonfigurować gry
Poniższe czynności są z [samouczka jedności](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Otwórz **Edytor jedności** , a następnie kliknij przycisk **Nowy**. 
    
    ![][51] 
    
2. Podaj **nazwę projektu** & **lokalizacji**, zaznacz **3-w** i kliknij pozycję **Utwórz projekt**.
    
    ![][52]

3. Zapisywanie sceny domyślne właśnie utworzony w ramach nowego projektu zgodnie z nazwą **MiniGame** poziomu nowego ** \_scen** folder w folderze **elementy zawartości** :
    
    ![][53]

4. Tworzenie **obiektu 3D -> płaszczyzny** jako pola gry i Zmień nazwę tego obiektu płaszczyzny jako **ziemi**

    ![][1]

5. Resetowanie składnika przekształcenie dla tego obiektu **podłożem** , tak aby znajdował się na początku. 

    ![][3]

6. Wyczyść pole wyboru **Pokaż siatkę** z **Gizmos menu** obiektu **ziemi** .

    ![][4]

7. Aktualizowanie składnika **Skala** dla obiektu **podłożem** ma być [X = 2, Y = 1, Z = 2]. 

    ![][5]

8. Dodawanie nowego **obiektu 3D -> kula** do projektu i Zmień nazwę tego obiektu kula jako **odtwarzacza**. 

    ![][6]

9. Zaznacz obiekt **odtwarzacza** i kliknij przycisk **Resetuj Przekształcanie** podobny do obiektu płaszczyzny. 

10. Aktualizacja **Położenie -> Przekształcenie -> współrzędnych Y** składnik Y odtwarzacza jako 0,5.  

    ![][7]

11. Tworzenie nowego folderu o nazwie **materiałów** w projekcie miejsce, w którym zostanie utworzony kolor odtwarzacza materiał. 

12. Tworzenie nowego **materiału** zatytułowanego **tła** w tym folderze. 

    ![][8]

13. Aktualizowanie koloru materiału aktualizując właściwość **Albedo** go. Można wybierać wartości RGB [0,32,64]. 

    ![][9]

14. Przeciągnij ten materiał do widoku sceny, aby zastosować kolor do obiektu **ziemi** . 

    ![][10]

17. Na koniec aktualizowanie **Przekształcenie -> Obrót -> Y** do 60 na obiekcie światła dla jasności. 

    ![][12]

### <a name="moving-the-player"></a>Przenoszenie odtwarzacza
Poniższe czynności są z [samouczka jedności](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Dodawanie składnika **RigidBody** do obiektu **odtwarzacza** . 

    ![][13]

2. Tworzenie nowego folderu o nazwie **skryptów** w projekcie. 

3. Kliknij pozycję **dodać składnik -> Nowy skrypt -> C# skrypt**. Nadaj mu nazwę **PlayerController**, a następnie kliknij pozycję **Utwórz i Dodaj**. To utworzy i dołączyć skrypt do obiektu odtwarzacza.  

    ![][14]

5. Przenoszenie tego skryptu w folderze **skryptów** w projekcie. 

6. Otwórz skrypt do edycji w edytorze Ulubione skrypt, zaktualizować kod skryptu poniższy kod, a następnie zapisz go. 

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
    
8. Należy zauważyć, że skrypt powyżej używa właściwości **szybkości** . W edytorze jedności zaktualizuj właściwość szybkości 10.  

    ![][15]

9. Wybierz przycisk **odtwarzania** w edytorze jedności. Teraz powinno być możliwe do sterowania mina przy użyciu klawiatury i należy obracanie i poruszanie się po. 

### <a name="moving-the-camera"></a>Przenoszenie aparatu fotograficznego
Poniższe kroki pochodzą z [samouczka jedności](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) i będzie wiążą **Główne kamery** do obiektu **odtwarzacza** . 

1. Aktualizowanie **Transform.Position** być X = 0, Y = 10.5, Z =-10.  
2. Aktualizowanie **Transform.Rotation** być X = 45, Y = 0, Z = 0.  

    ![][16]

2. Dodaj nowy skrypt o nazwie **CameraController** do **MainCamera** , a następnie umieść ją w folderze skryptów. 

    ![][17]

3. Otwarcie skrypt do edycji i Dodaj następujący kod w niej:

        using UnityEngine;
        using System.Collections;
        
        public class CameraController : MonoBehaviour {
        
            public GameObject player;
        
            private Vector3 offset;
        
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
            
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
    
5. W środowisku jedności — przeciągnij zmiennej odtwarzacza do przedział odtwarzacza obiektu główne kamery, tak aby dwóch są skojarzone ze sobą. 

    ![][18]

6. Jeśli trafienie odtwarzania w edytorze jedności i obracanie obiektu mina odtwarzacza następnie zostanie wyświetlone kamery w ruchu.  

### <a name="setting-up-the-play-area"></a>Aby skonfigurować obszaru odtwarzania
Poniższe kroki są z [samouczka jedności](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). Zostanie utworzony ściany otaczającego ziemi tak, aby obiekt mina odtwarzacza nie rozdzielni obszaru odtwarzania w ruchu. 

1. Kliknij pozycję **Tworzenie -> Utwórz pusty -> obiekt gry** i nadaj mu nazwę **ścian**

    ![][19]

2. Tego obiektu ściany — Tworzenie nowego **modułu -> 3-w obiektu** i nadaj mu nazwę "Zachód ścian". 

    ![][20]

3. Zaktualizuj **Położenie -> Przekształcenie** i **Przekształcenie -> Skala** dla tego obiektu ścian Zachód. 

    ![][21]

4. Duplikowanie ścian zachód na tworzenie **ścian wschód** przekształcenie zaktualizowana położenia i Skala. 

    ![][22]

5. Duplikowanie Ściana wschód na tworzenie **Ściana Północna** przekształcenie zaktualizowana położenia i Skala. 

    ![][23]

6. Duplikowanie ścian Ameryka Północna i Utwórz **ścian Południowej** przy użyciu przekształcenie zaktualizowana położenia i Skala. 

    ![][24]

### <a name="creating-collectible-objects"></a>Tworzenie kolekcjonowanych obiektów
Poniższe kroki są z [samouczka jedności](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). Zostanie utworzony niektórych atrakcyjnego obiektów wyglądających stanowiące zestawu kolekcjonowanych obiektów, które obiekt mina odtwarzacza musi zbierać, przez kolizji z nimi. 

1. Utwórz nowy **obiekt sześcian 3-w** i nadaj mu nazwę pobrania. 

2. Dostosowywanie **Obrót -> Przekształcenie** & **Skala -> Przekształcenie** pobrania obiektu. 

    ![][25]

3. Utwórz i Dołącz **Nowe C# skrypt** o nazwie **Rotator** do pobrania obiektu. Upewnij się umieścić skrypt w folderze skryptów. 

    ![][26]

4. Otwórz ten skrypt do edycji i aktualizowanie się następujące czynności: 

        using UnityEngine;
        using System.Collections;
        
        public class Rotator : MonoBehaviour {
        
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }

5. Teraz trafienie trybu odtwarzania w edytorze jedności i pokazu pobrania obiektu można obracając na osi.

6. Tworzenie nowego folderu o nazwie **Prefabs** 

    ![][27]

7. Przeciągnij obiekt **pobrania** i umieszczanie go w folderze Prefabs.

    ![][28]

8. Tworzenie nowego **pustego nożna obiektu** o nazwie **Pickups**. Resetowanie położenia na początek, a następnie przeciągnij obiekt pobrania tego gier obiektu.  

    ![][29]

9. Duplikowanie obiektu **pobrania** i utworzyć na obiekcie **podłożem** wokół obiektu **odtwarzacza** aktualizując wartości **X w Transform.Position i Z** prawidłowo. 

    ![][30]

10. Tworzenie **nowego materiału** o nazwie **pobrania** i aktualizowanie należy czerwonym kolorem aktualizując **Właściwości Albedo** podobne do nas o aktualizację obiektu ziemi. 

    ![][31]

11. Stosowanie materiały do 4 pobrania obiektów.

    ![][32]

### <a name="collecting-the-pickup-objects"></a>Zbieranie obiektów poczty
Poniższe kroki są z [samouczka jedności](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Windows Media Player zostanie odpowiednio zaktualizowana tak, aby mogła "zbieranie" pobrania obiektów przez kolizji z nimi. 

1. Otwarcie skrypt **PlayerController** dołączony do obiektu odtwarzacza do edycji i zaktualizować go do następujących czynności:  

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour {
        
            public float speed;
        
            private Rigidbody rb;
        
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
        
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
        
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
        
                rb.AddForce (movement * speed);
            }
        
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }

2. Utwórz nowy **znacznik** o nazwie **Wybierz pozycję w górę** (musi być zgodna co to jest skryptu)  

    ![][33]
    
    ![][34]

3. Stosowanie **znacznika** do obiektu Prefab pobrania. 

    ![][35]

4. Pole wyboru **IsTrigger** obiektu Prefab Włącz.

    ![][36]

5. Dodawanie sztywne treści do pobrania Prefab obiektu. Aby zoptymalizować wydajność collider statyczny, który użyliśmy zostanie odpowiednio zaktualizowana do dynamicznego collider. 

    ![][37]
  
6. Na koniec sprawdź właściwość **IsKinematic** prefab obiektu. 

    ![][38]

7. Trafień **odtwarzania** w edytorze jedności i będą mogli odtworzyć ten **użycia piłka** nożna, przenosząc obiekt Player, za pomocą klawiszy na klawiaturze podanie danych wejściowych kierunku. 

### <a name="updating-the-game-for-mobile-play"></a>Aktualizowanie gry dla urządzeń przenośnych odtwarzania
Powyżej zawarta podstawowe samouczek z jedności. Teraz możemy będzie zmodyfikować nożna, aby był urządzenia przenośnego przyjazny. Pamiętaj, że możemy użyć klawiatury dla gry pory do testowania. Teraz możemy będzie zmodyfikuj ją tak, aby odtwarzacza można sterować przy użyciu ruchu telefonu, to znaczy Używanie mieści się jako danych wejściowych. 

Otwarcie **PlayerController** skrypt do edycji i aktualizować metody **FixedUpdate** , aby przenieść obiekt odtwarzacza za pomocą ruchu z mieści się. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Ten samouczek zawiera podstawowe tworzenia gier z jedności i umieszczaniu to na urządzeniu wybór gry. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png  
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png  
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png

    
    
    
    
    
    
    
    
    
    
    
    
