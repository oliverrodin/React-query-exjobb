# chtgptexjobb


State management i React.js med hjälp av React Query 
 
 
 

 

Ett examensarbete utfört av: 

Oliver Rodin 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 
 

 

 

1. Sammanfattning 

 

 

 

 

 

 
 

 

 

2. Innehållsförteckning 

 

​​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​ 

​​ 

 

 

 

 

 
 

 

3 Introduktion 
 

3.1 Bakgrund 
 

Idag är efterfrågan på skalbara och effektiva webbapplikationer (PWA:s) stort. State management och datahantering är en kritisk faktor för att utveckla en robust och lättunderhållen applikation, och det är särskilt viktigt för frontend-utvecklare att ha en djup förståelse för det konceptet. 

Jag har under min Lia hos Apptech 24 AB uteslutande arbetat i en av deras produkter, EasyStaff. Appen är en modern take på ett intranät där företag delar nyheter, chattar, bjuder in till aktiviteter, håller koll på bemanning med frånvarohantering och kalender. Allt detta i en trygg och säker miljö skyddat av BankID.  

Under arbetet med Easy Staff så har jag förstått vikten av state management och datakommunikation. Ganska snabbt såg jag en möjlighet att i mitt examensarbete fördjupa mig i detta. Dels för att bygga på min portfolio med relevant kunskap men även för att generera nytta gentemot Apptech. 

Efter noggrant övervägande tillsammans med min handledare så har det resulterat i en migrering av Easy Staffs nuvarande datalager (state management, data-fetching) bestående av diverse tekniker och bibliotek till en lösning som bygger på React Query.  

Då React Query har varit i pipen på Apptech ett tag är mitt examensarbete ett ypperligt testtillfälle av biblioteket för att i efterhand kunna utvärdera om det är något som ska implementeras i kommande React-projekt hos Apptech. 

 

3.2 Syfte  
 
I grunden är syftet att testa React Query som en möjlig lösning på ett återkommande problem i dagens komponentbaserade frontend-utveckling, state-management. 

Utöver det är syftet också att fördjupa mina egna kunskaper i React Query och state-management i stort. Vilket förhoppningsvis kommer att gynna mig i min kommande karriär. 

Genom att lyfta in biblioteket i en redan existerande applikation så känner jag att mitt syfte hamnar i fokus, då jag inte behöver tänka på att utveckla en grund i form av en databas, en backend och ett UI utan jag kan fokusera uteslutande på fördjupningen i React Query och dess möjligheter för att centralisera datahantering och kommunikation med backend. 

 

 
 

3.3 Avgränsning 
 

Jag kommer inte att utveckla en egen applikation från grunden. 
 

Jag kommer inte att vidareutveckla Easy Staff funktionsmässigt utan jag kommer att fokusera på att implementera och utvärdera React Query i den redan existerande funktionaliteten. 
 

Jag kommer att använda mig av Easy Staffs redan existerande API och inte ändra någon form eller struktur i det. Jag vill se om React Query kan anpassa sig till de redan existerande behoven.  
 

Jag kommer använda React Query för att hantera och centralisera data, och kommer inte undersöka andra möjliga lösningar eller state-managementbibliotek.  
 

Jag kommer eventuellt att behöva ändra omfattningen av implementationen under arbetets gång på grund av tidsbrist. 

 

3.4 Metod 

För att uppnå syftet med examensarbetet och testa React Query som en möjlig lösning på state-management i Easy Staff, har jag valt att följa en iterativ arbetsprocess. 

Först har jag genomfört en noggrann research av React Query, dess fördelar och begränsningar, och hur det kan appliceras i Easy Staff. Efter researchen implementerades React Query i Easy Staff och fördelar och nackdelar dök upp. 

Efter implementeringen har jag testat och utvärderat resultaten genom att mäta prestanda, användarupplevelse och utvecklarupplevelse. Jag har också genomfört en jämförelse med Easy Staffs tidigare state-managementlösningar för att kunna utvärdera hur React Query presterar i jämförelse med den redan befintliga lösningen. 

 

 
 

4 Genomförande  

 

4.1 Introduktion till React Query och dess funktionalitet 
 

React Query är ett kraftfullt opensource-bibliotek som skapades av Tanner Linsley 2019.  
Det hanterar hämtning, cachning, synkronisering och uppdatering av server states. 

Överlag så ger biblioteket användaren möjligheter till att bygga snabba, responsiva applikationer med förbättrad UX. Detta genom att minska antalet nätverks-requests, förenkla datahantering, förbättra prestanda och användarupplevelsen. Biblioteket erbjuder många funktioner som trotts sin komplexitet är anpassningsbara till en mängd olika användningsområden. 

 

4.1.1 Kommunikation 
 

React Query kommunicerar mellan frontend och backend, det kan hantera en mängd olika datakällor så som REST-API, GraphQL och websockets. I denna rapport kommer jag dock att utgå från det första alternativet med hjälp av axios. 
Kommunikationen sker oavsett datakälla genom React Querys inbyggda hooks, här nedan kommer två exempel på hur det kan se ut. Första exemplet hämtar data från källa och det andra skickar data till källa. Gemensamt för de båda är att de är inkapslade i en custom-hook för att man ska kunna dela logiken bland flera komponenter i applikationen och på så sätt inte behöva repetera kod:  
 
 

 

 
 

 

 

När applikationen använder en RQ-hook för att hämta data anges en unik “query-key” för varje unik dataförfrågan. Denna query-key används sedan för att identifiera dataförfrågningen och för att cachea datat för vidare användning. 

När en dataförfrågan görs så returneras det omedelbart utan att förfrågan skickas om det redan skulle finnas i cachen. Om datat inte finns i cachen skickas nätverksförfrågan till API:t för att hämta det från databasen och när hämtningen gått igenom så cachas det automatiskt av RQ under den angivna nyckeln.  

Kombinerar man RQ:s teknik med egna custom-hooks som i exemplen ovan så kan samma data enkelt hämtas av flera olika komponenter i React så länge man anger rätt query-key i frågan. Detta minimerar antalet nätverksförfrågningar och förbättrar prestandan avsevärt. 

 

4.1.2 Cache 
 

React Querys cache-system har nämnts lite i förgående stycke men här beskrivs det lite mer ingående. Cachen fungerar på så vis att den lagrar data i minnet under en begränsad tid, default så lagras datan i 5 minuter men caheTimen kan ställas på varje useQuery individuellt om så önskas. Detta gör såklart att det går extremt snabbt för applikationen att rendera ut data på skärmen som har hämtats tidigare utan att det behöver göras nya nätverksförfrågningar. 

Utöver detta går det att konfigurera cachen på flera sätt, några exempel är cachTime som nämndes ovan, alltså hur länge ska datat ligga i cachen, man kan ställa in en specifik lagringstyp t.ex. “localStorage” eller “sessionStorage” och bestämma vad som ska ske när cachen blir full.  

 
 

React Query har ett tillhörande bibliotek som heter React-Query-DevTools som ger en fantastisk överblick av vad för data som är cachat under vilka nycklar, om de är mountade och vilken status varje cachenyckel har. För att förenkla arbetet med RQ och förstå bibliotekets fullständiga potential bör devtools användas, det är varje RQ-utvecklares bästa vän och ett fantastiskt kompliment från RQ-teamet.  

 

4.1.3 UX-förbättringar 

 
Utöver hämtning, cachning, synkronisering och uppdatering av server states kan React Query även förbättra användarupplevelsen (UX) på andra sätt. Till exempel hanterar RQ automatiskt laddningstillstånd och felhantering, vilket leder till att det blir lättare att bygga användarvänliga gränssnitt som kan förse användarna med viktig feedback under tiden som data hämtas från databasen.  

RQ erbjuder även funktioner som:  
- Polling - med hjälp av RQ:s refetch-funktion kan hämtning av data utföras med ett förbestämt tidsintervall. 
- Pagination/Infinite Scroll – Att rendera paginerat data förekommer väldigt ofta idag, detta förenklar RQ med bara några få simpla förändringar i useQuery-hooken. 
- Optimistic Updates  -  Uppdatera Cache innan servern, UI kan renderas om direkt. 
 

Dessa och många andra inte än nämnda funktioner underlättar utveckling av komplexa applikationer som bygger på uppdateringar i realtid. 

Överlag så förenklar RQ drastiskt hanteringen av asynkront serverdata på ett sätt som inte många andra state-managementbibliotek gör. Genom att centralisera serverdata i en och samma cache fast under egna unika query-keys har vi tillgång till samma data från hela applikationen och om vi använder oss av RQ:s hooks i egna custom-hooks så behöver inte likvärdig kod repeteras utan det är bara att anropa rätt hook för att få tillgång till rätt data.   

Detta leder till att utvecklare kan fokusera mer på att bygga användarupplevelsen och mindre på att hantera data och nätverksförfrågningar då mycket av logiken för detta sker under huven på RQ:s hooks. Detta kan leda till kortare utvecklingstider och mer robusta applikationer som över tid är lättare att underhålla. 

 

 

 
 

4.2 Implementering av React Query i Easy Staff 

Easy Staff är ett modernt intranät för kommunikation på arbetsplatsen. Applikationen består av 8st moduler: 

Chatt 

Nyheter 

Karriär  

Utveckling 

Anställda 

Lunch 

Admin/Profil 

Kalender 

Utöver dessa finns det även lite funktioner som login/logout, notifikationer och autentisering. Det var i ordningen ovan jag implementerade biblioteket i applikationen, modul för modul. 

Mitt mål efter första veckans fördjupning av React Query och genomgång av Easy Staff blev direkt att jag skulle implementera biblioteket genomgående i hela applikationen. Eftersom arbetet i de olika modulerna har varit snarlikt så kommer jag att beskriva tillvägagångsättet med hjälp av chatt-modulen här nedan. 

Vecka 2 började jag implementeringen med att installera biblioteket i applikationen och få den extremt lilla delen med boilerplate-kod på plats för att kunna börja använda mig av biblioteket. 
  

 

 
 

Detta är den boilerplatekod som måste vara på plats för att kunna börja använda sig av RQ. Utöver boilerplate-koden så satte jag också en grund för hur jag ville ha RQ-koden strukturerad i dokumentträdet. Det slutade med att jag skapade en “hooks”-mapp i varje modul och delade upp logiken på ett modulplan, RQ-logik för nyhetsmodulen fick bo i den modul-mappen.  På så sätt tycker jag att man fick en bra överblick av det hela. 

 

4.2.1 Hämta data 
 

Efter installationen var det bara att börja byta ut kommunikationen med backenden och basera den på RQ-hooks som kapslades in i egna custom-hooks som i exemplen i början. Jag valde att lägga upp logiken på sådant vis för att på ett enkelt sätt kunna dela den mellan komponenter i applikationen. Här nedan ser vi ett exempel från min kod, där jag använder mig av RQ:s useInfiniteQuery-hook som finns för att på ett enkelt sätt hämta bitar av data som sedan fylls på vart efter skärmen skrollas nedåt.  

 
 

 
 

 

Det tillvägagångssättet kompletterar också RQ-cachen på bästa sätt. Vill jag hämta mina states i detta fall min chatt-lista ifrån cachen kan jag enkelt göra det med hjälp av mina custom-hooks och detta gäller genomgående i hela applikationen.  
Här nedan hämtar jag in mina chattar till ChatListPage-komponenten för att sedan rendera ut dom i UIt:  

 

 
 

 

4.2.2 Uppdatera data 
 

När jag kände att jag fått koll på hur jag hämtar data från servern var det dags att angripa uppdatering av data. Även här görs detta med hjälp av custom-hooks men här har jag istället kapslat in RQ:s useMutation-hook för att uppdatera/mutera data. Till skillnad från när man hämtar data så vill man ofta utföra olika efteraktioner när man uppdaterat datat. Detta löser man med olika callbacks i useMutation-hooken, om datat laddas upp korrekt kan man med hjälp av onSucces hämta om listan med chattar i detta fall för att rendera ut den nya chatten man precis skapat. Men om något skulle gå snett och useMutation skulle kasta ett error så finns möjligheten att plocka upp detta i en onError-callback där man kan meddela användaren att något gått snett och konsol-logga error-meddelandet för utvecklarnas del. Här ser ni ett exempel på en custom-hook som laddar upp en ny chatt till databasen.   

 

 

 

Men om man gömmer useMutation-hooken i en custom-hook hur kan man då utföra olika actions från specifika komponenter? Det är ju oftast där UI och client-states finns, behöver man skicka dessa som argument till custom-hooken? Svar nej, här glänser RQ. Till skillnad från när man hämtar data och anropar en useQuery som körs direkt automatiskt så är det nu utvecklaren som bestämmer när useMutation-hooken ska köras. Har man en komponent som har funktionen att uppdatera data anropar man den specifika custom-hooken från komponenten och när det är dags att köra uppdateringen så anropar man en funktion som heter mutate(), det är den funktionen som bestämmer när useMutation-hooken ska köras och i sin tur datat ska skickas till servern.  

 
 

 

Det är även i mutate-funktionen som man kan komma åt samma callbacks som i hooken, onSucces, onError och onSettled. Detta betyder att man kan sätta specifika actions för varje komponent som använder sig av samma custom-hook medans man i custom-hooken kan köra actions som man alltid vill köra oavsett varifrån hooken anropas. Exempel på komponentspecifika-actions skulle kunna vara att man vill stänga en modal, navigera till en ny rutt efter success eller meddela användaren att något gått snett om uppdateringen kastar ett error. Här ser ni ett exempel från min kod där mutate-funktionen anropas i en submitHandler som i sin tur laddar upp den nya chatten till databasen. 

 

 
 

 
 
4.2.3 State management  

Efter att ha implementerat React Query i hela chatmodulen har antalet lokala states i react-komponenterna minskat drastiskt, delar av dem har centraliserats till RQ:s cache och på så sett hämtas de nu in via respektive custom-hook. Loading- och Error-states hanteras nu i RQ:s logik vilket betyder att dom nu också hämtas in från samma ställe.   

Detta kanske inte ser ut som någon stor skillnad men koden till höger är efter RQ implementerats, komponenten har nu bara states som är direkt kopplat till UI:t. All mutering av data sker direkt i cachen.  

Den största skillnaden är dock den stora minskningen av tillfälliga states, sådana som har funnits för att hålla koll på ett booleanskt värde för en toggle eller ett aktivt objekt av någon typ. Dessa har helt plockats bort och hanteras nu direkt i cachen. Ovan är isPinned och isPrivate exempel på sådana, innan RQ så behövde dessa finnas för att sätta värdet på två stycken toggles, dels visade värdet det tillfälliga statet på varje toggle innan komponenten renderades om efter en submit. Sedan var det också dessa states som skickades med i submiten.  

 
 

 

Med RQ plockar istället de två toggle-knapparna värdet direkt från cachen och när någon av de ändras så uppdateras också cachen direkt med hjälp av “optimistic updates” vilket betyder att värdet i cachen uppdateras direkt på klick och UI:t renderas lika snabbt om till det nya värdet. Efter det skickas datat till databasen i bakgrunden, om uppdateringen returnerar success så utför cachen en bakgrundshämtning av datat och uppdateras och får statusen fresh. Men eftersom det nya värdet redan finns i cachen så är detta inte något som syns i UI:t. Skulle det vara så att uppdateringen skulle kasta ett error så utförs en rollback och togglen renderas om. Här nedan ser ni hur detta ser ut i kod i useMutation-hooken:  

 

 
 

 

4.2.4 Förbättra UX 
 

Med React Query har jag kunnat förbättra användarupplevelsen på flera plan. Flera förbättringar sker under huven på RQ, om en rutt har besökts av användaren så ligger dess data i cachen så går användaren tillbaka till samma rutt senare renderas rutten omedelbart med datat från cachen. Om användaren loggar in på Easy Staff för att kolla dagens lunch och sedan jobbar ett tag med annat, nästa gång hen går tillbaka till Easy staff-fliken i webbläsaren hämtas sidan om i bakgrunden men renderas bara om utifall nya data existerar. Samma sak sker efter en tillfällig nätverksdipp. Detta sker för att försäkra att användaren ska få ta del av den senaste informationen. 

Utöver det som sker under huven har utvecklaren många möjligheter att förbättra användarupplevelsen, en sådan förbättring är som beskrivet i paragrafen ovan, möjligheten att göra “optimistic updates” direkt i cachen.  

En annan möjlighet som finns är RQ:s möjlighet att hämta data innan UI:t renderas ut genom prefetch(). Detta har gjort det möjligt att plocka bort stora mängder loding-bars och spinners. Vilket har resulterat i ett rappare UI som känns mer behagligt för användarens öga. Ett exempel på en prefetch i chat-modulen är att när jag hovrar över chattens ikon innan den klickas på så prefetchas chattlistan och de pinnade chattarna vilket betyder att datat redan finns när användaren klickar på ikonen och chatt-modalen öppnas. 

 

Pretech funktionen fyller cachen på samma sätt som en hämtning med useQuery, så om de delar samma queryKey populerar de samma cache-entry. Koden för den tidigare nämnda prefetchen ser ut så här: 

 

 
 

 
Utvecklaren kan också sätta ett refetch-intervall på. vald query. Detta har jag utnyttjat i Chat-modulen för att få ett mjukare flöde i chatten. Så fort ett chattrum är öppet (mounted) så refetchas meddelandena varannan sekund. På så sätt upplevs chatten direkt och rapp. Det enda som behöver göras för att dra nytta av denna funktion är att sätta ett refetch-intervall i vald useQuery-hook:  

 

 

Det tillvägagångssättet jag beskrivit ovan med hjälp av chatt-modulen, hämta data, uppdatera data,  state-management och förbättra UX har jag använt mig av genom alla moduler i applikationen. Tack vare att jag började med en av de mest komplicerade modulerna har jobbet efter flytet på bra, min förmåga att se vad react query kan och inte kan tillföra har förbättrats modul för modul och slutresultatet av implementationen är jag nöjd med trotts att jag valt att utelämna en del smidiga funktioner pga en del yttre faktorer.  

 
 

4.3 Kod- och prestandajämförelse före och efter implementering 

4.3.1 Skillnad kod och utvecklarupplevelse 

Efter implementeringen av React Query har utvecklarupplevelsen märkbart förbättrats. Tack vare cachen har nu applikationen en centraliserad plats för server-states. Kommunikationen mellan komponenter och statesen sker genom custom-hooks vilket också underlättar användningen av datat enormt. Komponenter som behövar ta del av samma data kan nu göra det på ett enkelt sätt då det bara är att anropa den aktuella hooken och därifrån plocka datat som ska renderas vilket i sin tur också reducerar antalet anrop gentemot databasen.  

Innan implementeringen gjorde varje komponent egna api-anrop för allt data som behövdes, behövde komponenten data från tre endpoints så resulterade det i tre useCallbacks respektive useEffects(). Allt efter att applikationen växer blir komponenterna allt mer komplexa och till slut nästintill omöjliga att underhålla.  

I nuläget anropas istället tre custom-hooks. Genom att jobba på detta sätt plockas mycket av logiken bort ifrån react-komponenterna och delas upp i flera custom-hooks som tillsammans bildar likt ett servicelager för all logik. Det betyder att komponenterna blir mer rena vy-komponenter som hanterar renderingen av UI:t. I slutändan blir kodmängden likvärdig men den sprids ut och hamnar i mer logiska fack. Repot blir mer översiktligt och applikationen mer skalbar. Vill man i framtiden ersätta React Query med något annat bibliotek för att hantera state-management och data-hämtning så behöver man inte in i komponenterna och grotta. Antingen modifierar man det servicelager som redan finns eller så skapar man bara ett nytt som förser komponenterna med data av samma struktur. Framtidssäkrad kod är bra kod. 

Länk till komponentexempel med och utan React. Query. 
 

4.3.2 Jämförelse prestanda 

När det kommer till prestandan har jämförelsen varit mer utmanande, prestandamätningar kan vara komplicerade och är beroende av flera faktorer som hårdvara, din webbläsare och andra applikationer som körs på datorn. För att få exakta mätningar bör applikationen testas på olika enheter och i olika webbläsare. Tiden för detta har inte riktigt funnits under dessa veckor, fokuset har också automatiskt hamnat på användarupplevelsen då det är där React Query kan göra underverk. Eftersom så pass mycket är förkonfigurerat under huven på RQ är det också svårt att veta vad och hur man ska testa prestandan.  

Det som har gjorts i testväg är lighthouse-rapporter på koden före och efter implementeringen för att få ett konkret kvitto på eventuella prestandaförbättringar. I rapporterna har inte prestanda-skillnaden varit speciellt märkbar, dels för att applikationen lider av andra större problem som t.ex. hanteringen av bilder och en för stor DOM, så det är där lighthouse har lagt sitt fokus.  

Det som går att utläsa rent konkret av rapporterna är att efter implementeringen av React Query har performence ökat från 69 till 82 där det är “Total Blocking Time” som framförallt har förbättrats, från 310ms ner till 130ms. Detta betyder att tiden tills att användaren kan börja integrera med appen har blivit kortare och det låter som en mycket rimlig prestandaförbättring eftersom det är just det som React Query förbättrar med sin cache.  
Båda lighthouse-rapporterna kommer finnas medskickade som bilagor om det finns ett intresse av att läsa dem.  

 
 

Utöver lighthouse har jag också mätt omladdningstiden (page reload) på 5 av rutterna i applikationen där man kan se en genomgående förbättring av prestandan. På alla rutter har omladdningstiden minskat med en sekund. Se resultat nedan: 

Nyhetssidan: 
 
Kalendersidan: 
 
Utvecklingsidan:  
 

 
 

Personalsidan: 
 

Kärriärssidan:  
 

Efter de jämförelser som har gjorts är det bara att konstatera att prestandaoptimeringar inte bara handlar om siffror utan användarupplevelsen har en minst lika stor inverkan på resultat. Det är för slutanvändaren applikationen byggs och det största målet är att tillfredsställa dem.  

 
 
 

4.4 Utvärdering 

I grova drag har jag gjort det jag hade planerat i mitt examens-arbete, jag har fördjupat mig i React Query, samt byggt på med mycket kunskap om state-management överlag. Jag har lyckats implementerat biblioteket och min nya kunskap i en redan existerande applikation i detta fall Easy Staff. Mitt mål var att centralisera hela applikationens server states med hjälp av React Query, det har jag i stort sett lyckats med, bortsett från en modul (notifikationer) som jag valde att hoppa över men det fanns redan med i planen i planeringsstadiet.  

Om jag jämför det utförda arbetet med planeringen så har jag lyckats estimerat rätt så bra, det enda som har ruckat lite på utförande kontra planeringen har varit sjukdomar och VAB på hemmaplan. Det har inneburit att jag behövt jobba lite längre pass men slår man ut timmarna så har jag nog lagt ungefär en vecka på fördjupning i React Query för att sedan kunna klara av implementeringen av biblioteket under vecka två respektive tre för att sedan kunna slutföra rapporten under vecka fyra. 

Om jag läser igenom mitt syfte med detta exjobb så tycker jag det har uppfyllts så gott det går på fyra veckor. Jag har absolut ökat min förståelse samt belyst vikten av state management i React.js. Under implementationen av biblioteket har jag inte bara lärt mig hur states hanteras i det utan även vikten av att samla dessa på ett ställe för att öka skalbarheten i applikationen. Jag har också lärt mig skillnaden på client- och serverstates, tidigare hade jag inte ens reflekterat över att det gick att gruppera states på det viset och att det faktiskt finns skillnader i hantering av de. Client-states ägs av applikationen, all vetskap om statet bor i appen medan server-states ägs av servern, clienten lånar bar det senaste datat för att rendera ut det på skärmen. Detta betyder att vi måste vara mer bejakande när det gäller hanteringen av server states, vi måste hela tiden se till att vi renderar det senaste datat. Eftersom statets sanning finns på servern så måste det detta kommuniceras via nätverksförfrågningar och det är detta som React Query löser så bra med många av deras funktioner. Det ser till att du alltid har fräscht data utan att överproducera nätvärksförfrågningar, och de förfrågningar som görs sker ofta i bakgrunden och påverka inte slutanvändaren.  

Har jag nått mitt fördefinierade mål med ex-jobbet?  
Det tycker jag att jag har, det finns flera konkreta exempel på hur React Query kan effektivisera Easy Staff, både prestandamässigt men även rent kodmässigt. Många av de exemplen har jag tagit upp och påvisat i denna rapport. 

 

 
 

4.5 Analys 

 

Jag har fått mycket gjort under detta ex-jobb men det är även saker som jag hade velat göra som fallit bort av olika anledningar. Grunden sitter men det är framförallt funktioner i React Query som inte matchar med applikationens sedan innan existerande arkitektur. 

Det största bortfallet och det enda jag saknar har varit användningen av “optimistic updates” som faktiskt är en av React Querys absolut största “selling points”, många rör sig mot en användning av RQ tack vare detta så det är tråkigt att det inte har fungerat så bra för min del. Anledningen till detta är dels strukturen i Easy Staffs API men även applikationens arkitektur som inte alltid rimmar med kraven för att kunna utföra en “optimistic update”.  

Funktionen bygger på rena data bindings, ska man uppdatera en property i ett objekt så uppdateras hela objektet utifrån ID, på så vis kan man direkt hämta det cachade datat med samma id och uppdatera det innan man skickar det till servern. I mitt fall är API:t emellanåt strukturerat på ett sådant sätt så att en uppdatering av en property kan ha en egen put-funktion och således ett eget nätverksanrop. Detta betyder att det bara är propertyns värde och objektets ID som skickas i request-bodyn vilket betyder att man inte har hela objektet när man ska utför uppdateringen av cachen.  
Men då kan man ju spontant tänka, det är väl bara hämta det cachade datat med samma id, spreada det och ersätta rätt property med det nya värdet?  
Teoretiskt så funkar det absolut men då bygger det på att man har en statisk queryKey eftersom det är genom den man kommer åt rätt cache-entry i cachen och kan hämta och sätta det data man vill uppdatera. Jamen det borde ju funka kan man tänka, inga konstigheter.  

I många av mina fall fungerar inte detta heller då jag använder mig av useInfiniteQuerys med dynamsika queryKeys, det vill säga att det finns möjlighet att filtrera datat med en söksträng eller en vald kategori. 

 

Detta resulterar i att jag inte kan hämta det aktiva datat från cachen eftersom jag aldrig kan vara säker på hur min queryKey ser ut och hur ska jag då kunna uppdatera cachen?  

En lösning på problemet skulle kunna vara att jag hämtar det aktuella objektet som ska uppdateras med hjälp av en useQuery och en getById, då har jag en statisk queryKey: 

Då kan jag ju i min optimistic update bara hämta det objektet och sedan uppdatera det och retunera tillbaka till cachen. Det fungerar absolut, objektet uppdateras i cachen under ovanstående cache-entry men uppdateringen renderas inte ut förens efter att nyhetssidan har renderats om. Vad nu detta? Jag fattade ingenting i stunden jag satt och provade mig fram, men efter ett tag klarnade det för mig. 

 
 

Omrenderingen av nyhetsobjektet sker inte för att det är inte getById-datat som renderas på skärmen i grunden utan det data som är synligt på skärmen kommer från listan med nyhetsobjekt. 

Men om jag i min uppdatering hämtar listan från cachen och sedan spreadar listan och ersätter objektet med samma ID med mitt getById-objekt? Detta funkar inte heller för jag kan inte hämta listan från cachen eftersom jag fortfarande inte kan veta hur listans queryKey ser ut. 

Här börjar listan på olika tillvägagångssätt att ta slut men jag har en sista idé.  
Om jag när jag hämtar objektet med getById renderar om den valda nyheten med det datat så borde den optimistiska uppdateringen fungera väl? Ja det stämmer men så som nyhetslistan är byggd så öppnas inte en modal när man väljer en nyhet utan nyhetskortet blir bara större med hjälp av en animation vilket betyder att när getByIdn utförs så kommer hela nyhetslistan renderas om och det är något vi absolut inte vill då det resulterar i en extrem dålig användarupplevelse.  

 

Detta betyder att om jag inte gör drastiska omstruktureringar i UI:t så kommer det att bli svårt att använda sig av optimistic updates i nyhetslistan när jag t.ex. gillar en nyhet eller liknande. Vilket betyder att den actionen är efter implementeringen av React Query långsammare än innan. 

Det här var en av anledningarna till att jag från början ville prova att implementera RQ i Easy Staff då det finns en hel del såna här direkta aktioner. Det kanske finns en lösning på detta problem men ingen jag har hittat på denna korta tid.  

 

 
 

5 Erfarenheter och lärdomar 

 

5.1 Erfarenheter 

 
Efter samspråk med handledare på Apptech så är den största erfarenheten de har dragit av mitt ex-jobb att få se ett intressant nytt bibliotek in action i en applikation som finns ute på marknaden och i kombination med deras beprövade arkitektur och struktur. På så sätt kan ett eventuellt beslut tas av att behålla den tidigare strukturen eller efter mer noggrann utvärdering släppa den vidarearbetade versionen med React Query till produktion. 

En erfarenhet som jag personligen har dragit av projektet är att värdera vikten av arkitektur i större applikationer, en dålig arkitektur resulterar i en svårunderhållen kod och en mindre skalbar produkt medan en bra arkitektur underlättar vidareutveckling, reducerar underhållet och bidrar till en mer skalbar produkt. Det behöver inte vara ett nytt häftigt bibliotek som löser ett sådant problem utan lösningen kan vara att se på det man redan har med andra ögon och strukturera om.  

Ska man lägga ner extrem många timmar på att föra in ett nytt bibliotek i en redan fungerande applikation, så ska man vara säker på att det hjälper och inte stjälper, för det är inte bara att göra.  

 
5.2 Lärdomar  
 

En viktig lärdom som jag har tagit med mig från detta ex-jobb är att ett väl utformat API är en viktig faktor för att underlätta frontend-utveckling. Om API:et inte är bra designat blir arbetet tyngre och kan leda till mängder av muterat data i frontenden, oavsett hur många avancerade bibliotek som används. Detta påverkar i sin tur användarupplevelsen negativt. Jag tror att det är avgörande att ha en bra kommunikation mellan frontend- och backend-utvecklare för att lösa problem, så som var en viss typ av logik ska hanteras eller hur en datastruktur ska se ut för ett specifikt objekt. 

Denna lärdom har blivit extra tydlig för mig under ex-jobbet eftersom React Query fungerar bäst med ren data-binding. Om detta inte är fallet, måste man börja mutera state och utföra komplicerade åtgärder för att få biblioteket att fungera som det ska. 

En annan lärdom jag har tagit med mig är att det inte bara är att byta ut logik i en redan fungerande applikation. Innan jag började med detta projekt var jag nog lite för naiv och hade inställningen hur svårt kan det vara?  
Så här i efterhand har jag förstått komplexiteten i det.  

De ovanstående lärdomarna har breddat min förståelse för konceptet programmering och pointerat vikten av att apputveckling är ett lagarbete som bygger på lyhördhet, social kompetens och ett konstant driv framåt. Det är en insikt som jag tycker är väldigt skön, det kommer att förbättra min inställning till kommande arbetsliv då jag definitivt är en lagspelare som älskar sociala sammanhang. 

 
 

Rent faktamässigt är den största lärdomen jag tagit med mig är all kunskap jag har fått i React Query, jag tror att detta är ett bibliotek vi kommer få se mer och mer av framöver. Biblioteket förenklar verkligen hanteringen av datalagret i React-applikationer med hjälp av sin cache. Parar man sedan ihop den med egna custom-hooks så har man en skalbar och centraliserad helhetslösning för applikationens server-states.  

 

5.3 Projektet idag – utveckling och framtid 

 
Eftersom Easy Staff är en applikation som befinner sig i produktion så vidareutvecklas den hela tiden och har även gjort det under tiden jag arbetat med mitt exjobb. 

Efter denna rapport är klar ska jag presentera resultatet för mina kollegor på apptech, tillsammans ska vi överväga om React Query är något att bygga vidare på i Easy Staff eller ej.  

Sedan kommer jag jobba för att vi ska införa React Query i våran teknikstack inför kommande projekt. Biblioteket är lysande om du frågar mig och jag tror att det gör sig ännu mer rättvisa om man implementerar det från start i ett nytt projekt så att biblioteket får en större chans att samspela med arkitekturen från start. På så vis kan man utnyttja hela biblioteket till fullo.  

 

 
 

6 Referenser 

 

Den officiella React Query dokumentationen. 

Blog om React Query 

React Query artikell. 

Video om React Query. 

Video med grundaren av React Query. 

 

 
