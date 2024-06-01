Starwars


>Popis hry

Tato hra je inspirována sci-fi filmy Star Wars. Jedná se o soubojovou arkádovou hru pro dva hráče, ve které se snažíte zničit nepřátelskou vesmírnou loď a přežít co nejdéle. Každý z hráčů má 5 životů, o které může snadno přijít - buď zásahem střelou, kterou proti němu vyšle protivník, nebo pokud se střetne s nepřátelským meteoroidem. Naopak každá z vesmírných lodí je imunní vůči svým meteoroidům, za které se může schovat při útoku nepřítele.

(player_1 => bílá vesmírná loď > imunní proti bílým meteoroidům

player_2 => červená vesmírná loď > imunní proti červeným meteoroidům)


>Ovládání hry:

Hráč 1:

    Pohyb: Šipky (nahoru, dolů, vlevo, vpravo)

    Střelba: Mezerník (podle délky stisku klávesy se vystřelí určitý počet střel min.1 max.3)


Hráč 2:

    Pohyb: W, A, S, D

    Střelba: Klávesa "C" (podle délky stisku klávesy se vystřelí určitý počet střel min.1 max.3)

>Popis kódu 

V této části, která bude značně nejdelší se Vám pokusím co nejlépe popsat a vysvětlit co daný kód umí.

Na začátku celého kódu musíme importovat několik knihoven (pygame, random, time)

    (řádky v kódu 1-3)
        import pygame
        import random
        import time


Poté je potřeba inicializovat celou hru pomocí
        
    (řádek v kódu 6)
        pygame.init() 
        
určit si barvy a velikost rozlišení obrazovky

    (řádky v kódu 8-12)
        # Barvy
        tyrkys = "#00fff8"
        # Obrazovka
        width = 1200
        height = 600


a pomocí

    (řádek v kódu 13)
        screen = pygame.display.set_mode((width, height))
        
si určit velikost rozlišení obrazovky


a pomocí 

    (řádek v kódu 14)
        pygame.display.set_caption("Starwars")

určit název herního okna


Nastavení hry

    (řádky v kódu 8 až 12)       
        fps = 60
        clock = pygame.time.Clock()

fps=počet snímků za sekundu
pygame.time.Clock() je konstruktor třídy Clock v knihovně Pygame, který vytváří nový objekt Clock je používán k regulaci rychlosti herní smyčky.




>>Classy (třídy)
Classa neboli třída je laicky řečeno jakýsi druh datové struktury, která nám organizuje kód do určitých logických celků. Tyto třídy mohou například reprezentovat herní postavy, překážky, efekty atd. Každá třída může mít atributy (proměnné) a metody (funkce), které definují chování a vlastnosti této třídy.
V této hře máme 6 tříd (Game, Plaeyr_1, Player_2, Bullet, WhiteStone, RedStone)

Popis class a jejich atributů a metod

>>> Game -> představuje hlavní herní logiku a funkce pro hru "Starwars" 

1. Inicializujeme pomocí __init__ () naši class jedná se o tzv. konstruktor, který nastavuje základní atributy (proměnné) dané classy:

    1.1 Herní proměnné 
	    	
            
            (řádky v kódu: 27 až 34)
            self.round_time = 0                             #Čas trvání aktuálního kola hry.
        self.slow_down_cycle = 0                        #Počítadlo cyklů pro zpomalení aktualizace času kola.        
        self.our_player_1 = our_player1             #Odkazy na objekty hráčů.
        self.our_player_2 = our_player2             #Odkazy na objekty hráčů.
        self.group_of_stones = group_of_stones          #Skupina meteoritů.  

        self.last_stone_time = 0                        #Poslední čas vytvoření meteoritu.
        self.stone_cooldown = 10                        #  self.stone_cooldown ŘÍKÁ ZA JAK DLOUHO SE VYGENERUJE DALŠÍ SÉRIE METEORITŮ (časové intervali mezi generování meteriotů)

     1.2 Hudba v pozadí:
		Zde se při zavolání classy před hlavní herní smyčkou načte hudba (řádek: 37) a začne se přehrávat hudba (řádek: 38) poté, aby hudba byla jen v pozadí je pomocí řádku 39 ztlumena 
        *hedba je neoficiálně stažena z youtube pomocí webové stránky: https://en3.onlinevideoconverter.pro/297wG/youtube-converter-mp3 
                

               (řádky v kódu: 37 až 39)             
             #Hudba v pozadí
            pygame.mixer.music.load("media/bg_starwars.mp3")
            pygame.mixer.music.play(-1, 0.0)
            pygame.mixer.music.set_volume(0.05)

    1.3 Obrázky v pozadí: 
        Podobně jako u bodu výše se soubor načte ze složky, kde byl uložen - > poté dané pozadí zmenšíme jak potřebujeme (zde podle velikosti šířky céle obrazovky hry a výšku volíme podle velikosti herního pole kde se nachází kosmické lodě) -> řádek 44 je laicky řečeno  nastaví proměnnou self.background_image_rect na obdélník (rect), který obklopuje celý obrázek self.background_image." -> řádek 45 jedná se o umístění obrázku (v tomto případu je po provedení tohoto řádku kódu levý horní roh obrázku self.background_image umístěn na pozici x=0 a y=100 na obrazovce.)
        *Jinými slovy, tento obdélník bude obsahovat informace o tom, kde se obrázek nachází a jak je velký. Tyto informace jsou užitečné pro umístění obrázku na obrazovku nebo pro detekci kolizí. 

            # Obrázek v pozadí
                (řádky v kódu: 43 až 46 hlavní pozadí na němž se pohybují vesmírné lodě)
            self.background_image = pygame.image.load("img/bg_starwars3.webp")
            self.background_image = pygame.transform.scale(self.background_image, (1250, 400))
            self.background_image_rect = self.background_image.get_rect()
            self.background_image_rect.topleft = (0, 100)

                (řádky v kódu: 48 až 51 pozadí pod hlavním pozadím)
            self.background_space_img = pygame.image.load("img/bg_space.jpg")
            self.background_space_img = pygame.transform.scale(self.background_space_img, (1200, 100))
            self.background_space_img_rect = self.background_space_img.get_rect()
            self.background_space_img_rect.topleft = (0,500)
        
        
    *pozn. Kdyby jste se divili proč je šířka vrchního pozdí 1250 a dolního jen 1200 je to kvůli tomu aby pozadí výše bylo správně vzhledově umístěno a postavě napravo nebyla u “seknuta hlava“ jinak by zde šla šířka pozadí nastavit pod proměnnou width

    1.4 Fonty 
        V této hře jsem vyzkoušel i dva způsoby vkládání textů. Konkrétně se jedná o tzv. systémové a vlastní fonty
        
                Systémové fonty:
                (řádek v kódu: 54 načtení systémového fontu)
            self.stone_font = pygame.font.SysFont("ebrima", 15)
            
                Pokud byste si chtěli vybrat svůj systémový font, napište do nového dokumentu např. ve Visual Studio tento kód níže a v terminálu se vám zobrazí
                
            import pygame
            fonts = pygame.font.get_fonts()
            for x in fonts:
                print(x)

                Vlastní fonty
                (řádky v kódu: 54 až 57 načtení vlastní fontu z internetu odkaz: )
            
            self.custom_font_small=pygame.font.Font("fonts/DeathStar-VmWB.ttf",10)
            self.custom_font_medium=pygame.font.Font("fonts/DeathStar-VmWB.ttf",25)
            self.custom_font_large=pygame.font.Font("fonts/DeathStar-VmWB.ttf",40)

    1.5 Aktualizace hry 
        Tato aktualizace má za úkol, aby při zavolání v této funkce se proměnná round_time zvětšila o 1. Je to důležité jelikož na základě round_time pracuje časomíra, která je zobrazena pod názvem hry, když na sebe mohou vesmírné lodě střílet 
                
                (řádky v kódu: 61 až 65)
            def update(self):
                self.slow_down_cycle += 1
                if self.slow_down_cycle == fps:
                    self.round_time += 1
                    self.slow_down_cycle = 0
    
    1.6 Vykreslení
        Metoda (funkce) draw vykresluje herní prvky na obrazovku 
        1. načtu si barvu > 2. Nastavím si texty > 3. vkreslení do obrazovky 

                1. načtu si barvu (řádky v kódu: 67 až 68)
            
            def draw(self):
                dark_yellow = pygame.Color("#938f0c") 


                2. Nastavím si texty (řádky v kódu: 71 až 107) 
                        #Nastevení textu
                catch_text = self.custom_font_large.render("Star", True, dark_yellow)
                catch_text_rect = catch_text.get_rect()
                catch_text_rect.centerx = width // 2
                catch_text_rect.top = 5
                catch_text1 = self.custom_font_large.render("wars", True, dark_yellow)
                catch_text_rect1 = catch_text1.get_rect()
                catch_text_rect1.centerx = width // 2
                catch_text_rect1.top = 40

                player_text_1 = self.custom_font_medium.render("PLAYER  1", True, dark_yellow)
                player_text_1_rect = player_text_1.get_rect()
                player_text_1_rect.topleft = (10, 4)

                player_text_2 = self.custom_font_medium.render("PLAYER  2", True, dark_yellow)
                player_text_2_rect = player_text_2.get_rect()
                player_text_2_rect.topright = (width - 5, 4)

                lives_text_1 = self.custom_font_medium.render(f"Zivoty: {self.our_player_1.lives}", True, dark_yellow)
                lives_text_rect_1 = lives_text_1.get_rect()
                lives_text_rect_1.topleft = (10, 30)

                lives_text_2 = self.custom_font_medium.render(f"Zivoty: {self.our_player_2.lives}", True, dark_yellow)
                lives_text_rect_2 = lives_text_2.get_rect()
                lives_text_rect_2.topright = (width - 5, 30)

                    #POUZE sekundy
                #time_text = self.stone_font.render(f"Cas hry: {self.round_time}s", True, dark_yellow)
                #time_text_rect = time_text.get_rect()
                #time_text_rect.centerx = width//2
                #time_text_rect.top = 75

                    #Sedkundy a minuty
                minutes = int(self.round_time // 60)
                seconds = int(self.round_time % 60)
                time_text1 = self.stone_font.render(f"Čas: {minutes} min {seconds} s", True, dark_yellow)
                time_text_rect1=time_text1.get_rect()
                time_text_rect1.center=(width // 2 , 85)

                3. vkreslení do obrazovky (řádky v kódu: 110 až 117)
                screen.blit(catch_text, catch_text_rect)
                screen.blit(catch_text1, catch_text_rect1)
                screen.blit(player_text_1, player_text_1_rect)
                screen.blit(player_text_2, player_text_2_rect)
                screen.blit(lives_text_1, lives_text_rect_1)
                screen.blit(lives_text_2, lives_text_rect_2)
                #screen.blit(time_text, time_text_rect)     # Pouze sekundy
                screen.blit(time_text1, time_text_rect1)

                4. + tvary a ohraničení (řádky v kódu: 121 až 122)
                pygame.draw.rect(screen, tyrkys, (0, 100, width, height - 200), 5)
                pygame.draw.line(screen, tyrkys, (width // 2, 105), (width // 2, 495), 5)
    
2.	Kameny (řádek v kódu125 až 141)
Metoda kameny +generuje nové meteority v pravidelných intervalech:
Ze začátku mám zakomentované testování poté pokud proměnná do které se zaznamenává čas (round_time) je při odečtu minulého zaznamenaného času (řádek 33: self.last_stone_time )větší nebo rovna self.stone_cooldown (řádek 34) -> pomocí smyčky for nastaví náhodný počet červených a bílích meteroidů a zavoláme si potřebnou classu do které se zaznamená x(zvoleno), y(náhoda) souřadnice a také se načte příslušný obrázek nakonec se určí typ meteroidu (to jsem si uvedl navíc nemuselo by to v kódu být jako ten typ). Dále jak s tím pracuje daná classa  si povíme níže (RedStone, WhiteStone)
Potom se ještě self_last_stone_time se aktualizuje na self.round_time

           
                (řádek v kódu 125 až 141)
                def kameny(self):
            #current_time = pygame.time.get_ticks()
        #print(self.round_time)
        #print(self.last_stone_time)
        #print(self.stone_cooldown)
        #print(current_time)

        if self.round_time - self.last_stone_time >= self.stone_cooldown:                                                                
            for i in range(random.randint(1,3)):
                self.group_of_stones.add(RedStone(1300, random.randint(120, 470), pygame.image.load("img/red_stone.png"), 0))
                self.group_of_stones.add(WhiteStone(-100, random.randint(120, 470), pygame.image.load("img/white_stone.png"), 1))
                self.last_stone_time = self.round_time
           
            Určil jsem si také velice důležitou podmínku (řádky v kódu: 138 až 141)
            Zaručuje mi, že se nebude neřízeně generovat až moc meteoroidů (vyzkoušej ji odstranit a uvidíš co se stane)

            if self.stone_cooldown > 2:                                        # RYCHLOST PŘIDÁVÁNÍ METEORITŮ
                self.stone_cooldown -= 1                                 # kdyby zde nebyla podmínka tak bys potom neřízeně generovalo až moc rychle 
            else:
                pass     

3. Pauza hry
Metoda pause_game pozastavuje hru a zobrazuje textové zprávy.
V této metodě se nadefinují opět barvy poté si nadefinuje jaký chceceme mít hlavní text, podnadpis a text pro start a jejich umístění. Nakonec si vyplním obrazovku černou barvou a vykreslím(vypálím/vyblituju) daný text a nesmíme zapomenout na update obrazovky

Pod tím následuje smyčka pro pauzu která se v principu podobá hlavní herní smyčce akorát zde se při stisku ENTER pauza nastaví na False a celá hra se spustí 

                (řádky v kódu: 146 až 189)
                #Nastavíme barvy 
        dark_yellow = "#938f0c"
        black=(0,0,0)

        # Hlavní text pro pauzu
        main_text_craete = self.custom_font_large.render(main_text, True, dark_yellow)
        main_text_craete_rect = main_text_craete.get_rect()
        main_text_craete_rect.center = (width//2, height//2 - 60)

        # Podnadpis pro puaznutí
        subheading_text_create = self.custom_font_medium.render(subheading_text,True, dark_yellow)
        subheading_text_create_rect = subheading_text_create.get_rect() 
        subheading_text_create_rect.center = (width//2, height//2 + 20)

        # Pro start
        subheading_text_2_create = self.custom_font_small.render(subheading_text_2,True, dark_yellow)
        subheading_text_2_create_rect = subheading_text_2_create.get_rect() 
        subheading_text_2_create_rect.center = (width//2, height//2 + 60)
        
        #Zobrazení hlavního textu a podnapisu
        screen.fill(black)
        screen.blit(main_text_craete,main_text_craete_rect)
        screen.blit(subheading_text_create,subheading_text_create_rect)
        screen.blit(subheading_text_2_create,subheading_text_2_create_rect)

        pygame.display.update()

        #Zastevení hry 
        
        paused = True
        while paused:
            for one_event in pygame.event.get():
                if one_event.type == pygame.KEYDOWN:
                    if one_event.key == pygame.K_RETURN:
                        paused = False
                
                if one_event.type == pygame.QUIT:
                        paused = False
                        lets_continue = False                                       


>>> Player_1 (Player_2)

Uvedu příklad pouze pro player_1 jelikož player_2 je téměř totžný
Pokusím se vám tyto classy co nejlépe popsat a porovnat 

(SPOLEČNÉ) hned ze začátku jak Player_1 tak i  Player_2 představují hráče ve hře a dědí od pygame.sprite.Sprite (řádky 191 a 245) >co to znamená >znamená to že jak Player_1 tak Player_2 jsou podtřídou třídy Sprite z modulu pygame.sprite. Dědičnost v objektově orientovaném programování umožňuje, aby třída (potomek) zdědila atributy a metody jiné třídy (rodiče). V našem případě bychom to mohli chápat nějak takto:
1.Vlastnosti a Metody pygame.sprite.Sprite:
(řádky v kódu:  )


1. Vlastnosti a Metody (pygame.sprite.Sprite):

pygame.sprite.Sprite poskytuje základní vlastnosti a metody, které jsou užitečné pro práci s herními objekty, jako jsou například sprity. To zahrnuje možnosti pro správu obdélníků (rect), detekci kolizí, vykreslování a aktualizaci stavu objektů.
Tím, že Player_1 dědí od pygame.sprite.Sprite, automaticky získává tyto vlastnosti a metody a může je využívat nebo přepisovat podle potřeby.

2. Skupiny spritů (pygame.sprite.Group):
(řádky v kódu: 191 a 248)
pygame.sprite.Sprite umožňuje instancím (objektům) být přidávány do skupin spritů (pygame.sprite.Group). Skupiny spritů usnadňují správu a manipulaci s kolekcemi spritů. Můžete například aktualizovat všechny sprity ve skupině najednou nebo je všechny najednou vykreslit.
V kódu Player_1 používá skupinu spritů pro střely (self.bullets řádek č. 203 a 259), což usnadňuje jejich správu a aktualizaci.

3. Metoda __init__ v pygame.sprite.Sprite:
Při volání super().__init__() v konstruktoru Player_1 se volá konstruktor rodičovské třídy pygame.sprite.Sprite. To zajišťuje, že jakékoli inicializační kód v pygame.sprite.Sprite je proveden i pro instanci Player_1. 
>>částečně stejné 
(řádky v kódu: 194 až 214 s řádky 250 až 270) jsou zde uvedeny atributy(proměnné)  Player_1 a Player_2, kde největší odlišnost je v tom, že při načítání se načte jiný obrázek (popis načítání a umístění obrázku je popsán v bodě 1.3) (je zde ale navíc použito pygame.transform.rotate(self.image, 270)  který obrázek pootočí o požadovaný počet stupňů)
""
Poté jsou zde doposud nezmíněné atributy jako self.lives = 5 (životy), self.speed = 8 (rychlost vemirných lodí)


>Metoda update(self)
díky této metodě, která je volána do hlavní herní smyčky se hráči ovládají, popis ovadání je poznamenán v bodě OVLÁDÁNÍ HRY, na konci této metody je voláno self.bullets.update()  což aktualizuje všechny střely, které patří do skupiny self.bullets.


            (řádky v kódu 217 až 229)
        def update(self):
            keys = pygame.key.get_pressed()
            if keys[pygame.K_LEFT] and self.rect.left > 605:
                self.rect.x -= self.speed
            if keys[pygame.K_RIGHT] and self.rect.right < width - 10:
                self.rect.x += self.speed
            if keys[pygame.K_UP] and self.rect.top > 108:
                self.rect.y -= self.speed
            if keys[pygame.K_DOWN] and self.rect.bottom < height - 108:
                self.rect.y += self.speed
                #střelba
            if keys[pygame.K_SPACE]:
                self.fire_bullet()
            
            self.bullets.update()


>Metoda fire_bullet(self):

jedná se o jednoduchou metodu kdy při jejím zavolání se current_time nastaví na aktuální čas (zde jsem si chtěl vyzkoušet počítat čas podle metody pygame.time.get_ticks(), která je součástí knihovny  Pygame.) Tento čas se počítá od momentu, kdy byla volána funkce pygame.init() (řádek v kódu 6). Tato metoda vrací počet milisekund, které uplynuly od doby, kdy byla Pygame inicializována. > potom se počítá množství vystřených střel a zároveň musí být splněna podmínka, která hráči umožňuje po uplynutí určité doby vystřelit, tak aby hráč nestřílel neustále, a poté se po každém výstřelu přehraje zvuk střely. 


            (řádky v kódu 235 až 241)
        def fire_bullet(self):
            current_time = pygame.time.get_ticks()
            if len(self.bullets) < 3 and current_time - self.last_shot_time >= self.shot_cooldown:
                bullet = Bullet(self.rect.left, self.rect.centery, -10, shooter=1)
                self.bullets.add(bullet)
                self.last_shot_time = current_time
                self.zap_sound.play()

>Metoda draw(self, surface):

Další a zároveň poslední metodou v classe Game je draw, která vykreslí obrázek hráče na daný povrch na pozici danou jeho obdélníkem (rect) a poté vykreslí střely na daný povrch.
            

            (řádky v kódu 243 až 245)
        def draw(self, surface):
        surface.blit(self.image, self.rect)
        self.bullets.draw(surface)

>>> Bullet

podobně jako u Plaeyer_1 a Player_2 
dědí od pygame.sprite.Sprite (řádek v kódu 302 Player_1) >co to znamená >znamená to že Bullet je podtřídou třídy Sprite z modulu pygame.sprite. Dědičnost v objektově orientovaném programování umožňuje, aby třída (potomek) zdědila atributy a metody jiné třídy (rodiče). V konstruktoru jsme si definovali i souřednice x, y a poté i speed(rychlost pohybu hráče) a shooter(druh střelce)
self.image = pygame.Surface((15, 5)) je velikost střely a poté pomocí podmínky když známe typ shootra vykreslí se nám pomocí podmínky příslušná barva střel > poté převedeme na rect a umístíme danou střelu podle daných x a y souřadnic 

>>>>Metoda update(self):
>Aktualizace pozice:
Přidává hodnotu speed k souřadnici x obdélníku (rect), což způsobuje pohyb střely ve směru její rychlosti.

            řádek v kódu 321
            self.rect.x += self.speed

>Odstranění střely, pokud opustí obrazovku:
Pokud střela opustí obrazovku (její pravý okraj je menší než 0 nebo její levý okraj je větší než šířka obrazovky width), zavolá se metoda kill(), která odstraní střelu ze všech skupin spriteů, ve kterých je.

            řádek v kódu 322 a 323 
            if self.rect.right < 0 or self.rect.left > width:
                self.kill()

>>>RedStone (WhiteStone tato třída je téměř stejná až na to že se x souřadnice přičítá)

Tato classa představuje ve hře červený kámen/meteoroid, který se pohybuje z levé strany na levou stranu hracího pole. U classy WhiteStone se pohybuje bílý kámen/meteoroid pohybující se pravé strany na levou což je jediný rozdíl co tyto classy odlišuje (u classy RedStone se odečítá x a u classy WhiteStone se x odečítá) Obě dvě tyto classy dědí od pygame.sprite.Sprite stejně jako například Player_1.

        řádky v kódu 325 až 335

    class RedStone(pygame.sprite.Sprite):
        def __init__(self, x, y, image, stone_type):
            super().__init__()
            self.image = image
            self.rect = self.image.get_rect()
            self.rect.center = (x, y)
            self.type = stone_type
            self.speed = random.randint(1, 5)
                #putování stone
        def update(self):
            self.rect.x -= self.speed

>Jako první si vytvoříme konstruktor, co daná classa bude obsahovat 

        (řádek v kódu: 326)
        def __init__(self, x, y, image, stone_type):


>>x, y: Počáteční souřadnice středu kamene.
>>image: Obrázek kamene, který bude zobrazen.
>>stone_type: Typ kamene, který může ovlivňovat jeho vlastnosti nebo chování.

>Inicializace rodičovské třídy: 

        (řádek v kódu 329)
        super().__init__()

>Nastavení obrázku kamene a uložení obrázku, který se použije při vykreslování:
        
        (řádek v kódu 328)
        self.image = image

>Uložení obrázku, který se použije při vykreslování:

        (řádek v kódu 329)
        self.rect = self.image.get_rect()

>Nastavení rect pro pozicování:
Získá rect pro obrázek a nastaví jeho středové souřadnice na (x, y). Tím se určí počáteční pozice kamene na obrazovce.


        (řádky v kódu 329 a 330)
        self.rect = self.image.get_rect()
        self.rect.center = (x, y)

>Nastavení typu kamene (to jsme si vytvořil pro možné rozšíření kódu)
        
        
        (řádek v kódu 331)
        self.type = stone_type

>Nastavení rychlosti kamene
Nastaví náhodnou rychlost kamene v rozmezí od 1 do 5. To určuje, jak rychle se kámen pohybuje po obrazovce.

        (řádek v kódu 332)
        self.speed = random.randint(1, 5)

>6.Metoda update(self) 
Metoda, která se volá každým snímkem pro aktualizaci stavu kamene. U classy RedStone se self.speed odečítá od polohy obrázku, kde se zrovna nachází a pohybuje se směrem doleva. Naopak u classy WhiteStone je to naopak zde se self.speed přičítá a kámen se tedy pohybuje doprava.

        (řádky v kódu 334 a 335)
        def update(self):
            self.rect.x -= self.speed


Pod tímto by mělo být tzv. použití logiky, kterou jsme si výše udělali a v kódu níže se bude jen volat nad a do hlavní herní smyčky. Jelikož je to moje první hra v pygame zkusil jsem přidat i nějakou herní logiku do a pod hlavní herní smyčku, jelikož určité internetové zdroje uvádí. Že t není špatně, psát herní logiku do části, kde se nachází hlavní herní smyčka, ale není to přehledné, proto se doporučuje využít objektově orientovaného programování.

        
>Před hlavní herní smyčkou 

>>Vytvoříme si dvě skupiny jednu pro hráče a druhou pro meteoroidy 

        (řádky v kódu 354 a 357)  
        
        stone_group = pygame.sprite.Group()
        player_group = pygame.sprite.Group()

>>Do skupin hráčů přidáme hráče

        (řádky v kódu 358 až 360)
        one_player_1 = Player_1()
        one_player_2 = Player_2()
>>Vytvoření objektu Game:

je inicializována s hráči one_player_1 a one_player_2 a skupinou stone_group.


