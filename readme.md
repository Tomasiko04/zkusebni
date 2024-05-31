Starwars


>Popis hry

Tato hra je inspirována sci-fi filmy Star Wars. Jedná se o soubojovou arkádovou hru pro dva hráče, ve které se snažíte zničit nepřátelskou vesmírnou loď a přežít co nejdéle. Každý z hráčů má 5 životů, o které může snadno přijít - buď zásahem střelou, kterou proti němu vyšle protivník, nebo pokud se střetne s nepřátelským meteoroidem. Naopak každá z vesmírných lodí je imunní vůči svým meteoroidům, za které se může schovat při útoku nepřítele.

(player_1 => bílá vesmírná loď > imunní proti bílým meteoroidům

player_2 => červená vesmírná loď > imunní proti červeným meteoroidům)


Ovládání hry:

Hráč 1:

    Pohyb: Šipky (nahoru, dolů, vlevo, vpravo)

    Střelba: Mezerník (podle délky stisku klávesy se vystřelí určitý počet střel min.1 max.3)


Hráč 2:

    Pohyb: W, A, S, D

    Střelba: Klávesa "C" (podle délky stisku klávesy se vystřelí určitý počet střel min.1 max.3)

Popis kódu 

V této části, která bude značně nejdelší se Vám pokusím co nejlépe popsat a vysvětlit co daný kód umí.

Na začátku celého kódu musíme importovat několik knihoven (pygame, random, time)

    řádky v kódu 1-3
        import pygame
        import random
        import time


Poté je potřeba inicializovat celou hru pomocí
        
        pygame.init() 
        
určit si barvy a velikost rozlišení obrazovky

    řádky v kódu 8-12
        # Barvy
        tyrkys = "#00fff8"
        # Obrazovka
        width = 1200
        height = 600


a pomocí

    řádek v kódu 13
        screen = pygame.display.set_mode((width, height))
        
si určit velikost rozlišení obrazovky


a pomocí 

    řádek v kódu 14
        pygame.display.set_caption("Starwars")

určit název herního okna


Nastavení hry

    řádky v kódu 8-12       
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