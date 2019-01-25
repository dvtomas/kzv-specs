Dobrý den,

tak jsem si to mnohokrát pročetl, abych aspoň trochu porozuměl tomu, jak je to koncipováno.  Moje otázky a připomínky nejsou úplně 100% promyšlené, hodně věcem jsem nerozuměl, a lepší odpovědi budu asi schopný až v další iteraci. Jen tak bokem: proč je některý text černě a některý modře?

Abych si ujasnil: Je to, co nyní posíláte, vlastně už předběžný draft oficiální dokumentace komunikace, o kterém očekáváte, že se po několika iteracích touto dokumentací stane, nebo je to spíše takové mailování pro nás, abychom si ujasnili, jak se to bude chovat, a skutečnou dokumentaci potom teprve budete psát?

Jinak, zde je moje nůše dotazů a názorů. Věci co mi přijdou OK nekomentuji, takže celý tón mé odpovědi bohužel z principu vyznívá negativně, není v tom nic osobního, jen snaha dobrat se výsledku. 

# 1. a., b.

Rozumím tomu správně, že příznaky "připravenost MS" a "stav MS", jak je nazýváte, dohromady kódují následující 3 stavy:

MS není připraven
MS je připraven, ale neměří
MS měří

? 

Potom navrhuji mít místo těchto dvou flagů prostě jeden příznak stavu, NotReady|Ready|Measuring. Bude to srozumitelnější a nebude možné vyjádřit nesmyslný stav "MS není připraven, ale měří".

# 1. c., d.

Předpokládám, že tyto informace má smysl posílat pouze ve stavu Measuring, je to tak? Struktura zpráv MS by to asi měla respektovat, tj. bude to nějaký (tagged) union? Pokud s tím nějak souvisí vaše poznámka *"Při návrhu zpráv z MP-Host se snažím, aby byl co nejmenší počet jejich typů. Věřím přitom, že délka UDP zpráv není při přenosu po LAN tak kritická."*, já se kloním spíše k tomu navrhnout datové struktury tak, aby věrně a korektně odrážely modelovaný problém. V samoúčelném omezování počtu typů nevidím přínos. Co se týče délky UDP zpráv, úplně pro jistotu bych se snažil nepřekročit 512 bajtů (viz např. [zde](https://stackoverflow.com/a/1099359/3220468)).

Co se týče názvu a ztráty trasové informace: Jak si představujete, že by mohlo ke ztrátě dojít? Programové chyby bych neuvažoval, ty mohou vzniknout jakékoliv, a nikdy se nedají všechny ošetřit, pouze opravit. Chyby právě navrhovaného protokolu (race conditions apod.) vyřešme nyní správným návrhem protokolu. Pokud máte přeci jen strach z nespolehlivosti UDP, byl bych pro použití spolehlivého transportu (TCP), než nespolehlivost UDP ručně látat o vrstvu výše. Pokud máte jiný důvod, rád bych ho znal. Jinak nejsem moc fanda předběžných ad-hoc oprav něčeho, o čem ani není jasné, jakým mechanismem se může rozbít.

# 2.

 - a. nevím co je tposit
 - b. Nerozumím, co mám s tím Názvem dělat. Celé to povídání o "rekonstrukci trasové informace" rozprostřené na různých místech dokumentu mi není jasné. Mohl byste možná v samostatném odstavci podrobněji rozvést jen tuto rekonstrukci?
 - c. Nerozumím větě "zadáváno ve dvojicích začátek/konec"

# 3.

Nerozumím větě *"pokud dojde k nějaké změně některé z jeho položek s výjimkou nepravidelnosti samotného staničení (kilometráže)"*. Můžete ji prosím zkusit nějak přeformulovat?

Dále - abych si udělal obrázek: Vyskytuje se u vás rozdělení trasových zpráv na bodové a liniové? (Tj. bodová je třeba návěstidlo nebo změna KM, liniová je např. nástupiště, které někde začíná a jinde končí)? 
 
 b. Viz hmmm výše - hmmm nikde výše nevidím, ani nevím co by to mělo být
 c. obsah tsection jsem nikdy neviděl, mohl byste mi prozatím poslat na ukázku alespoň LM3?

# 5.

Tady je mi to celé bohužel trochu nesrozumitelné, přitom jde asi o klíčový bod celého dokumentu. Mohl byste možná body proložit ještě informacemi o tom, co kdy dělá obsluha?

 2. Znamená to, že MP **nevysílá** žádné stavové zprávy, dokud se nezafixuje postavení?
 3. viz PreSyncInfo: Nikde preSyncInfo nevidím. Co je to hmmInc? 

...
Kde (a v kterém bodě) se dozvím, jak se má měření jmenovat? (tj. timestamp startu měření?) V bodě 8?

Co je to, a k čemu je mi užitečné MeasuringActiveCh ? 

Může se někdy stát, že MeasuringActive != !preSyncFlag? Za jakých okolností?

...

**Není mi jasné následující:** Skončilo měření, MS dopsal HostOut.tmp a shodil Ready, čímž indikuje, že HostOut je připravený. Nyní je MS připravený na další měření. Znamená to, že má poslat zase status s Ready=1? Co když MP prošvihne packet s Ready=0, bude si pořád myslet, že se do HostOut.tmp píše...? Nebo jsem opoměl nějakou indikaci přechodu mezi stavem ukončování měření a připraveností k dalšímu?
