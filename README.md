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
    - program, ami az adathozzáférést vezérli (létrehozás, módosítás, törlés, lekérdezés)
    - adatmentések és a mentések visszatöltése (Backup and recovery)
    - lekérdező nyelv biztosítása (SQL)
      - SQL/DQL: Data Query Language - a SELECT utasítás
      - SQL/DML: Data Manipulation Language: INSERT, UPDATE, DELETE
    - adatbázis struktúra karbantartása (SQL/DDL: Data Definition language)
    - az adathozzáférések szabályozása (SQL/DCL: Data Control language)

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

- SQL: (SEQUEL: Structured English Query Language)
  - [ingyenes könyv PDF-ben](https://devportal.hu/download/E-bookok/Adatkezeles%20otthon%20es%20a%20felhoben/Adatkezeles%20otthon%20es%20a%20felhoben-%20Foti-Turoczy.pdf)
  - [ugyanez papír alapon](https://www.libri.hu/konyv/foti_marcell.adatkezeles-otthon-es-a-felhoben.html)

### SQL képességek

  - adatok táblázatban (két dimenziós struktúra: table=relation) 
    - vízszintesen: sorok, rekordok (row=record=tuple)
    - függőlegesen: oszlopok, mezők (column=field=attribute)

    - vizszintesen ritkán változnak, függőlegesen állandóan. Az idejének nagy részét sorok létrehozása, módosítása, törlése és lekérdezése tölti ki.

|Kulcs| Név         | Cím                        | Befizetés/Kifizetés | Partner     | Partner címe               |
|-|-|-|-|-|-|
|    1| Kiss József | 1000 Budapest, Fő utca 1   |              +5000  | Nagy Zoltán | 2000 Szentendre Al utca 1. | 
|    2| Kiss József | 1000 Budapest, Fő utca 1   |              +2000  | Gipsz Jakab |                            |
|    3| Kiss József | 1000 Budapest, Fő utca 1   |              -4000  | Fő Kálmán   |                            |
|    4| Nagy Zoltán | 2000 Szentendre Al utca 1. |              -5000  | Kiss József | 1000 Budapest, Fő utca 1   |
|    5| Nagy Zoltán | 2000 Szentendre Al utca 1. |              +5000  | Pofá Zoltán |                            |
|    6| Nagy Zoltán | 2000 Szentendre Al utca 1. |              +5000  | Tüdő Pál    |                            |

  - Kulcs (KEY): Elsődleges kulcs (Primary key: PK)
    A tábla kulcsa olyan adat, ami egyértelműen azonosítja a sort. Vagyis kétszer nem szerepelhet a táblázatban.
    Például (de nem kizárólagosan):
    - identity: egy szám, ami minden sor hozzáadásával nő
    - guid: globális egyedi azonosító, lokálisan generált (kliens program is generálhatja), de a világon egyedi érték
    - ez biztosítja az **Entity integrity service**-t

  - normalizálás (optimális tárolási és visszakeresési forma eléréséhez)
    - 1NF, 2NF, 3NF [Példa](https://support.microsoft.com/hu-hu/help/283878/description-of-the-database-normalization-basics)

Pénzmozgások
---
|Kulcs| Partner1 | Befizetés/Kifizetés | Partner 2|
|-|-|-|-|
|    1|        4 |              +5000  | 2 | 
|    4|        1 |              +2000  | 3 |
|    5|        1 |              -4000  | 4 |
|    6|        2 |              -5000  | 1 |
|    7|        2 |              +5000  | 5 |
|    8|        2 |              +5000  | 6 |


Partnerek
---
|Kulcs|Név         |Cím                        |
|-|-|-|
|     1|Kiss József| 1000 Budapest, Fő utca 1  |
|     4|Nagy Zoltán| 2000 Szentendre Al utca 1.|
|     5|Gipsz Jakab|                           |
|     6|Fő Kálmán  |                           |
|     7|Pofá Zoltán|                           |
|     8|Tüdő Pál   |                           |

  - Távoli kulcs (Foreign key: FK)
    egy másik tábla PK-jára mutató mező
    Ez az alapja a **referential integrity** védelmi képességnek. Nem tartalmaz az adatbázis árvénytelen hivatkozást.


DQL példa
---
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

DML példák
---
```sql
update 
	Partnerek
set
	Nev='Kiss Lajos'
where
	Kulcs=1
```

```sql
insert
	Partnerek
	(Nev, Cim) 
values
	('Gipsz Jakab', '3000 Nem tudom hol')
```

```sql
delete
	Partnerek
where
	Kulcs=5
```

### Database Schema (definiálja az adatbázis felépítését, struktúráját)
- Tables
- Relationships (táblák közti kapcsolatok, FK-k)
- Business rules
- Domains

### Az adatbázis védelmi képességei (integrity services)
    a lényeg, hogy ne kerülhessen be az adatbázisba érvénytelen információ
- Entity integrity
  garantálja, hogy minden sornak van egyedi azonosítója (PK)
- Referential integrity
  biztosítja, hogy az FK mezőben lévő információ létezó PK-ra m utat.
  (felvitelkor, módosításkor és törléskor )
- Semantic integrity
  Biztosítja, hogy a strukturalis és szemantikus szabályok ki legyenek kényszerítve
  - not null
  - check constraint

### Adatbázis nézetek (Views)
Az adatbázis elé egy olyan felületet (virtuális táblázatot) generálhatunk, amihez saját jogosultságokat lehet rendelni.

példa (egyben DDL példák):
létrehozás
```sql
drop view PezmozgasokNezet

create view 
	PenzmozgasokNezet
as
select
	p1.Nev Partner1Nev
	,p2.Nev Partner2Nev
	,Osszeg
from
	Penzmozgas penz
	inner join Partnerek p1 on p1.Kulcs = penz.Partner1
	inner join Partnerek p2 on p2.Kulcs = penz.Partner2
```

törlés
```sql
drop view PezmozgasokNezet
```

### Database integrity operations (transactions)
a tranzakciók jellemzői
- Atomic: nem részekre bontható, a benne foglalt utasítások összetartoznak, vagy együtt hajtódnak végre, vagy nem hajtódnak végre.
- Consistency: konzisztencia megtartása, a tranzakció az adatbázist a korábbi konzisztens (az integritási szabályoknak megfelelő) állapotból konzisztens (az integritási szabályoknak megfelelő) állapotba viszi
- Isolation: elszigetelt végrehajtás, amit a tranzakcióban végzünk, azt mások addig, amíg nem véglegesítettük nem láthatják.
- Durable: tartós, ha a tranzakciót lezártuk (COMMIT paranccsal) akkor annak az eredménye tartósan megmarad.

```sql
begin transaction
insert
	Partnerek
	(Nev, Cim) 
values
	('Gipsz Jakab', '3000 Nem tudom hol')


select * from Partnerek

insert
	Penzmozgas
	(Partner1, Partner2, Osszeg)
values
	(1,8,-5000)

--rollback           --ezzel visszavonnánk, nem csinálna semmit
commit transaction   --ezzel lezárjuk, minden módosítás végleges
```

### Data Dictionary
- központosítottan tárolt adat definíciók, schemák és hivatkozási kulcsok
- leíró adatok (Metadata)
- szabályok (Rulas)

### Database communication
- ODBC (Open Database Connectivity)
- OLE DB (Object Linking and Embedding)
- ADO (ActiveX Data Object)
- (EF Entity Framework)
- JDBC (Java Database Connectivity)
- XML - Extensible Markup Language

```xml
<Parner>
  <Nev Value="Gipsz Jakab" />
  <Cim>3000 Nem tudom hol</Cim>
</Partner>
```
- (JSON - JavaScript Object Notation)
```json
{
    "Nev": "Gipsz Jakab",
    "Cim": "3000 Nem tudom hol"
}
```

### Data Warehousing (Adattárház)
- Különböző adatok különböző forrásokból és/vagy adatbázisból
- Az adatok denormalizáltak (például előre felösszegezett részösszegek)
- Adatbányászat (Data mining)

### Biztonsági kérdések
- Concurrency/LOCK konkurrens módosítás ellen a lock mechanizmusok 
- database partitioning: a rész adatbázisokon lehet jogosultságot osztani
- Polyinstantiating: azonos kulccsal több rekord létrehozása, hogy különböző jogosultsági szinteken állók ne tudjanak következtetni az összegzésekből például.
- Database contamination: adatszennyezés, ugyanebből a célból

figyelem: ezek implementálása nagyon komoly kihívás

- az adatbázis perzisztens tárolásánál figyelni kell arra, hogy ne csak az OS biztonsági képességeiben bízzunk. (titkosított mentések, titkosított partíció az adatbázisfile-oknak)
- többszintű biztonsági rendszer esetén szükséges az adatokat (memóriában és perzisztens tárolón is) fail safe. Azaz, ha valamilyen hiba történik, akkor a rendszer olyan állapotba kerül, hogy emberi beavatkozás nélkül nem használható.

## Softvare Develoment
### Mi a probléma a szoftverfejlesztéssel?
Összetett és rengeteg kihívással teli feledatot tartalmazó problémakör. Gyakran
- hozzáférnek érzékeny adatokhoz
- nagyobb közönség számára hozzáférhetőek
- sok esetben saját szoftver készül
- a szoftver lehet "túl erős"
  - közvetlen írányítés hardware és szoftvert elemek felett
  - közvetlen írányítás az állományok felett
  - közvetlenül hozzáférhet szerverekhez
  - közvetlenül hozzáférhet I/O eszközökhöz
  - közvetlen hozzáférés rendszer segédprogramokhoz
  - közvetlen hozzáférés feladatok futtatásához/leállításához
  - beállíthat dátumot, időt, jelszót
  - képes indokolatlan lekérdezésekhez (DDoS)
  - hozzáférés naplóhoz/audit loghoz
  - hozzáférés erőforrásokhoz.

Amennyiben saját szoftvert gyártunk fontos, hogy a biztonsági kérdések megfelelő súlyt kapjanak a kezdés pillanatától kezdve. Utólag biztonságot adni egy rendszernek nem lehet.

### Programozási nyelvek
Célja: A igenyeknek megfelelően bonyolult, egyszerű összeadással/logikai műveletekkel (amit a CPU támogat) nem egyszerűen leírható vagy leírhatatlan feladatok megoldása magasabb szinten.

- 1GL: Gépi kód (a processzor számára közvetlenül értelmezhető utasítássorozat számokból)
- 2GL: Assembly 
- 3GL: Fordított (Interpretált) nyelvek (Fortran, Algol)
- 4GL: Olyan nyelv, ami megpróbálja a természetes emberi nyelvet utánozni + használja az SQL-t adathozzáférésre.
- 5GL: Grafikus felületen keresztüli kód létrehozás

Managelt nyelvek
---
Forráskód ---(fordítás)---> Köztes nyelv ---------(interpreter/virtualis gép)-----------> Gépi kód
                            - Byte code
                            - IL (MSIL MS Intermediate Language)

- Objektumorientált nyelvek (Bankszámla nyilvántartáson keresztül)

```csharp
    class Program
    {
        static void Main(string[] args)
        {
            var cegesBankszmla = new Bankszamla("Kerékgyártás KFT");

            System.Console.WriteLine(cegesBankszmla.Nev);
            cegesBankszmla.Jovairas(5000);
            cegesBankszmla.Terheles(2000);
            cegesBankszmla.Terheles(4000);
        }
    }

    public class Bankszamla
    {
        private string nev;

        public Bankszamla(string nev)
        {
            this.nev = nev;
        }

        public string Nev { get {return this.nev;} }

        private int egyenleg;
        public int Egyenleg { get {return this.egyenleg;} }

        public void Terheles(int osszeg)
        {
            if (osszeg < 0)
            {
                throw new Exception("terhelni csak pozitív számot lehet");
            }

            if (Egyenleg<osszeg)
            {
                throw new Exception($"a terhelendő összeg {osszeg} nagyobb, mint a rendelkezésre álló összeg {egyenleg}");
            }

            egyenleg = egyenleg - osszeg;
            System.Console.WriteLine($"{osszeg} terhelése megtörtént, új egyenleg: {egyenleg}");
        }

        public void Jovairas(int osszeg)
        {
            if (osszeg < 0)
            {
                throw new Exception("jovairni csak pozitív számot lehet");
            }

            egyenleg = egyenleg + osszeg;
            System.Console.WriteLine($"{osszeg} jóváírása megtörtént, új egyenleg: {egyenleg}");
        }
    }
```
Eredmény
```
PS D:\Repos\CISSP201711\OOPDemo> dotnet run
Kerékgyártás KFT
5000 jóváírása megtörtént, új egyenleg: 5000
2000 terhelése megtörtént, új egyenleg: 3000

Unhandled Exception: System.Exception: a terhelendő összeg 4000 nagyobb, mint a rendelkezésre álló összeg 3000
   at OOPDemo.Bankszamla.Terheles(Int32 osszeg) in D:\Repos\CISSP201711\OOPDemo\Program.cs:line 46
   at OOPDemo.Program.Main(String[] args) in D:\Repos\CISSP201711\OOPDemo\Program.cs:line 14
```

### Objektum orientált programozási alapelvek
- Encapsulation (Egységbezárás)
  Osztályon kívülről csak az látszik, amit engedünk
- Abstraction (Absztrakció)
  Csak azzal foglalkozunk, ami fontos.
- Inheritance (Leszármaztatás)
```csharp
    public class KamatozoBankszamla : Bankszamla
    { //csak a kamatozással kell foglalkoznom, a Bankszamla osztály már hozza az egyenleg/jóváírás/terhelés tulajdonságokat

    }
```
- Polimorphism (többféle változat kialakítható, az ősosztály tulajdonságai felülírhatók)

### Objektumorientált programozás előnyei
- Modularity
- Reusable (két fontos feltétel)
  - High Cohesion (Erős kohézió)
    Egy osztályhoz a feladatok mennyire hasonló vagy azonos felelősségi körhöz tartoznak.
  - Low Coupling (Gyenge csatolás)
    (Erős) Csatolás akkor van két osztály között, ha az egyik változása esetén **nem kizárható** a másik változása.
    Gyenge csatolás esetén a változás kizárható.

```

               +------------------+
               |  Űrlap 1         |
               |------------------|
               |                  |
               |                  |+------------>
               |                  |             +
               +------------------+             v
                                         +---------------------+        +---------------------+
                                         | Repository          |        |  Adatbázis          |
                                         |---------------------|        |---------------------|
                                         |                     |        |                     |
               +------------------+      |  Minden adatbázis-  |        |                     |
               |  Űrlap 2         |      |  hozzáféréssel      |        |                     |
               |------------------|      |  kapcsolatos kód    |        |                     |
               |                  |      |                     |+------>|                     |
               |                  |+---->|                     |        |                     |
               |                  |      |                     |        |                     |
               |                  |      |                     |        |                     |
               +------------------+      +---------------------+        |                     |
                                                ^                       +---------------------+
                                                |
               +------------------+             |
               |  Űrlap 3         |             |
               |------------------|             +
               |                  | +----------->
               |                  |
               |                  |
               +------------------+
                      <-------------+Erős kohézió-gyenge csatolás+------------>
```

### Object Oriented Concepts
- Class/Object
  - Class: a létrehozandó objektum tervrajza
  - Object: a tervrajz alapján létrejövő konkrét építmény
- Biztonsági szempontból ez a megoldás **fekete doboz**.
- Az osztály/objektum másik definíciója: olyan dolog, aminek van:
  - Állapota (state) 
    (Egyenleg és név)
  - Viselkedése (behaviour)
    (Terhelés és jóváírás)
  és
  - Azonosítható (identity)
    (például a rá hivatkozó referencia azonosítja)

Az objektumorientált programozás előnye, hogy **felületet definiál**, amit a felhasználó kód hívni tud, és a felület alatt a megvalósítás lecserélhető.

```csharp
        static void Main(string[] args)
        {
            // var cegesBankszmla = new Bankszamla("Kerékgyártás KFT");

            // System.Console.WriteLine(cegesBankszmla.Nev);
            // cegesBankszmla.Jovairas(5000);
            // cegesBankszmla.Terheles(2000);
            // cegesBankszmla.Terheles(4000);

            var kamatozoBankszmla = new KamatozoBankszamla("Kerékgyártás KFT");

            System.Console.WriteLine(kamatozoBankszmla.Nev);
            kamatozoBankszmla.Jovairas(5000);
            kamatozoBankszmla.Terheles(2000);
            kamatozoBankszmla.Terheles(4000);
        }

```
Példának tekintsük mondjuk
- a tv távirányítóját
- rádió hangerőszabályozóját
  - feladat: hangerőszabályzás
  - felület: forgatni kell, hogy a hangerőt állítsuk
  - megvalósítás: lehet elektronikus potenciométer vagy optikai szenzor. Teljesen mindegy, amíg a hangerőt állítja.

### Szoftverfejlesztési módszertanok
A szoftverfejlesztés nem valódi mérnöki folyamat, hanem sokak szerint egy kaotikus tevékenység, ami időről időre mégis eredményt hoz.

![Project management](/Pics/ProjectManagement.jpg)

- [Projektek sikeressége 1](https://cdn2.hubspot.net/hubfs/542571/Doomed%20from%20the%20Start.pdf?t=1513091800721)
- [Projektek sikeressége 2](https://www.versionone.com/assets/img/files/CHAOSManifesto2013.pdf)

# Szoftverfejlesztési módszertanok / életciklus modellek

## két fontos állítás
- A módszertan használata növeli az elkészült rendszer minőségét. A nem megfelelő, vagy nem nem megfelelően használt módszertan azonban nem garantál semmit. (Nem biztosítja a projekt üzleti és/vagy felhasználói elégedettségi sikerét)

- Minden szoftver fejlesztést (előállítást) végző szervezet sorban ugyanazokon a fázisokon megy keresztül. Az elkészül szoftver minősége a szoftver (előállítási) folyamat minőségén múlik. (CMM Capability Maturity Model)

## Waterfall (vízesés) model (1970 Winston Royce)
[wikipédia cikk] https://en.wikipedia.org/wiki/Waterfall_model
- Amíg el nem készül egy fázis, addig nem lépünk tovább
- Ha egy fázisról elléptünk, visszafelé sosem lépünk
1. Rendszer követelmények
2. Szoftver követelmények
3. Előzetes tervek
4. Részletes tervek készítése
5. Fejlesztés és hibajavítás
6. Tesztelés
7. Üzemeltetés

Továbbfejlesztés: egyet vissza lehet lépni
Módosított:
- továbblépés előtt ellenőrzés
  - Verification: a követelményekkel szemben
  - Validation: a való világgal szemben

Nem lett túl sikeres, mire elkészült, addigra elterjedt a

## Spiral model (1988 Barry Boehm)
Négy terület 

[Összefoglaló kép](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ec/Spiral_model_%28Boehm%2C_1988%29.svg/333px-Spiral_model_%28Boehm%2C_1988%29.svg.png
)

- Célok, alternatívák és keretek meghatározása
- Alternatívák elemzése, kockázatok azonosítása és megszüntetése
- Fejlesztés és ellenőrzés
- Következő fázis tervezés

Iteratív fejlesztési módszertan, felkészül a szervezet arra a lehetőségre, hogy az első alkalommal nem sikerül tökéletes végeredményt előállítani.

## Agile (agilis) szoftverfejlesztés

## [Kiindulási elvek](http://agilemanifesto.org/iso/hu/manifesto.html)
A jobb oldalon szereplő fogalmak értékesek, de a baloldalon szereplők még értékesebbek.

- _Egyének és együttműködésük_ a folyamatok és eszközök helyett
- _Működő szoftver_ az átfogó dokumentáció helyett
- _Együttműködés az ügyféllel_ a szerződés érvényesítése helyett
- _Reagálás a változásokra_ a terv követése helyett.

## [Alapelvek](http://agilemanifesto.org/iso/hu/principles.html)
Az Agilis Kiáltványt alkotó elvek

- Legfontosabbnak azt tartjuk, hogy az ügyfél elégedettségét a működő szoftver mielőbbi és folyamatos szállításával vívjuk ki.
- Elfogadjuk, hogy a követelmények változhatnak akár a fejlesztés vége felé is. Az agilis eljárások a változásból versenyelőnyt kovácsolnak az ügyfél számára.
- Szállíts működő szoftvert gyakran, azaz néhány hetenként vagy havonként, lehetőség szerint a gyakoribb szállítást választva.
- Az üzleti szakértők és a szoftverfejlesztők dolgozzanak együtt minden nap, a projekt teljes időtartamában.
- Építsd a projektet sikerorientált egyénekre. Biztosítsd számukra a szükséges környezetet és támogatást, és bízz meg bennük, hogy elvégzik a munkát.
- A leghatásosabb és leghatékonyabb módszer az információ átadásának a fejlesztési csapaton belül, a személyes beszélgetés.
- A működő szoftver az elsődleges mércéje az előrehaladásnak.
- Az agilis eljárások a fenntartható fejlesztést pártolják. Fontos, hogy a szponzorok, a fejlesztők és a felhasználók folytonosan képesek legyenek tartani egy állandó ütemet.
- A műszaki kiválóság és a jó terv folyamatos szem előtt tartása fokozza az agilitást.
- Elengedhetetlen az egyszerűség, azaz az elvégezetlen munkamennyiség maximalizálásának művészete.
- A legjobb architektúrák, követelmények és rendszertervek az önszerveződő csapatoktól származnak.
- A csapat rendszeresen mérlegeli, hogy miképpen lehet emelni a hatékonyságot, és ehhez hangolja és igazítja az működését.

## IDEAL model
[wikipédia cikk](https://en.wikipedia.org/wiki/IDEAL_model)

[pdf](https://www.sei.cmu.edu/library/assets/idealmodel.pdf)

- Initial
- Diagnosing
- Establishing
- Acting
- Learning

# Software Capability Maturity Model (SCMM)
A szoftver előállítási képesség érettségi modellje
Minden szoftver fejlesztést (előállítást) végző szervezet sorban ugyanazokon a fázisokon megy keresztül. Az elkészül szoftver minősége a szoftver (előállítási) folyamat minőségén múlik. (CMM Capability Maturity Model)

## Initial (Level 1)
Fekete doboz, az eredmény előrejelezhetetlen, a siker pedig egyéni odafigyelés és erőfeszítés eredménye.

## Repetable (Level 2)
Fekete dobozok sorozata. Van kód újrafelhasználás, _hasonló_ projektek esetén megismétlődik az eredmény, az alpfető fejlesztési folyamat felismerhető.

## Defined (Level 3)
Szabványos: nincs fekete doboz, a folyamat _áttekinthető_. A folyamat dokumentált, szabványosított, és szoftver előállításhoz minden projekt a szervezet szabványos folyamatait használja.

## Managed (Level 4)
Előrejelezhető: A folyamat és a termék mennyiségi mérőszámokkal ellenőrzött. A folyamat méréséhez és megítéléséhez részletes adatok állnak rendelkezésre.

## Optimizing (Level 5)
Folyamatosan fejlődő: A folyamat részei cserélődnek, a rossz kikerül.

# System Development Life Cycle (SDLC)
A biztonsági szempontokat a rendszer teljes életciklusa során érvényesíteni kell.
## Conceptual definition
Egyszerű definíció, amit minden _érdekelt fél_ elfogad. (Pl. Fejlesztők, ügyfelek, vezetők)

- Tisztázza a rendszer célját és az alapvető követelményeket.
- Nem kell, hogy hosszabb legyen néhány bekezdésnél.

## Functional requirement determination
Egy lista a fejlesztendő rendszer képességeiről. Fontos, hogy minden _érdekelt fél_ elfogadja.
Ez adja az alapot a fejlesztéshez, és a fejlesztés végén jó eszköz, az eredmény ellenőrzéséhez.

## Control specification development
- Tervezni kell, hogy csak a jogosult felhasználók érhessék el a rendszert, és ők se léphessék túl a jogosultságukat.
- A bizalmas adatokat biztosítani kell megfelelő titkosítással.
- A számonkérhetőség és nyomozhatóság érdekében a rendszernek megfelelő _naplót_ kell vezetnie.
- A rendszer rendelkezésreállását a fontosságának megfelelően kell biztosítani.

Ez a tevékenység nem egyszeri alkalom, ha változik a terv újra kell végezni.

## Design review
A működési struktúra (pl. üzemeltetési környezet) elemei közti kommunikáció meghatározása, kezdeti ütemtervek kidolgozása.

Ha a terv kész, akkor az összes résztvevővel felül kell vizsgáltatni, hogy a tervek jók-e?

## Code review walktrhough
A fejlesztés során érdemes a fejlesztett kódot több szemmel megvizsgálni.

## System tests review
A fejlesztők és a felhasználók ellenőrzik, hogy a rendszer az általános, és a szokatlan/ritka feltételek mellett is az elvárt eredményt adja. A forgatókönyveket előre rögzítik.

## Maintenance and change management
A rendszer üzembehelyezésétől kezdve folyamatosan biztosítani kell a működőképességet változó környezetben. Fontos, hogy minden változás formális változás felügyeleti programban történik.

# Change Management
## Request control
Ki kérheti, költség/előny analízis, prioritás meghatározása.
## Change control
A fejlesztő elemzi, teszteli, dokumentálja és a telepítéshez _eszközt ad_.
## Release control
Ha a fejlesztés elkészült, akkor ez befogadja. 
- Fontos lépés annak ellenőrzése, hogy a fejlesztés során használt debug kapcsolók és egyéb hibakezeléshez használt eszközök kikerültek az eredményből. 
- Acceptance testing: a változás a felhasználók számára érthető és használható
## Security administration
- Configuration identification
- Configuration control
- Configuration status accounting
- Configuration audit

# Software Testing
## Methods
- Black-box
  nem ismerjük a belső felépítést (forráskódot)
- White-box
  birtokában vagyunk a forráskódnak.
- Gray-box
## Static/Dynamic
- Dynamic
  a rendszert működés közben teszteljük
- Static
  a rendszer forráskódját elemzőprogramokkal vizsgáljuk
## Testing types
- Unit test
  Egyszerre egy meghatározott részt tesztelünk, rögzített körülményekkel
- Integration test
  Az együttműködő részek mőködését vizsgáljuk a követelmények szerint.
- Acceptance
  Vizsgáljuk, hogy a rendszer megfelel-e a felhasználó/ügyfél kéréseinek.
- Regression
  Egy-egy változás után a rendszerünk még mindig megfelel-e képességekben (kiszolgált funkciókban, működésben), teljesítményben és biztonsági kérdésekben az elvártaknak.

# API (Application Programmint Interface)
Szolgáltatás használatához ad egy felületet, amit nem egy felhasználó (ember) hanem egy alkalmazás ér el (pl.: email szolgáltató/adatbázis/felhőszolgáltatás). A külső szolgáltatóknál azonosítani kell magunkat, hogy jogosultak legyünk használni a szolgáltatást. Az azonosításhoz pedig titkot kell használni (név+jelszó, api kulcs, stb.)

```
        +---------------------+                                           +------------------+
        | Szolgáltatás        |                                           | Alkalmazás       |
        |---------------------|                                           |------------------|
        |                     |                                           |                  |
        |                     |                                           |                  |
        |                     |                                           |                  |
        |                     |                                           |                  |
        |           +---------+                                           |                  |
        |           | API     |<--------HTTP/HTTPS-----------------------+|                  |
        |           |---------|                                           |                  |
        |           |         |+----XML/JSON----------------------------->|                  |
        |           |         |                                           |                  |
        +-----------+---------+                                           +------------------+
```
A titkokra nagyon kell vigyázni! A titkokat

pl:
- fejlesztés közben a fejlesztőnek az eléréshez használnia kell
- a tesztrendszeren teszteléshez használni kell
- az éles rendszeren használni kell

DE 
- a fejlesztőnek nem feltétlenül kell tudnia
- a tesztelőnek nem feltétlenül kell tudnia
- a felhasználónak nem feltétlenül kell tudnia

Első lépés: a [forráskódtól/telepítéstől függetleníteni](http://netacademia.blog.hu/2016/05/14/hogy_kerulhetjuk_el_a_szoftverpusztulast_12factor_app_3_beallitasok_konfiguraciokezeles) kell!

A felhőszolgáltatók lehetőséget adnak az alkalmazásbeállítások felülírására, így nem kell a telepítésnek a kulcsokat/titkokat tartalmaznia.

De használhatjuk a [Docker secret](https://docs.docker.com/engine/swarm/secrets/) megoldását, vagy az Azure-ban a [key vault](https://azure.microsoft.com/en-us/services/key-vault/).

A lényeg: sem a forráskód, sem a telepítő csomag ne tartalmazzon titkokat!

# Code repositories
- A hozzáférési jogosultságok kialakítása és ellenőrzése. A lekérdezési és módosítási jogosultságok megfelelő kezelése.
- Titok nem kerülhet be a kódtárba!!!!!

# DevOp
[12 tényezős alkalmazásfejlesztés](http://netacademia.blog.hu/2016/04/16/hogy_kerulhetuk_el_a_szoftverpusztulast_12factor_app_1_a_kodbazis)
[12 Factor App](https://12factor.net/)

```

                    +----------------------------------+
    +----------------------+                            |
    |                |     |                            |
    |                |     |                            |
    | Quality        |     |                            |
    |  Assurance     |     |                            |
    |                |     |       Software Development |
    |                |     |                            |
+---|----------------|--+  |                            |
|   |                |XX|  |                            |
|   |                |XX|  |                            |
|   +----------------------+                            |
|                    |  |                               |
|                    |  |                               |
|                    +----------------------------------+
|  Operations           |
|                       |
|                       |
+-----------------------+
```
Nincs külön fejlesztés, üzemeltetés és minőségbiztosítás, hanem a feladat az, hogy minőségileg biztosított kockázatmentes és nagyon sűrű telepítés és biztonságos üzemeltetés jöjjön létre.

# SLA - Service Level Agreements
A rendszer üzemeltetőjével szemben támasztott elvárások gyűjteménye.
- System Uptime (Rendszer rendelkezésreállása -a teljes működési idő százalékában)
  Példa: 1 nap 24 óra, 24*60=1440 perc, 1440*60=86400 másodperc. Ennek 1%-a: 864 másodperc=14 perc 24 másodperc.
  99%: napi 14 perc 24 másodperc maximális kiesés
  99.9%: napi 1 perc 26,4 másodperc
  99.99%: napi max 8,64 másodperc
- Maximum consecutive downtime (Maximális egybefüggő állásidő - másodpercben, percben, stb.)
- Peak load (legnagyobb terhelés)
- Average load (átlagos terhelés)
- Responsibility for diagnostics (A diagnosztikáért felelős)
- Failover time (áthidalási idő, amennyiben redundancia van a rendszerben)

# Input Validation
Beviteli adatok ellenőrzése. Cél: a nem használható adatok, be se kerüljenek a rendszerbe.

dotnet template segítségével webalkalmazást készítettünk
```
dotnet new mvc --auth Individual --use-local-db
```
## Data Type
A kapott adat típusa megfelelő. (pl.: Szám, szöveg, dátum, stb.)
## Data range
A kapott adat a megadott határokon belül van
## Data formatting
A kapott adat megfelel formai szabályoknak (pl. email cím)
## ID Number
Azonosító, telefonszám, stb. adatok bevitelénél

# Good Coding Practices
## Least privilege
Mindig csak azokat az erőforrásokat kell elindítani, megnyitni, stb., amik valóban szükségesek. Ne futtassuk az alkalmazásunkat admin-ként.

## Hiding secrets
Nem szabad a titkokat napvilágra engedni. Titkos adatokat nem szabad nem titkosított módon tárolni.

## Layered defense
Nincs egyetlen megoldás, használjunk olyan sok egymást támogató megoldást, amennyit csak lehet.

## Weakest link
Részegységenként kezeljük a biztonsági rendszert, az egyes függőségeket külön-külön. Az összetett rendszer olyan biztonságos, mint a leggyengébb részegysége.

# Expert systems (Szakértői rendszerek)
Több tanulmány kimutatta, egy jól beállított szakértői rendszer jobb döntéseket képes hozni, mint egy emberi szakértő.

- nem terheli emóció
- mindig rendelkezésre áll

## Knowledge base systems
Két részből állnak

### Knowledge base
- ha a hurrikán 4-es vagy annál magasabb kategóriájú vihar, akkor az árvíz eléri a tengerszint feletti 20 láb magasságot
- Ha a hurrikánnal járó szél meghaladja a 120 mérföldet, akkor a favázas épületek megsemmisülnek
- Ha a hurrikán szezon később kezdődik, akkor a vihar erősebb lesz, ha megközelíti a partot.

### Inference engine (következtető motor)
4-es kategóriájú vihar érkezik + a szél sebessége 140 mérföld => evakuálás

## Forward chaining
megadjuk a megfelelő adatokat, majd a rendszer levonja a következtetéseket

## Backward chaining
Egy elméletet képes ellenőrizni: megadjuk a végeredményt, és visszaadja, hogy ehhez milyen bemeneti adatok esetén következik a végeredmény.

## Fuzzy logic ([elmosódott halmazok logikája](https://hu.wikipedia.org/wiki/Elmos%C3%B3dott_halmazok_logik%C3%A1ja))

példa:

```
            ^
Igaz    +---+--+
        |      |
        |      |
        |      +-----+
        |            |
        |            |
        |            |
        |            +------+
        |                   |
        |                   |
        |                   +------+
        |                          |
        |                          |
        |                          |
        |                          |
        |                          |
        |                          |
        |                          +----+
        |                               |
        |                               |
        |                               |
        |                               |
Hamis   +----5----10---15---20----25---30---35----40------------------------------------>
```

- Nem csak igaz és hamis létezik, hanem átmenet: egy igazságért, annak a bizonyosságnak a foka, hogy az adott állítás igaz vagy nem.
- Fuzzyfication
  Egy függvény, ami minden bemeneti elemhez és állításhoz az igazságértéket rendeli.
- Inference
  Hozzárendeli az igazságértéket az adott állításokból összeállított következtetéshez.
- Composition
  Minden lehetséges szabálycsomagot összeállít.
- Defuzzication 
  Az adott szabálycsomag igazságértékét számolja ki.

Sikerek:
- Japán gyorsvasút fékezőrendszere (1985 Hitachi)
- Altatási mélység szabályozás

lásd: [Philips kapitány (Captain Phillips, 2013, Tom Hanks)](https://www.youtube.com/watch?v=vnjOjTzrjOw)
Isty: eredeti pilóta neve: Chesley B. Sullenbergert

## [Artificial Neural Networks](https://en.wikipedia.org/wiki/Artificial_neural_network) ([Mesterséges neuronhálózatok](https://hu.wikipedia.org/wiki/Neur%C3%A1lis_h%C3%A1l%C3%B3zat))
Lineáris számítási igényű, eredménnyel tanítható
```                                                                                                 Tanítás
        +---+                                                      +--+
        | 1 | +                 +---+                   +--------->| 1|                             1 <- ezen nem kell sokat változtatni 
        +---+ |                 |   | +-----------------+          +--+
              ++----0.7---->--> |   |
              |                 +---+
        +---+  |                                                   +--+
        | 0 |+->                                                   | 1|                             0 <- Az ide vezető útvonalak súlyát növelni kell 
        +---+  |                                   +-------------->|  |
              |                                   ^               +--+
              |                                   |
        +---+  |                +----+             |
        | 0 |+>+----0.1-------> |    |+------------+
        +---+         ^         |    |             |               +--+
                      |         +----+             |               | 0|                             1 <- az ide vezető útvonalak súlyát pedig csökkenteni kell
                      |                            +-------------> |  |
        +---+         |                                            +--+
        | 1 |         +
        |   |+-------->+
        +---+          |        +-----+                            +--+
                       +------->|     |+-------------------------->| 1|                              1 <-ezen nem kell sokat változtatni
        +---+          ^        |     |                            +--+
        | 0 |          |        +-----+
        |   |+-------->+
        +---+

  Input réteg              Számítási réteg                    Output réteg
```
Három réteg
- Input
- Számítási
- Output

- A számítási réteg több rétegből állhat.
- Az egyes "neuronok" értékeit a kapcsolatok súlyával megszorozva (lineáris kombinációt véve) adja a cél neuron állapotát
- Az eredmény és az elvárt eredmény különbségét felhasználva

### igazolt eredmények
- hangfelismerés
- kézírás felismerés
- arcfelismerés
- időjárás előrejelzés
- összefüggések/következmények feltárása

## DSS: Decision Support Systems
Nem műveletet végez hanem információt szolgáltat. Gyakran szakértői rendszer van mögötte. Biztonsági alkalmazásokban is használják.
