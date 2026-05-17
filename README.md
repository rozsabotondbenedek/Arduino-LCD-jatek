# Futó ember LCD játék

## Projekt leírása
A „Futó ember LCD játék” egy egyszerű, de szórakoztató akadályugratós játék Arduino platformra. A játék lényege, hogy egy futó emberkét irányítunk egy 16x2 karakteres LCD kijelzőn, aki folyamatosan jobbról balra görgető pályán fut. A játékos feladata, hogy egy gomb megnyomásával időben átugorja a közeledő akadályokat (tömör földblokkokat). Ha a futó ember nekiütközik egy akadálynak, a játék véget ér. A pontszám a megtett távolsággal arányosan növekszik.

### Képernyőképek

<img width="1876" height="975" alt="image" src="https://github.com/user-attachments/assets/f9e16b92-5070-4896-9b97-18f88d1220b5" />
<img width="1607" height="974" alt="image" src="https://github.com/user-attachments/assets/15e244ff-c622-4e66-b253-bae0698c8b6e" />


---

## Projekt elérhetősége
A projekt logikai szimulációja és a menürendszer alapjainak tesztelése a TinkerCAD platformon készült.

* TinkerCAD Projekt: https://www.tinkercad.com/things/4C1szHQpWr2-futo-ember-lcd-jatek?sharecode=21DBikm690Beg-QKeUvHQBRCv8oKDEfz4UVXIXV-aEI

---


## Működése

### A játék indítása
- Bekapcsolás után a kijelzőn egy villogó **"Press Start"** felirat jelenik meg
- A felhasználó megnyomja a gombot → ezzel elindítja a játékot

### Irányítás
- A játékos **egyetlen gombot** használ az irányításhoz
- A gomb megnyomására a futó emberke **felugrik**
- **Nincs más vezérlés** – a karakter automatikusan fut jobbról balra

### A játék menete
1. A karakter folyamatosan fut az alsó sorban
2. Jobb oldalról véletlenszerű időközönként **földblokkok (akadályok)** érkeznek
3. A felhasználónak **időben meg kell nyomnia a gombot** hogy átugorja az akadályt
4. Ha sikeresen átugrik → a játék folytatódik, nő a pontszám
5. Ha **nekimegy az akadálynak** → a játék véget ér

### Pontszám
- A **jobb felső sarokban** látható a megtett távolsággal arányos pontszám
- Minél tovább fut a karakter, annál több pontot gyűjt
- Játék vége után újrakezdéskor a pontszám nulláról indul

## Alkatrészlista

| Alkotóelem | Mennyiség | Leírás |
|------------|-----------|--------|
| Arduino Uno (klón) | 1 | Vezérlőegység |
| LCD 16x2 karakteres | 1 | Kijelző (HD44780 kompatibilis) |
| Nyomógomb | 1 | Ugrás vezérléséhez |
| Potenciométer (10kΩ) | 1 | LCD kontrasztállításhoz |
| Ellenállás (220Ω) | 1 | LCD háttérvilágításhoz |
| Breadboard | 1 | Próbapanel az összekötésekhez |
| Dupont vezetékek | több | Áthidaló kábelek (férfi-férfi) |

## Bekötési táblázat

| LCD láb | Arduino láb | Megjegyzés |
|---------|-------------|------------|
| RS | 11 | Register Select |
| E | 9 | Enable |
| D4 | 6 | Adatbusz 4. bit |
| D5 | 5 | Adatbusz 5. bit |
| D6 | 4 | Adatbusz 6. bit |
| D7 | 3 | Adatbusz 7. bit |
| R/W | 10 | Írás/olvasás (földelve = csak írás) |
| V0 | Potenciométer | Kontrasztállítás (középső láb) |
| Vcc | 5V | Tápfeszültség |
| GND | GND | Föld |
| A (Anód) | 220Ω → 5V | Háttérvilágítás (+) |
| K (Katód) | GND | Háttérvilágítás (-) |

## Egyéb csatlakozások

| Alkatrész | Arduino láb | Megjegyzés |
|-----------|-------------|------------|
| Nyomógomb (egyik láb) | 2 | Belső felhúzó ellenállással |
| Nyomógomb (másik láb) | GND | Földre kötve |
| PIN_AUTOPLAY | 1 | Automatikus játék jelző kimenet |
| PIN_CONTRAST | 12 | Kontraszt vezérlő (földelve) |
---
## Változók:
| Változó neve | Típus | Elérhetőség | Leírás |
|--------------|-------|-------------|--------|
| `terrainUpper` | `char[17]` | globális | Felső sor terepe (16 karakter + lezáró null) |
| `terrainLower` | `char[17]` | globális | Alsó sor terepe |
| `buttonPushed` | `bool` | globális | Gombnyomás jelzője (megszakításból) |
| `lcd` | `LiquidCrystal` | globális | LCD objektum |
| `heroPos` | `byte` | statikus (loop) | Hős aktuális pozícióállapota |
| `newTerrainType` | `byte` | statikus (loop) | Következő tereptípus (üres/felső/alsó blokk) |
| `newTerrainDuration` | `byte` | statikus (loop) | Jelenlegi tereptípus hátralévő ciklusai |
| `playing` | `bool` | statikus (loop) | Játék állapota (true = fut, false = game over) |
| `blink` | `bool` | statikus (loop) | Villogás állapota a kezdőképernyőn |
| `distance` | `unsigned int` | statikus (loop) | Megtett távolság (pontszám alapja) |

| Makró neve | Érték | Leírás |
|------------|-------|--------|
| `PIN_BUTTON` | 2 | Gomb láb száma |
| `PIN_AUTOPLAY` | 1 | Automatikus játék jelző lába |
| `PIN_READWRITE` | 10 | LCD RW lába |
| `PIN_CONTRAST` | 12 | Kontraszt beállító lába |
| `SPRITE_RUN1` | 1 | Futás sprite 1 |
| `SPRITE_RUN2` | 2 | Futás sprite 2 |
| `SPRITE_JUMP` | 3 | Ugrás sprite |
| `SPRITE_JUMP_UPPER` | '.' | Pont karakter a fejhez ugráskor |
| `SPRITE_JUMP_LOWER` | 4 | Ugrás alsó része |
| `SPRITE_TERRAIN_EMPTY` | ' ' | Üres terület (szóköz karakter) |
| `SPRITE_TERRAIN_SOLID` | 5 | Tömör földblokk |
| `SPRITE_TERRAIN_SOLID_RIGHT` | 6 | Földblokk jobb széle |
| `SPRITE_TERRAIN_SOLID_LEFT` | 7 | Földblokk bal széle |
| `HERO_HORIZONTAL_POSITION` | 1 | A hős vízszintes pozíciója a képernyőn |
| `TERRAIN_WIDTH` | 16 | A pálya szélessége karakterekben |
| `TERRAIN_EMPTY` | 0 | Üres tereptípus |
| `TERRAIN_LOWER_BLOCK` | 1 | Alsó sorban lévő blokk |
| `TERRAIN_UPPER_BLOCK` | 2 | Felső sorban lévő blokk |
| `HERO_POSITION_OFF` | 0 | A hős láthatatlan |
| `HERO_POSITION_RUN_LOWER_1` | 1 | A hős az alsó sorban fut (1. póz) |
| `HERO_POSITION_RUN_LOWER_2` | 2 | A hős az alsó sorban fut (2. póz) |
| `HERO_POSITION_JUMP_1` | 3 | Ugrás kezdete |
| `HERO_POSITION_JUMP_2` | 4 | Félig felfelé |
| `HERO_POSITION_JUMP_3` | 5 | Ugrás a felső sorban |
| `HERO_POSITION_JUMP_4` | 6 | Ugrás a felső sorban |
| `HERO_POSITION_JUMP_5` | 7 | Ugrás a felső sorban |
| `HERO_POSITION_JUMP_6` | 8 | Ugrás a felső sorban |
| `HERO_POSITION_JUMP_7` | 9 | Félig lefelé |
| `HERO_POSITION_JUMP_8` | 10 | Éppen landolás előtt |
| `HERO_POSITION_RUN_UPPER_1` | 11 | A hős a felső sorban fut (1. póz) |
| `HERO_POSITION_RUN_UPPER_2` | 12 | A hős a felső sorban fut (2. póz) |

---

## Forráskód
```cpp
#include <LiquidCrystal.h>

#define PIN_BUTTON 2           // Gomb láb száma
#define PIN_AUTOPLAY 1         // Automatikus játék jelző lába
#define PIN_READWRITE 10       // LCD RW lába
#define PIN_CONTRAST 12        // Kontraszt beállító lába

#define SPRITE_RUN1 1          // Futás sprite 1
#define SPRITE_RUN2 2          // Futás sprite 2
#define SPRITE_JUMP 3          // Ugrás sprite
#define SPRITE_JUMP_UPPER '.'  // Pont karakter a fejhez ugráskor
#define SPRITE_JUMP_LOWER 4    // Ugrás alsó része
#define SPRITE_TERRAIN_EMPTY ' '   // Üres terület (szóköz karakter)
#define SPRITE_TERRAIN_SOLID 5     // Tömör földblokk
#define SPRITE_TERRAIN_SOLID_RIGHT 6  // Földblokk jobb széle
#define SPRITE_TERRAIN_SOLID_LEFT 7   // Földblokk bal széle

#define HERO_HORIZONTAL_POSITION 1    // A hős vízszintes pozíciója a képernyőn

#define TERRAIN_WIDTH 16       // A pálya szélessége karakterekben
#define TERRAIN_EMPTY 0        // Üres tereptípus
#define TERRAIN_LOWER_BLOCK 1  // Alsó sorban lévő blokk
#define TERRAIN_UPPER_BLOCK 2  // Felső sorban lévő blokk

#define HERO_POSITION_OFF 0          // A hős láthatatlan
#define HERO_POSITION_RUN_LOWER_1 1  // A hős az alsó sorban fut (1. póz)
#define HERO_POSITION_RUN_LOWER_2 2  // A hős az alsó sorban fut (2. póz)
#define HERO_POSITION_JUMP_1 3       // Ugrás kezdete
#define HERO_POSITION_JUMP_2 4       // Félig felfelé
#define HERO_POSITION_JUMP_3 5       // Ugrás a felső sorban
#define HERO_POSITION_JUMP_4 6       // Ugrás a felső sorban
#define HERO_POSITION_JUMP_5 7       // Ugrás a felső sorban
#define HERO_POSITION_JUMP_6 8       // Ugrás a felső sorban
#define HERO_POSITION_JUMP_7 9       // Félig lefelé
#define HERO_POSITION_JUMP_8 10      // Éppen landolás előtt
#define HERO_POSITION_RUN_UPPER_1 11 // A hős a felső sorban fut (1. póz)
#define HERO_POSITION_RUN_UPPER_2 12 // A hős a felső sorban fut (2. póz)

// LCD inicializálása: (RS, E, D4, D5, D6, D7)
LiquidCrystal lcd(11, 9, 6, 5, 4, 3);
static char terrainUpper[TERRAIN_WIDTH + 1];  // Felső sor terepe
static char terrainLower[TERRAIN_WIDTH + 1];  // Alsó sor terepe
static bool buttonPushed = false;              // Gombnyomás jelző

// Egyedi karakterek inicializálása
void initializeGraphics() {
  static byte graphics[] = {
    // Futás 1. póz
    B01100, B01100, B00000, B01110,
    B11100, B01100, B11010, B10011,
    // Futás 2. póz
    B01100, B01100, B00000, B01100,
    B01100, B01100, B01100, B01110,
    // Ugrás
    B01100, B01100, B00000, B11110,
    B01101, B11111, B10000, B00000,
    // Ugrás alsó része
    B11110, B01101, B11111, B10000,
    B00000, B00000, B00000, B00000,
    // Talaj (tömör föld)
    B11111, B11111, B11111, B11111,
    B11111, B11111, B11111, B11111,
    // Talaj jobb széle
    B00011, B00011, B00011, B00011,
    B00011, B00011, B00011, B00011,
    // Talaj bal széle
    B11000, B11000, B11000, B11000,
    B11000, B11000, B11000, B11000,
  };
  
  int i;
  // A 0-s karaktert kihagyjuk, így az lcd.print() gyorsan ki tud rajzolni több karaktert
  for (i = 0; i < 7; ++i) {
    lcd.createChar(i + 1, &graphics[i * 8]);
  }
  
  // Terep inicializálása üresre
  for (i = 0; i < TERRAIN_WIDTH; ++i) {
    terrainUpper[i] = SPRITE_TERRAIN_EMPTY;
    terrainLower[i] = SPRITE_TERRAIN_EMPTY;
  }
}

// A pálya balra léptetése félkarakteres lépésekben
void advanceTerrain(char* terrain, byte newTerrain) {
  for (int i = 0; i < TERRAIN_WIDTH; ++i) {
    char current = terrain[i];
    char next = (i == TERRAIN_WIDTH - 1) ? newTerrain : terrain[i + 1];
    
    switch (current) {
      case SPRITE_TERRAIN_EMPTY:
        // Üres: ha mellette tömör, akkor jobb szélessé válik
        terrain[i] = (next == SPRITE_TERRAIN_SOLID) ? 
                     SPRITE_TERRAIN_SOLID_RIGHT : SPRITE_TERRAIN_EMPTY;
        break;
        
      case SPRITE_TERRAIN_SOLID:
        // Tömör: ha mellette üres, akkor bal szélessé válik
        terrain[i] = (next == SPRITE_TERRAIN_EMPTY) ? 
                     SPRITE_TERRAIN_SOLID_LEFT : SPRITE_TERRAIN_SOLID;
        break;
        
      case SPRITE_TERRAIN_SOLID_RIGHT:
        terrain[i] = SPRITE_TERRAIN_SOLID;
        break;
        
      case SPRITE_TERRAIN_SOLID_LEFT:
        terrain[i] = SPRITE_TERRAIN_EMPTY;
        break;
    }
  }
}

// Hős kirajzolása és ütközésvizsgálat
bool drawHero(byte position, char* terrainUpper, char* terrainLower, unsigned int score) {
  bool collide = false;
  char upperSave = terrainUpper[HERO_HORIZONTAL_POSITION];
  char lowerSave = terrainLower[HERO_HORIZONTAL_POSITION];
  byte upper, lower;
  
  // Pozíció alapján sprite-ok kiválasztása
  switch (position) {
    case HERO_POSITION_OFF:
      upper = lower = SPRITE_TERRAIN_EMPTY;
      break;
    case HERO_POSITION_RUN_LOWER_1:
      upper = SPRITE_TERRAIN_EMPTY;
      lower = SPRITE_RUN1;
      break;
    case HERO_POSITION_RUN_LOWER_2:
      upper = SPRITE_TERRAIN_EMPTY;
      lower = SPRITE_RUN2;
      break;
    case HERO_POSITION_JUMP_1:
    case HERO_POSITION_JUMP_8:
      upper = SPRITE_TERRAIN_EMPTY;
      lower = SPRITE_JUMP;
      break;
    case HERO_POSITION_JUMP_2:
    case HERO_POSITION_JUMP_7:
      upper = SPRITE_JUMP_UPPER;
      lower = SPRITE_JUMP_LOWER;
      break;
    case HERO_POSITION_JUMP_3:
    case HERO_POSITION_JUMP_4:
    case HERO_POSITION_JUMP_5:
    case HERO_POSITION_JUMP_6:
      upper = SPRITE_JUMP;
      lower = SPRITE_TERRAIN_EMPTY;
      break;
    case HERO_POSITION_RUN_UPPER_1:
      upper = SPRITE_RUN1;
      lower = SPRITE_TERRAIN_EMPTY;
      break;
    case HERO_POSITION_RUN_UPPER_2:
      upper = SPRITE_RUN2;
      lower = SPRITE_TERRAIN_EMPTY;
      break;
    default:
      upper = lower = SPRITE_TERRAIN_EMPTY;
      break;
  }
  
  // Felső karakter berajzolása
  if (upper != ' ') {
    terrainUpper[HERO_HORIZONTAL_POSITION] = upper;
    collide = (upperSave != SPRITE_TERRAIN_EMPTY);
  }
  
  // Alsó karakter berajzolása
  if (lower != ' ') {
    terrainLower[HERO_HORIZONTAL_POSITION] = lower;
    collide |= (lowerSave != SPRITE_TERRAIN_EMPTY);
  }
  
  // Pontszám számjegyeinek meghatározása
  byte digits = (score > 9999) ? 5 : (score > 999) ? 4 : 
                (score > 99) ? 3 : (score > 9) ? 2 : 1;
  
  // Játéktér kirajzolása
  terrainUpper[TERRAIN_WIDTH] = '\0';
  terrainLower[TERRAIN_WIDTH] = '\0';
  char temp = terrainUpper[16 - digits];
  terrainUpper[16 - digits] = '\0';
  
  lcd.setCursor(0, 0);
  lcd.print(terrainUpper);
  terrainUpper[16 - digits] = temp;
  
  lcd.setCursor(0, 1);
  lcd.print(terrainLower);
  
  // Pontszám kiírása
  lcd.setCursor(16 - digits, 0);
  lcd.print(score);
  
  // Eredeti terep visszaállítása
  terrainUpper[HERO_HORIZONTAL_POSITION] = upperSave;
  terrainLower[HERO_HORIZONTAL_POSITION] = lowerSave;
  
  return collide;
}

// Gombnyomás megszakításkezelő
void buttonPush() {
  buttonPushed = true;
}

void setup() {
  // I/O lábak beállítása
  pinMode(PIN_READWRITE, OUTPUT);
  digitalWrite(PIN_READWRITE, LOW);
  pinMode(PIN_CONTRAST, OUTPUT);
  digitalWrite(PIN_CONTRAST, LOW);
  pinMode(PIN_BUTTON, INPUT);
  digitalWrite(PIN_BUTTON, HIGH);
  pinMode(PIN_AUTOPLAY, OUTPUT);
  digitalWrite(PIN_AUTOPLAY, HIGH);
  
  // Megszakítás beállítása (2-es láb -> 0-ás megszakítás)
  attachInterrupt(0, buttonPush, FALLING);
  
  initializeGraphics();
  lcd.begin(16, 2);
}

void loop() {
  static byte heroPos = HERO_POSITION_RUN_LOWER_1;
  static byte newTerrainType = TERRAIN_EMPTY;
  static byte newTerrainDuration = 1;
  static bool playing = false;
  static bool blink = false;
  static unsigned int distance = 0;
  
  // ========== KEZDŐKÉPERNYŐ ==========
  if (!playing) {
    drawHero(blink ? HERO_POSITION_OFF : heroPos, 
             terrainUpper, terrainLower, distance >> 3);
    
    if (blink) {
      lcd.setCursor(0, 0);
      lcd.print("Press Start");
    }
    
    delay(250);
    blink = !blink;
    
    if (buttonPushed) {
      initializeGraphics();
      heroPos = HERO_POSITION_RUN_LOWER_1;
      playing = true;
      buttonPushed = false;
      distance = 0;
    }
    return;
  }
  
  // ========== JÁTÉK FUTÁSA ==========
  
  // Pálya léptetése
  advanceTerrain(terrainLower, newTerrainType == TERRAIN_LOWER_BLOCK ? 
                 SPRITE_TERRAIN_SOLID : SPRITE_TERRAIN_EMPTY);
  advanceTerrain(terrainUpper, newTerrainType == TERRAIN_UPPER_BLOCK ? 
                 SPRITE_TERRAIN_SOLID : SPRITE_TERRAIN_EMPTY);
  
  // Új akadály generálása
  if (--newTerrainDuration == 0) {
    if (newTerrainType == TERRAIN_EMPTY) {
      newTerrainType = (random(3) == 0) ? TERRAIN_UPPER_BLOCK : TERRAIN_LOWER_BLOCK;
      newTerrainDuration = 2 + random(10);
    } else {
      newTerrainType = TERRAIN_EMPTY;
      newTerrainDuration = 10 + random(10);
    }
  }
  
  // Ugrás vezérlése
  if (buttonPushed) {
    if (heroPos <= HERO_POSITION_RUN_LOWER_2) {
      heroPos = HERO_POSITION_JUMP_1;
    }
    buttonPushed = false;
  }
  
  // Ütközésvizsgálat és állapotfrissítés
  if (drawHero(heroPos, terrainUpper, terrainLower, distance >> 3)) {
    playing = false;  // Game Over
  } else {
    // Állapotgép
    if (heroPos == HERO_POSITION_RUN_LOWER_2 || heroPos == HERO_POSITION_JUMP_8) {
      heroPos = HERO_POSITION_RUN_LOWER_1;
    } else if ((heroPos >= HERO_POSITION_JUMP_3 && heroPos <= HERO_POSITION_JUMP_5) && 
               terrainLower[HERO_HORIZONTAL_POSITION] != SPRITE_TERRAIN_EMPTY) {
      heroPos = HERO_POSITION_RUN_UPPER_1;
    } else if (heroPos >= HERO_POSITION_RUN_UPPER_1 && 
               terrainLower[HERO_HORIZONTAL_POSITION] == SPRITE_TERRAIN_EMPTY) {
      heroPos = HERO_POSITION_JUMP_5;
    } else if (heroPos == HERO_POSITION_RUN_UPPER_2) {
      heroPos = HERO_POSITION_RUN_UPPER_1;
    } else {
      heroPos++;
    }
    
    distance++;
    
    // Automatikus játék jelzés
    digitalWrite(PIN_AUTOPLAY, 
                 terrainLower[HERO_HORIZONTAL_POSITION + 2] == SPRITE_TERRAIN_EMPTY ? 
                 HIGH : LOW);
  }
  
  delay(50);
}
