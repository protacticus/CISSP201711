# CISSP201711
CISSP tanfolyam 2017 november-december.

## Segédeszközök
- [MarkDown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
- [GitHub](https://github.com/gplesz/CISSP201711)
- [Visual Studio Code](https://code.visualstudio.com/)
  [nyílt forráskódú](https://github.com/Microsoft/vscode) multiplatform (Windows, linux, OsX) fejlesztőkörnyezet.

## Témakörök
- Computing Systems
- Databases
- Software Development

## Computing Systems
### Hardware
### Processor
- Moore törvénye (Gordon Moore)
  - 1965: ezt követően tíz évig _a tranzisztorok sűrűsége_ meg fog duplázódni az integrált áramkörökön
  - 1975: újraértékelt, és további 10 évre kiterjeszti
  - David House (Intel): 18 havonta a _teljesítmény_ duplázódik (a mennyiség mellett a sebességgel is számol)

  Példa: Lotus 1-2-3 (DOS, 640k)/Excel párharca (Windows)

### Execution Type
- Multitasking
Több feladat egyidejű végrehajtására képes a rendszer. A legtöbb rendszer nem képes erre, az operációs rendszer szimulálja a CPU idejének a felosztásával.

- Multiprocessing
Párhuzamos végrehajtásra képes rendszerek. Egynél több processzorral egyszerre több feladat végrehajtása zajlik.
  - SMP (Simmetric Multiprocessing): egy számítógép több processzort tartalmaz, ugyanaz az OS kezeli őket, általában nem csak az OS közös, hanem az adathozzáférés és a memóriahasználat is. Több processzor, általában 2-16 db.
  - MPP (Massive Parallel processing): több száz processzor vagy több ezer processzor, minden processzorhoz saját adathozzáférés és operációs rendszer. Például particionált adatok (facebook ország szerinti adatai, google keresés)

- Multiprogramming
A multitaskinghoz hasonlóan több feladat egyidejű végrehajtására képes programozást jelent.

```



                                                                                +--------------+
                                                                                | Adatbázis    |
        +----------------+                                                      |--------------|
        | Böngésző       |                        +-------------------+    +--->|              |
        |----------------|                        | Alkalmazásszerver |    ^    |              |
        |                |                        |-------------------|    |    |              |
        |                +----------------------->|Kérés végrehajtása |+---+    +--------------+
        |                |                        |Vár az adatbázisra | (miközben az adatbéázisra vár, közben a többi 
        |                |                        |Kérés végrehajtás  |  feladatot végrehajtja)
        |                |                        |       folytatása  |
        +----------------+                        +-------------------+
```
   - inkább a régi nagy mainframe-ek sajátja, a multitask pedig a PC-kre jellemző
   - a multitaskot az OS kezeli, a multiprogramminghoz speciálisan megírt programra van szükség

- Multithreading (többszálú végrehajtás)
  Lehetővé teszi az operációs rendszer szolgáltatásával, hogy egy alkalmazáson belül párhuzamos feladatvégrehajtás történjen úgy, hogy különleges programozásra nincs szükség. Azért hatékony, mert a szálak közti váltások sokkal gyorsabbak (50 utasítás) mint a processzek közti váltások (1000 utasítás)

- Process (folyamat)
  Egy alkalmazás és a futtatásához szükséges környezet.
- Thread  (szál)
  A folyamaton (alkalmazáson) belül végrehajtási szál, saját call stack-kel. Az OS biztosítja. 

### Processing Types
A magas biztonságú rendszerek úgy felügyelik az információfeldolgozás folyamatát, hogy különböző biztonsági szintekhez rendelik őket.
- besorolás nélküli
- érzékeny
- bizalmas
- titkos
- szigorúan titkos

#### Single state
Egyszerre csak egyféle biztonsági szintet kezel a rendszer. A biztonsági adminisztrátor kezeli, nem a számítógép, vagy az OS.

#### Multi State
Egyszerre több biztonsági hozzáférést képes nyújtani. Ehhez tartalmaz védelmi mechanizmust, hogy a biztonsági szintek átlépését kivédje. Ez a mechanizmus nagyon költséges, és az ilyen rendszerek nagyon ritkák. Egyes drága MPP rendszerekhez ritkán van olyan indok, ami értelmessé teszi ezt a fajta kialakítást.

### Protection rings
Az operációs rendszerek tervezésénél védelmi körökbe szervezik a feladatokat.

```
+----------------------------------------------------------------+
|   User-level programs and applications                         |
|----------------------------------------------------------------|
|                                                                |
|                                                                |
|        +----------------------------------------------+        |
|        |  Drivers, protocols                          |        |
|        |----------------------------------------------|        |
|        |                                              |        |
|        |                                              |        |
|        |        +--------------------------+          |        |
|        |        |  Other OS component      |          |        |
|        |        |--------------------------|          |        |
|        |        |   +------------------+   |          |        |
|        |        |   | OS kernel/memory |   |          |        |
|        |        |   |------------------|   |          |        |
|        |        |   |                  |   |          |        |
|        |        |   |                  |   |          |        |
|        |        |   |                  |   |          |        |
|        |        |   +------------------+   |          |        |
|        |        |                          |          |        |
|        |        +--------------------------+          |        |
|        |                                              |        |
|        |                                              |        |
|        +----------------------------------------------+        |
|                                                                |
+----------------------------------------------------------------+
```
- ring 0 (legbelsőbb kör)
- ring 1
- ring 2
- ring 3 (felhasználói rendszerek)

### Process states

```
                   Process needs another time slice
 New process    <--------------------------------------+^                   Stopped
      +         +                                       |                       ^
      |         |                                       |        When process   |
      |         |                                       |        finished or    |
      |         v      If CPU is available              +        terminated     +
      +-----> Ready +------------------------------> Running  +----------------->
                                                     ^   +
                                        +------------+   |
                                        |                |
                                        |                | block for I/O,
                                        |                | resources
                                        |unblocked       |
                                        |                |
                                        |                |
                                        |                |
                                        +                |
                                                         |
                                     Waiting <-----------+
```


- Ready
  A folyamat kész arra, hogy elkezdje a végrehajtást. Ha lesz szabad processzor, akkor ezt a folyamatot végre is hajthatja. A hozzárendelt erőforrások ki vannak neki osztva.
- Waiting
  A folyamatunk külső erőforrásra vár. A futása addig, amíg az erőforráshozzáférés meg nem történik, blokkolva van.
- Running
  A folyamat CPU végrehajtás közben van. Addig tart, amíg (a) nem végez, (b) le nem jár az időszelete (c) egy erőforráshozzáférés miatt blokkolt állapotba kerül.
- Supervisory
  Amikor magasabb jogok szükségesek (egy körrel beljebb kellene kerülni), driver telepítés, stb.
- Stopped
  Amikor a folyamat végzett, vagy meg kell szakítani (hiba, nem elérhető erőforrás, stb.) A hozzárendelt erőforrások elvonhatók, újraoszthatók.

### Memory
- ROM: Read-Only Memory
- PROM: Programmable Read-Only Memory
- EPROM: Erasable Programmable Read-Only Memory
- EEPROM: Electronically Erasable Programmable Read-Only Memory
- Flash: blokkonként törölhető EEPROM
- RAM: Random Access Memory
  - dinamic: kapacitásokból áll, a CPU-nak időnként frissítenie kell.
  - static: flip-flop áramkörök, nem kell a CPU-nak frissíteni. Amíg áram alatt van nem felejt. Gyorsabb és dágább.
Cache RAM: Level1: a processzoron, Level2: static RAM

### Registers
Speciális memóriák, szinkron sebességgel dolgoznak, mint a processzor végrehajtási egysége (ALU: Arithmetic-logical unit)
- Register addressing: amikor a porcesszor a regisztert címzi, hivatkozza (register 1)
- Inmediate addressing: közvetlen műveletvégzés a regiszter segítségével (add 2 to register 1)
- Direct addressing: a registerben a memóriahely címe van, amivel dolgozni kell, ahol az adat van. (a cím egy lapon van a paranccsal)
- Indirect addressing: a registerben lévő memóriahelyen újabb cím vár minket: az a cím, ahol az adat van, amivel dolgozni kell. (a cím egy lapon van a paranccsal)
- Base + Offset addressing: ha nem azonos lapon van az adat, akkor a lapcím + a lapon belüli cím jelenti az adat címét, amivel dolgozunk.

### Primary, Secondary Storaga
- Primary memory: "valódi memória", amit a processzor a regiszter műveletekkel eléri (gyors és drága)
- Secondary storage: először be kell tölteni a primary memóriába, hogy használni tudjuk. (lassú és olcsó)

### Virtual Memory, Virtual Storage
Olyan másodlagos memóriák, amiket az operációs rendszer használ, segítségével elsődlegesnek látszik. Hátránya a sebessége.

### I/O Devices
- Monitor
- Printer
- Keyboard/Mouse
- Modems
- I/O structures
  - Memory-Mapped I/O
  - IRQ
  - DMA
- Firmware
  - BIOS
  - Device Firmware

### Demo a multithreading-hez

```
dotnet new console
```

a program
```csharp
    class Program
    {

        private static void Tennivalo(object state)
        {
            System.Console.WriteLine("elindult a szálon a feladat");
            Thread.Sleep(4000);
            System.Console.WriteLine("megállt a szálon a feladat");
        }

        static void Main(string[] args)
        {
            ThreadPool.QueueUserWorkItem(Tennivalo);
            ThreadPool.QueueUserWorkItem(Tennivalo);
            ThreadPool.QueueUserWorkItem(Tennivalo);
            ThreadPool.QueueUserWorkItem(Tennivalo);
            ThreadPool.QueueUserWorkItem(Tennivalo);
            ThreadPool.QueueUserWorkItem(Tennivalo);
            ThreadPool.QueueUserWorkItem(Tennivalo);

            Console.ReadLine();
        }
    }
```
eredmény
```
elindult a folyamat
elindult a folyamat
elindult a folyamat
elindult a folyamat
elindult a folyamat
elindult a folyamat
elindult a folyamat
megállt a folyamat
megállt a folyamat
megállt a folyamat
megállt a folyamat
megállt a folyamat
megállt a folyamat
megállt a folyamat
```
Megjegyzések:
- semmilyen különleges kód nem kell a párhozamos programozáshoz
- az első négy thread azonnal elindult, mivel a gép amin futtattam 4 processzormagot tartalmaz
- majd lassabban, de elindult a többi szál is, mielőtt végrehajtotta volna az összes futó feladatot

## Databases

- Mi is az az adatbázis?
  Adatok gyűjteménye, felhasználók által hozzáférhető módon kialakítva, lehetséges mind az adatok megtekintése, mind létrehozás és módosítás. Vagyis, az adatok, kiegészítve a hozzáférés módjával (DBMS)

  - DBMS: DataBase Magement System

- Relational: A túlnyomó része relációs adatbázis (1970 IBM E.F Codd)
- Hierarchical: 

```
                                        +<----------------++------------->+
                                        v                                 v
                        <-----------+  com +----------->                 hu
                        +                              +
                        |                              |
                        |                              |
                        |                              |
                        |                              |
                        |                              |
                        |                              |
                        v                              |
                                                    v
    +<----+ microsoft.com+---->                  cisco.com
    |                         +
    |                         |
    |                         |
    |                         |
    v                         v
app.microsoft.com       www.microsoft.com
```
- object oriented
- distributed network

- SQL: (SEQUEL)
  - [ingyenes könyv PDF-ben](https://devportal.hu/download/E-bookok/Adatkezeles%20otthon%20es%20a%20felhoben/Adatkezeles%20otthon%20es%20a%20felhoben-%20Foti-Turoczy.pdf)
  - [ugyanez papír alapon](https://www.libri.hu/konyv/foti_marcell.adatkezeles-otthon-es-a-felhoben.html)

- SQL képességek

  - adatok táblázatban (két dimenziós struktúra)
    - vízszintesen: sorok, rekordok (row)
    - függőlegesen: oszlopok, mezők (column)

    - vizszintesen ritkán változnak, függőlegesen állandóan. Az idejének nagy részét sorok létrehozása, módosítása, törlése és lekérdezése tölti ki.

|Kulcs| Név         | Cím                        | Befizetés/Kifizetés | Partner     | Partner címe               |
|    1| Kiss József | 1000 Budapest, Fő utca 1   |              +5000  | Nagy Zoltán | 2000 Szentendre Al utca 1. | 
|    2| Kiss József | 1000 Budapest, Fő utca 1   |              +2000  | Gipsz Jakab |                            |
|    3| Kiss József | 1000 Budapest, Fő utca 1   |              -4000  | Fő Kálmán   |                            |
|    4| Nagy Zoltán | 2000 Szentendre Al utca 1. |              -5000  | Kiss József | 1000 Budapest, Fő utca 1   |
|    5| Nagy Zoltán | 2000 Szentendre Al utca 1. |              +5000  | Pofá Zoltán |                            |
|    6| Nagy Zoltán | 2000 Szentendre Al utca 1. |              +5000  | Tüdő Pál    |                            |

  - Kulcs (KEY): Elsődleges kulcs (Primary key: PK)
    A tábla kulcsa olyan adat, ami egyértelműen azonosítja a sort. Vagyis kétszer nem szerepelhet a táblázatban.
    - identity: egy szám, ami minden sor hozzáadásával nő
    - guid: 

  - normalizálás (optimális tárolási és visszakeresési forma eléréséhez)
    - 1NF, 2NF, 3NF

Pénzmozgások
------------
|Kulcs| Partner1 | Befizetés/Kifizetés | Partner 2|
|    1|        2 |              +5000  | 2 | 
|    2| 1        |              +2000  | 3 |

|    3| 1        |              -4000  | 4 |
|    4| 2        |              -5000  | 1 |
|    5| 2        |              +5000  | 5 |
|    6| 2        |              +5000  | 6 |


Partnerek
-----------
|Kulcs|Név         |Cím                        |
|     1|Kiss József| 1000 Budapest, Fő utca 1  |
|     4|Nagy Zoltán| 2000 Szentendre Al utca 1.|
|     5|Gipsz Jakab|                           |
|     6|Fő Kálmán  |                           |
|     7|Pofá Zoltán|                           |
|     8|Tüdő Pál   |                           |

  - Távoli kulcs (Foreign key: FK)
    egy másik tábla PK-jára mutató mező
    Ez az alapja a referential integrity védelmi képességnek. Nem tartalmaz az adatbázis árvénytelen hivatkozást.

  - Az adatbázis védelmi képességei
    a lényeg, hogy ne kerülhessen be az adatbázisba érvénytelen információ

```sql
select
	p1.Nev
	,p2.Nev
	,Osszeg
from
	Penzmozgas penz
	inner join Partnerek p1 on p1.Kulcs = penz.Partner1
	inner join Partnerek p2 on p2.Kulcs = penz.Partner2

```
és az eredmény:

```
Kiss Zoltán	Nagy Zoltán	5000
Nagy Zoltán	Kiss Zoltán	-5000
```

