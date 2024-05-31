Starwars


Popis hry

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




Classy (třídy)
    Classa neboli třída je laicky řečeno jakýsi druh datové struktury, která nám organizuje kód do určitých logických celků. Tyto třídy mohou například reprezentovat herní postavy, překážky, efekty atd. Každá třída může mít atributy (proměnné) a metody (funkce), které definují chování a vlastnosti této třídy.
    V této hře máme 6 tříd (Game, Plaeyr_1, Player_2, Bullet, WhiteStone, RedStone)

Popis class a jejich atributů a metod

> Game -> představuje hlavní herní logiku a funkce pro hru "Starwars" 

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