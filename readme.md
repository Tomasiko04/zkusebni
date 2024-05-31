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