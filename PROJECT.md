# PROJECT.md — Progetto LSMSDB 2026-2027

Large Scale and Multi-Structured Data Bases — Università di Pisa
Docenti: Pietro Ducange, Federico Frati
Titolo ufficiale: *Design and develop of an Application interacting with NoSQL Databases*

Materiale del prof (cartella `project/`):
- `LSMSDB_Project2026-2027.pdf` — specifiche del progetto
- `LSMSDB_Project IDEA Presentatio Template26_27.pdf` — template slide per la presentazione dell'idea
- `Large Scale and Non-Structured Databases ... ExampleIdeaPresentation.pdf` — esempio (Dev2Enterprise)

---

## 1. Cosa chiede il progetto

Progettare un'applicazione completa che gestisce un **"big dataset"** su un data store distribuito, con **almeno due architetture NoSQL** (**Document DB obbligatorio**). Il focus principale è il **livello dati distribuito** (modellazione e organizzazione). L'applicazione è un **servizio** che usa i driver nativi dei DB ed espone le funzionalità tramite **API RESTful**. Linguaggio e DBMS liberi (consigliati Spring, Swagger, Postman).

### Dataset
- **Reale**, volume **≥ 50/100 MB**.
- Almeno una tra:
  - **Variety**: più sorgenti e/o formati (es. recensioni da TripAdvisor + Booking)
  - **Velocity/Variability**: i dati perdono importanza col tempo perché ne arrivano di nuovi
- Si può usare web scraping / crawling.

### Vincoli obbligatori
- Definire e implementare **CRUD** per **ogni** architettura NoSQL scelta.
- **Analytics e statistiche** sul dataset come funzionalità principali.
- Almeno **3 pipeline di aggregazione** (reali) per il Document DB.
- Se **Graph DB**: almeno un paio di query "on-graph" tipiche (sia di dominio sia graph-centriche).
- Se **Key-Value DB**: almeno **2 entità** e **1 relazione** one-to-many o many-to-many.
- **Indici** per il Document DB: giustificarli e **validarli sperimentalmente**. Sul Graph DB solo se servono.
- **Deploy** sia su **cluster locale** sia su **cluster virtuale** del VIRTUAL LAB UNIPI (accesso da chiedere ai docenti).
  - Almeno **3 replica set** per il Document DB, gestiti in termini di **eventual consistency**.
  - Se Neo4j: repliche non obbligatorie sul cluster virtuale, ma **una istanza** deve girarci insieme al Document DB.
  - Se Key-Value: **3 repliche** sul cluster virtuale insieme a quelle del Document DB.
- Gestire la **consistenza tra i diversi DB**.
- Argomentare ed eventualmente progettare **sharding/partizionamento** per Document e Key-Value DB.
- Dopo il deploy: considerazioni sul **teorema CAP** + statistiche/test di performance su letture e scritture.

---

## 2. Fase di Design (sezione "Design" della documentazione)

1. Descrizione breve dell'applicazione (storytelling per non esperti).
2. Attori principali + requisiti funzionali e non funzionali.
3. **Diagramma UML delle Analysis Classes** (con i campi principali).
4. **Mock-up** di tutte le schermate.
5. Organizzazione dei dati: collezioni (Document DB), namespace e chiavi (Key-Value), entità e relazioni (Graph DB, con snapshot del grafo).
6. Progetto del DB distribuito: **replica set** e **sharding**, considerando la cooperazione tra DBMS diversi.

### Presentazione dell'idea (prerequisito per consegnare)
- **10 min + 10 min di domande**, copre i punti 1-4 + evidenzia quali requisiti/entità vanno su quale DB.
- Solo i progetti con **presentazione approvata** possono consegnare la versione finale.
- Struttura slide del template:
  1. Titolo / autori
  2. Application Highlights (bullet list)
  3. Actors and main mock-ups (i), (ii)… (quante slide servono)
  4. Dataset Description: Source, Description, Volume, Variety, Velocity/Variability
  5. Preliminary UML Class Diagram
  6. Relevant Queries and Entities — Document DB (requisiti funzionali rilevanti **senza** CRUD/find/search/browse; elenco entità, **non** la struttura delle collezioni)
  7. Idem — Key-Value DB (elenco entità, **non** la struttura delle chiavi)
  8. Requirements and Entities — Graph DB (requisiti, entità, snapshot del grafo)

Nell'esempio (Dev2Enterprise: piattaforma sviluppatori ↔ aziende, dati da GitHub/GitLab/Kaggle ≈500MB) ci sono anche: slide Actors con icone, 5 mock-up, UML con classi e molteplicità, Graph DB con nodi/relazioni + 4 query (raccomandazione progetti/skill, anomaly detection, funding recommendation), Document DB con 3 requisiti analitici (ranking, leaderboard, dashboard).

---

## 3. Fase di Implementazione e Test (sezione "Implementation and Test")

La documentazione finale deve contenere:
- Descrizione dei **moduli principali** del software
- Organizzazione e naming di **package e classi** + snapshot del codice più rilevante
- Codice delle **query più rilevanti** (sia in query language nativo, es. aggregazioni MongoDB / query KV / query on-graph, sia nel linguaggio scelto)
- Descrizione degli **scenari di test** e degli **unit test** (se presenti)
- Descrizione dei **test eseguiti**
- **Documentazione completa delle API REST** con esempi d'uso

---

## 4. Consegna, discussione, valutazione

- **Documentazione finale**: caricata su **Google Classroom** **solo dal referente** del gruppo.
- **Codice, dump del DB, eseguibili**: su un **server git** (indicare l'indirizzo nella documentazione).
- **Nessuna revisione** prima della consegna.
- Scadenza: fissata dal docente, di solito **1 settimana prima** della data d'esame ufficiale.
- La discussione avviene **prima dello scritto**: presentazione estesa + **demo** delle API.
- Linee guida per la discussione: **entro fine novembre 2026**.
- Dopo la valutazione, ogni membro può fare l'orale in qualsiasi appello dell'anno accademico.

### Voto del progetto
| Peso | Voce |
|---|---|
| 25% | Idea, definizione dei requisiti, diagrammi UML |
| 40% | Architettura del DB e definizione delle query |
| 25% | Implementazione |
| 10% | Chiarezza della documentazione complessiva |

La valutazione individuale dipende dal voto complessivo e dalle risposte date in discussione.

---

## 5. Riferimenti: progetti passati

Fonte: https://github.com/Ruggero1912/AIDE-unipi (cartella LSMSDB). Elenco di progetti 2021-2024, quasi tutti MongoDB + Neo4j in Java. Riferimento principale scelto: **BeatBuddy** (2023/24, voto massimo). Altro riferimento: **Rechype** (2021), l'unico con anche un key-value DB (HaloDB, cache di immagini, chiavi `type:id:Image`, nessuna gestione di consistenza: non è un buon modello).

Dall'orale di EventinZona (2023): la consistenza Mongo↔Neo4j va gestita con **hinted handoff + retry con timeout**, non con rollback; i **cronjob** vanno fuori dal server applicativo (script esterni), altrimenti ogni nodo ripete le stesse scritture.

### Cosa ha fatto BeatBuddy (letto da `documentation_BeatBuddy.pdf`, 62 pagine)

**Struttura della documentazione**: 1 Application manual (highlights, user/admin manual con screenshot) → 2 Design (attori, requisiti funzionali e non, use case diagram, UML) → 3 Data modeling (dataset, script, volumi, collezioni, grafo) → 4 Implementation (package, operazioni Mongo rilevanti, query Neo4j, admin) → 5 Database choices (VM, indici, sharding, replica, CAP) → 6 Future implementation.

**Dataset (variety)**: 10.000 artisti Spotify (Kaggle) + 1,5 milioni di recensioni Amazon + ~20.000 recensioni da riviste musicali. API: Spotify (album, tracklist), MusicBrainz + FanArt.tv (foto artista). Recensioni distribuite sugli album in base alla distribuzione ASIN (esponenziale); utenti generati dai nomi nelle recensioni + Faker; password SHA256. Script Python (spotipy, pymongo, neo4j).

**Stima dei volumi**: tabella con % di utilizzo → richieste/giorno per ogni azione (ricerca, caricamento pagina, like, recensione) → operazioni Mongo/Neo4j. Poi **usata per giustificare** indici e scelte di design.

**MongoDB** (4 collezioni: users, albums, artists, reviews):
- Reviews in collezione separata (crescita rapida), ma le **ultime 5** sono embeddate nell'album (`last_reviews`) e un riassunto è embeddato nell'utente (`reviewedAlbums`). La scrittura di una recensione aggiorna 3 collezioni → consistenza gestita a mano.
- Contatori (`likes`, `averageRating`) denormalizzati, aggiornati **una volta al giorno** da un job admin.
- 3 pipeline di aggregazione giustificate nel dettaglio (es. `$group → $match(≥10 recensioni) → $lookup → $project → $sort → $limit`; artisti con ≥3 album per media rating), con considerazioni sul costo del `$lookup` (fatto il più tardi possibile, su campo indicizzato).
- Classifiche settimanali calcolate dall'admin e salvate in **file JSON**, così la pagina "Most Popular" non interroga il DB.

**Indici (validati sperimentalmente con MongoDB Compass / execution time)**: text index su `users.username`; text index su `albums.songs.name` (un solo text index per collezione); indice composto `(title, artists)` (update dei like: da >1h a pochi secondi); **nessun** indice su `reviews` (30k scritture/giorno vs poche letture) né su `artists` (insieme fisso), motivato. Neo4j: indici su User.username, Song.albumName, Album.albumName, scelti confrontando alternative (`db.clearQueryCaches()` tra i test).

**Neo4j**: nodi User, Song, Album; relazioni `FOLLOW`, `LIKES_A`, `LIKES_S` (con timestamp). Like e follow **solo su Neo4j** (niente duplicazione), contatori copiati su Mongo una volta al giorno → **eventual consistency** dichiarata come scelta di prodotto. Nessuna relazione Song→Album (evita pattern relazionale, accetta ridondanza). Query on-graph: suggerimento canzoni per gusti (utenti simili), canzoni popolari tra i seguiti, utenti da seguire (amici di amici), tutte con score = numero di archi.

**Implementazione**: Spring Boot 3 + MVC + Thymeleaf; package `config`, `controllers` (pagine), `controllers.api` (`@RestController`), `controllers.model`, `model.dummy` (mapping aggregazioni), `repository` (interfacce Spring Data + classi Repo per aggregazioni/query complesse), `utility`. Job settimanale che lancia uno script Python via `ProcessBuilder` per aggiornare gli album.

**Distribuzione**: 3 VM. Replica set Mongo `BB` con 3 nodi e priorità 5/3/1 (il primario sul nodo con meno altri processi); URI `w=1&readPreference=nearest&retryWrites=true`. Sharding su 3 server con chiavi diverse per collezione (albums: prima lettera del titolo, range A-F/G-O/P-Z bilanciati con conteggi reali; users: username; reviews: **hash** di albumID; artists: name). Neo4j Community: niente replica, istanza su una VM diversa da web server e primario Mongo. **CAP**: scelta esplicita AP, motivata con i requisiti (alta disponibilità, dati a volte non aggiornati).

### Differenze da tenere presenti per il 2026-27
- Il testo di quest'anno richiede **deploy sul cluster locale e sul virtuale**, **3 replica set** con eventual consistency, **test di performance read/write** e considerazioni CAP dopo il deploy: BeatBuddy ha solo la parte descrittiva sul CAP, noi dobbiamo misurare.
- **Almeno 3 query on-graph** ("un paio" nel testo, BeatBuddy ne ha 3 + CRUD) e almeno 3 pipeline: lo standard è lo stesso.
- Un Key-Value DB è opzionale (serve solo se lo scegliamo; allora 2 entità + 1 relazione e 3 repliche sul cluster virtuale). Doc + Graph basta per i "due NoSQL".
- BeatBuddy usa una web app con pagine server-side; il testo del prof chiede invece **un servizio con API RESTful** (Swagger/Postman), quindi la documentazione API va curata di più.

---

## 6. Pipeline operativa

Base teorica: gli appunti in `cambria_notes/` (Big Data, ACID vs BASE/CAP, Key-Value, Document DB, Graph DB, Design a Social Network, recap SE e REST, lab Redis). **MongoDB e Neo4j come strumenti non sono ancora stati visti**: la settimana prossima arrivano. Le fasi sono ordinate in modo che le prime non dipendano da loro.

Scadenze note: gruppi (da 3) entro **fine novembre**; presentazione dell'idea da far **approvare** prima di poter consegnare; linee guida per la discussione entro fine novembre.

### Fase 0 — Adesso: gruppo, idea, dataset (nessun tool necessario)
1. **Gruppo da 3**: chiuderlo presto (secondo round di formazione a novembre).
2. **Brainstorm di 3-4 idee** di dominio. Criteri di scelta, derivati dal testo del prof:
   - esiste un **dataset reale ≥ 100 MB** (meglio 200+) scaricabile/scrapabile;
   - ha **variety** (≥2 sorgenti/formati unibili) e/o **velocity** (dati che invecchiano);
   - ha dati con **relazioni forti** (utenti-utenti, utenti-item) → giustifica un Graph DB con query graph-centriche vere (percorsi, raccomandazione, centralità), non solo "follow";
   - ha **analytics naturali** (ranking, trend, statistiche) → 3 pipeline di aggregazione credibili;
   - ha un caso d'uso per cache/sessioni/contatori temporanei, **solo se** vogliamo il Key-Value (opzionale; Doc + Graph bastano).
3. **Verifica del dataset prima di innamorarsi dell'idea**: scaricarlo, contare record/MB, guardare campi, qualità, licenza/ToS dello scraping. Salvare uno script riproducibile in `data/` (non committare il dato grezzo se pesante).
4. **Mandare/presentare l'idea ai docenti** in forma di bozza (Application Highlights + dataset) durante il corso o i ricevimenti (lunedì 17-19, su prenotazione): feedback precoce, serve l'approvazione.

### Fase 1 — Adesso: requisiti e UML (si fa con quanto già visto)
1. **Attori** e **use case** (diagramma + elenco). Distinguere CRUD (obbligatorio ma non va nelle slide) dai requisiti "interessanti".
2. **Requisiti funzionali e non funzionali** — i non funzionali guidano il CAP: decidere subito se l'app è AP (disponibilità, dati a volte vecchi) o CP, e dichiararlo.
3. **Mock-up** di tutte le schermate (Figma/Excalidraw/carta): ogni schermata = una o più query, quindi i mock-up sono già una lista di access pattern.
4. **UML Analysis Classes**: entità, attributi principali, molteplicità. Regola dagli appunti: non considerare tutte le combinazioni di relazioni, solo quelle che supportano query dei requisiti.
5. **Slide dell'idea** col template del prof (10 min + 10 min domande). Punti 6-8 del template: requisiti per DB e **elenco** entità, non struttura di collezioni/chiavi.

→ Output: bozza di slide + `docs`/sezione "Design" della documentazione. È già l'**25%** del voto.

### Fase 2 — Adesso/inizio prossima settimana: access pattern e assegnazione delle entità ai DB
Questo è il cuore del 40% "architettura + query" e si può fare **sulla carta**:
1. **Tabella degli access pattern**: per ogni requisito → query, frequenza stimata (stile BeatBuddy: % utilizzo → richieste/giorno → operazioni DB), read-heavy o write-heavy.
2. **Assegnare ogni entità/dato a un DB** con motivazione (appunti Document DB 2.9 e Graph DB 2.3):
   - **Document DB**: entità principali e dati con crescita/lettura per documento; decidere **embed vs reference** (embedding per dati letti insieme e limitati; collezione separata per dati a crescita rapida, eventualmente con ultimi N embeddati); una vs più collezioni decisa dal **workload**, non dalla struttura logica.
   - **Graph DB**: solo ciò che vive di relazioni; mappare query di dominio → query graph-centriche (tabella come in appunti 2.3); archi con direzione corretta, pochi property sugli archi, attenzione ai cicli.
   - **Key-Value (se scelto)**: chiavi `tipo:id:campo`, namespace, 2 entità + 1 relazione 1-N o N-N.
3. **Politica di consistenza tra DB**: per ogni dato duplicato su più DB, decidere chi è la fonte di verità e come si propaga (eventual consistency dichiarata, retry con timeout/hinted handoff, job esterni, non rollback). Documentarla ora.
4. **Bozza di sharding** (shard key presente in tutti i documenti, algoritmo range/hash/list; hash per evitare hot spot sui dati a crescita rapida) e di replica (N, W, R; 3 replica set).

→ Questo prepara le slide 6-8 e la sezione "Progetto del DB distribuito".

### Fase 3 — Adesso, in parallelo: ambiente e dati (poco dipendente dai tool NoSQL)
1. **Chiedere ai docenti l'accesso al Virtual Lab UNIPI** (ci vuole tempo; serve per il deploy).
2. **Repo git** del progetto (server git indicato nella documentazione): struttura `api/` (servizio), `data/` (script di raccolta/import), `docs/`, `deploy/`.
3. **Stack**: scegliere linguaggio/framework per il servizio REST (Java + Spring Boot + Swagger è il consigliato e coperto nel recap SE/REST), Docker Compose per sviluppo locale.
4. **Script di acquisizione/pulizia** del dataset (Python o Java), idempotente, con log dei volumi finali (servono nella documentazione).
5. Se Key-Value: Redis è l'unico già visto in lab → si può già provare Jedis in locale.

### Fase 4 — Prossima settimana (MongoDB, poi Neo4j): si passa all'implementazione
1. Installare Mongo e Neo4j in locale (Docker); import del dataset.
2. Scrivere le **3+ pipeline di aggregazione** reali e le **query graph** (≥ 3).
3. **Indici**: ipotizzarli dagli access pattern, poi **validarli con `explain()`/tempi** prima-dopo (richiesto sperimentalmente; documentare anche gli indici *non* messi, come BeatBuddy).
4. CRUD + API REST per ogni DB scelto; Swagger/Postman.
5. Rivedere le scelte di Fase 2 alla luce di ciò che Mongo/Neo4j permettono davvero (es. limiti di `$lookup`, un solo text index per collezione).

### Fase 5 — Dopo l'approvazione: cluster, test, documentazione
1. Replica set con ≥ 3 nodi in locale, poi sul Virtual Lab; Neo4j (e KV se presente) secondo i vincoli di sezione 1.
2. Sharding dove argomentato.
3. **Test di performance read/write** e scenari di consistenza/partizione → considerazioni CAP *misurate* (differenza rispetto a BeatBuddy).
4. Documentazione finale (struttura BeatBuddy, sezione 5) + dump DB + repo; consegna dal referente su Classroom ~1 settimana prima dell'esame.

### Cosa fare **questa settimana** (riassunto)
- [ ] Decidere il gruppo
- [ ] 3-4 idee candidate + verifica dei dataset (volume, variety/velocity)
- [ ] Bozza di requisiti, attori, use case per l'idea favorita
- [ ] Richiedere il Virtual Lab
- [ ] Preparare repo e Docker Compose vuoto per la prossima settimana

---

## 6b. Archivio idee candidate (1-12), non scelte

Idee valutate e scartate il 2026-11-06 a favore dell'idea 13 (sezione 6g). Restano qui solo pochi punti per idea, per memoria. Le versioni ampliate e le verifiche dei dataset sono state rimosse. Dove non indicato diversamente, dimensioni e dettagli dei dataset sono dalla memoria e **non verificati**.

**1. Film/serie: recensioni e raccomandazioni**
- MovieLens (~25M rating) + metadati TMDB/IMDb, uniti dagli ID.
- Grafo utente-film-attori-registi (utenti simili, raccomandazioni, percorsi attore-attore).
- Dominio molto battuto, poco originale.

**2. Rete di paper scientifici (citazioni e coautori)**
- OpenAlex: licenza CC0 (verificata), snapshot enorme (~745 GB), serve un sottoinsieme via API.
- Grafo di citazioni e coautori molto naturale (centralità, comunità, percorsi); era la raccomandazione provvisoria.
- Rischio: costo e tempo dell'estrazione API; manca un utente "sociale".

**3. Sviluppatori e repository**
- GH Archive + Stack Overflow data dump: variety e velocity entrambe.
- Troppo simile all'esempio Dev2Enterprise del prof.

**4. Mobilità (trasporto pubblico, bike sharing)**
- GTFS + GTFS-realtime oppure corse storiche di bike sharing; velocity forte.
- Grafo di fermate o stazioni (percorsi, nodi critici).
- Poco utente e raccomandazione: più un'app di analytics.

**5. Viaggi e alloggi (Inside Airbnb + Yelp)**
- Inside Airbnb: licenza CC BY 4.0 (verificata), 10 voci per l'Italia. Yelp: 4,35 GB compressi, licenza non verificata.
- Variety debole: i dati si uniscono solo se si sceglie la stessa città; Airbnb non ha una rete sociale.
- Key-Value naturale (sessioni, cache).

**6. Libri e lettori (Goodreads)**
- Dataset UCSD di accesso incerto; struttura simile all'idea 1 e poco aggiunge.

**7. Supply chain software (vulnerabilità e dipendenze)**
- Fonti verificate: NVD (419.513 CVE), OSV (npm ~220 MB zip, PyPI 37,6 MB; nel campione PyPI il 52% ha un alias CVE e il 45% sono segnalazioni di malware), EPSS, Exploit-DB (~47.000 exploit con CVE nei `codes`).
- Grafo di dipendenze da deps.dev: l'API dà il grafo di **una versione per volta** (express: 71 nodi), nessun download bulk trovato.
- Molto originale e con query graph vere (blast radius, cammino di dipendenza); rischio principale l'estrazione del grafo e le licenze miste.

**8. Hacker News: discussioni e trend**
- Dump con milioni di item; ranking con decadimento temporale; Key-Value ben motivato (front page, voti).
- Dominio nerd, vicino al "social generico".

**9. Wikipedia: link, visite e navigazione**
- Clickstream (CC0), Pageviews API, grafo dei link; gioco "WikiRace" con cammini minimi.
- Pochi dati sociali veri.

**10. Bitcoin: flussi di fondi e anomalie**
- Dataset Elliptic (~200k transazioni etichettate) o estratti della blockchain; tracciamento dei fondi sul grafo.
- Dominio tecnico; da verificare cosa conta come "dati reali".

**11. Trasporto aereo: rotte e ritardi**
- BTS On-Time Performance + OpenFlights + meteo; rete di aeroporti, hub critici.
- Troppo vicino all'idea 4.

**12. Parlamento europeo: voti degli eurodeputati italiani** (seconda scelta)
- HowTheyVote (ODbL, share-alike): 18,2 milioni di voti nominali; i soli italiani sono 1.947.558 righe, 142 eurodeputati, circa 90 MB. L'API del Parlamento europeo si unisce con lo stesso ID.
- La Camera dei deputati non ha i voti per singolo deputato (solo totali), quindi non utilizzabile.
- Punti deboli: poche scritture degli utenti e grafo piccolo (142 nodi) da giustificare per un Graph DB.

---

## 6g. Idea 13 — Social network per tifosi di Formula 1 (recap)

*Nata dalla discussione col compagno di gruppo il 2026-11-05. Nome di lavoro: **Paddock**(provvisorio).

### In breve
Un social per tifosi in cui si seguono piloti, scuderie e altri utenti, si fanno **pronostici prima di ogni gara** (podio, pole, giro veloce) dentro leghe con gli amici, si votano e commentano le gare, e si esplorano statistiche storiche e di telemetria. Il carico è a **picchi nei weekend di gara** (24 all'anno). Verificata oggi (2026-11-05) la disponibilità dei dati; gli utenti sono sintetici.

### Perché ci piace
- **Scritture utente frequenti e a picchi:** pronostici, voti, commenti, follow. Dà carico reale per i test read/write, per la consistenza fra DB e per il CAP *misurato*.
- **Key-Value ben motivato:** classifica in diretta, stato della gara in cache, sessioni, rate limiting.
- **Velocity naturale:** dati che arrivano a raffica e perdono importanza in fretta.
- **Dati tecnicamente ricchi:** risultati storici dal 1950, giri, pit stop, posizioni, meteo e telemetria dal 2023.
- **Dominio non usato dai progetti passati.**

### Attori
- **Tifoso registrato:** profilo, segue piloti, scuderie e utenti, fa pronostici, partecipa a leghe, vota e commenta, vede feed e statistiche.
- **Visitatore non registrato:** consulta risultati e statistiche.
- **Amministratore:** importa i dati dopo ogni sessione, chiude le finestre dei pronostici, calcola i punteggi, modera i commenti, ricalcola gli indicatori.
- *(Opzionale)* **Moderatore** di lega.

### Requisiti funzionali (oltre al CRUD, che non va nelle slide)
1. **Pronostici:** prima della qualifica o della gara l'utente indica podio, pole e giro veloce; a gara finita il sistema assegna i punteggi.
2. **Leghe fra amici:** classifica interna per gara e stagionale.
3. **Classifica globale dei pronosticatori** (stagione, ultime gare) e per lega.
4. **Follow** di piloti, scuderie e utenti; **feed** con risultati, pronostici e commenti dei seguiti.
5. **Voti e commenti** sulla gara (valutazione della gara, "pilota del giorno").
6. **Analytics storiche:** risultati, statistiche per pilota e scuderia, confronto fra piloti.
7. **Analytics di gara:** strategie ai box, degrado delle gomme, sorpassi, meteo.
8. **Raccomandazioni:** chi seguire (utenti e piloti) in base a follow e pronostici simili.
9. **Esplora la rete dei piloti:** "chi ha corso con chi", collegamento fra due piloti attraverso i compagni di squadra.
10. **Replay di una gara** (simulazione dei dati in arrivo) per la demo e per i test di carico.

### Requisiti non funzionali
- **Disponibilità alta** durante i weekend di gara: scelta **AP**, con dati a volte non aggiornati (classifica che si aggiorna con qualche secondo di ritardo).
- **Eccezione da trattare con cura:** l'accettazione di un pronostico entro la scadenza deve avvenire **una sola volta e con regola chiara** (finestra chiusa all'inizio della qualifica o della gara). Qui si può usare una scrittura con write concern più forte sulla sola collezione dei pronostici. Il resto resta a consistenza eventuale. È un buon esempio da mostrare nella parte sul CAP.
- Scalabilità sui **picchi di scrittura** del weekend; letture veloci sui dati storici.
- Rispetto di licenze e limiti di richieste delle fonti.

### Dati

**Jolpica** (`api.jolpi.ca`, successore compatibile di Ergast; il dump statico di Ergast non c'è più) — verificato
- **Copertura:** 77 stagioni (dal 1950), 1.172 gare, 884 piloti, 214 scuderie, 26.291 risultati di gara, 11.386 risultati di qualifica.
- **Giri e pit stop:** nel 2024, 24 gare, **26.574 righe di giri** e **825 pit stop**. I giri esistono dal 1996 (nel primo GP del 1995 e del 1990 non ce ne sono).
- **Dump di database:** CSV e SQL scaricabili dall'API (`/data/dumps/download/`). Il dump gratuito è quello *ritardato* di 14 giorni; il più recente richiede una chiave di sostenitore. Dimensione del dump ritardato: **14,6 MB** (CSV), quindi i soli dati Jolpica sono piccoli.
- **Limiti senza autenticazione:** 4 richieste al secondo e **500 richieste all'ora**. Se si scarica via API i giri di 30 stagioni servono molte pagine: meglio il dump.
- **Licenza:** codice Apache 2.0; i dati sono "liberi per uso non commerciale" secondo la documentazione dei dump. Una licenza formale per i dati non l'ho trovata.

**OpenF1** (`api.openf1.org`) — verificato
- **Cosa offre:** 18 endpoint (sessioni, giri, posizioni, meteo, pit, stint, sorpassi, messaggi di direzione gara, telemetria dell'auto a circa 3,7 Hz, posizione in pista a circa 3,7 Hz e altro). Storico **dal 2023**.
- **Accesso:** storico **gratuito e senza autenticazione**; i dati in diretta (sessione in corso o conclusa da meno di 30 minuti) richiedono un abbonamento. Quindi con il piano gratuito l'ingestione avviene **almeno 30 minuti dopo la fine della sessione**.
- **Limiti piano gratuito:** 3 richieste al secondo e **30 richieste al minuto**.
- **Licenza:** CC BY-NC-SA 4.0 (dalla pagina). L'uso dichiarato è "educativo, apprendimento personale, ricerca e fan engagement non commerciale": la nostra situazione. Lo **share-alike** si applica a un dump derivato. Progetto non ufficiale, non associato a Formula 1.
- **Misure (gara del Bahrain 2024, un solo weekend), come risposte JSON:** giri 564 KB, posizioni 80 KB, meteo 34 KB, telemetria dell'auto di **un solo pilota 5,7 MB**. La richiesta della telemetria di tutta la sessione fallisce ("too much data"): va fatta **per pilota** (o per intervalli).
- **Stima (non misurata):** ~115 MB di telemetria per gara (20 piloti) e oltre 2 GB per la stagione 2024. Per scaricare la telemetria di una gara servono circa 20 richieste (una per pilota), quindi circa un minuto di limite di richieste; 24 gare richiedono circa 20 minuti.

**Volume (opzioni, da decidere):**
- *Solo Jolpica* (risultati, giri, pit): ~15 MB di dump, **sotto la soglia**; non basta da solo.
- *Più OpenF1 senza telemetria* (giri, posizioni, meteo, stint, pit, sorpassi per 2023–2026): pochi MB a gara; probabilmente decine di MB in totale. Va misurato.
- *Più telemetria* di alcune gare (o di una stagione): centinaia di MB o più, con 3,7 Hz per pilota. **È il modo per stare sopra 100 MB** e dà la tabella che giustifica sharding e indici.
- *Più utenti sintetici* (utenti, follow, pronostici, voti, commenti): volume arbitrario ma dichiarato come generato.

**Variety e velocity**
- **Variety:** due API con schemi, identificatori e copertura diversi (1950– contro 2023–; `driverId` Jolpica contro `driver_number` per stagione di OpenF1), unite per anno, round e pilota con riconciliazione dei nomi. È una variety **moderata**: due fonti dello stesso dominio. Si può rafforzare con Wikidata/Wikipedia per biografie e circuiti (**non verificato**).
- **Velocity:** forte e ciclica. I dati dei weekend arrivano dopo ogni sessione (con ritardo di almeno 30 minuti nel piano gratuito) e perdono importanza in fretta; la classifica provvisoria dura minuti.

### Dati sintetici (utenti e interazioni)
Utenti, follow, leghe, pronostici, voti e commenti sono **generati** (come gli utenti di BeatBuddy, creati con Faker), con distribuzioni realistiche (pochi utenti molto attivi, molti poco attivi). Va dichiarato nella documentazione. I pronostici per le gare passate si generano a partire dai risultati reali con un rumore controllato, così da avere punteggi sensati. Dimensioni da fissare (es. ordine di decine di migliaia di utenti e centinaia di migliaia di pronostici).

### Assegnazione dei dati ai DB

**Document DB (MongoDB)**
- **Collezioni:** `drivers`, `constructors`, `circuits`, `races` (con riepilogo risultati embeddato), `laps` (separata, volume alto), `telemetry` (campioni a 3,7 Hz **raggruppati in bucket** per pilota e giro: *bucket pattern*), `pit_stops`, `users`, `predictions` (volume e scritture alte, separata), `comments`/`ratings`, `leagues`.
- **Embed contro riferimento:** i risultati di una gara sono letti insieme alla gara (embeddati); giri e telemetria crescono e si leggono per sottoinsiemi (collezioni separate); le ultime N voci di un feed potrebbero essere embeddate nell'utente (stile `last_reviews` di BeatBuddy).
- **Pipeline di aggregazione (almeno 3):**
  1. **Strategia ai box:** per scuderia e circuito, numero e durata media dei pit stop e posizioni guadagnate o perse.
  2. **Degrado delle gomme:** variazione media del tempo sul giro in funzione dell'età dello stint per mescola e circuito.
  3. **Classifica dei pronosticatori:** punteggio totale per lega e stagione, con `$lookup` sul profilo fatto il più tardi possibile (dopo `$group`, `$sort` e `$limit`).
  4. *(Opzionali)* Affidabilità (percentuale di ritiri) per scuderia e stagione; sorpassi per circuito.
- **Indici (da validare con `explain()`):** ipotesi su `predictions` (utente, gara), `laps` (gara, pilota, giro), `telemetry` (gara, pilota, giro), `users.username`. Documentare anche gli indici *non* messi.

**Graph DB (Neo4j)**
- **Nodi:** `Driver` (884), `Constructor` (214), `User`, `League`; eventualmente `Race`.
- **Archi:** `DROVE_FOR` (pilota → scuderia, con stagioni), `TEAMMATE_OF` (derivato, con stagioni), `FOLLOWS` (utente → utente, utente → pilota, utente → scuderia), `MEMBER_OF` (utente → lega).
- **Query graph-centriche:**
  1. **Cammino minimo fra due piloti** attraverso i compagni di squadra ("gradi di separazione").
  2. **Chi seguire:** amici di amici che seguono i miei piloti, con punteggio uguale al numero di archi in comune.
  3. **Piloti simili:** piloti con il maggior numero di scuderie o compagni in comune.
  4. **Comunità di tifosi** (utenti raggruppati dai piloti che seguono).
  5. **Rivalità storiche:** coppie di piloti che sono stati compagni nella stessa scuderia in più stagioni.
- **Indici:** su `User.username`, `Driver.id`, `Constructor.id`; confrontare alternative come in BeatBuddy, con `db.clearQueryCaches()` fra le prove.
- Follow e appartenenza alle leghe **solo su Neo4j** (come like e follow di BeatBuddy), con contatori copiati su Mongo con un job.

**Key-Value (da decidere)**
- Se lo scegliamo: servono **2 entità e una relazione 1-N**, e **3 repliche sul cluster virtuale** insieme a quelle del Document DB (vincolo del testo del prof).
- **Entità candidate:** `Session` (sessione di login: `session:{token}` → utente, con TTL) e `LeaderboardEntry` (classifica di lega: `lb:{lega}:{gara}` con punteggi), più `ratelimit:{chiave}`. **Relazione 1-N:** Lega → voci della classifica (o Utente → Sessioni).
- **Perché ha senso:** la classifica in diretta e il rate limiting sono accessi ad alta frequenza e semplici, il caso d'uso classico.

### Consistenza fra DB
- **Fonti di verità:** i pronostici e i risultati vivono su Mongo; follow e leghe su Neo4j.
- **Dati derivati:** classifiche su Key-Value (ricalcolate da Mongo dopo l'ingestione dei risultati), contatori di follower su Mongo (job periodico), archi `TEAMMATE_OF` su Neo4j (derivati da `DROVE_FOR`).
- **Propagazione:** job esterni (non nel server applicativo), con retry e timeout (hinted handoff), non rollback. Eventual consistency dichiarata come scelta di prodotto (AP).
- **Eccezione:** la scadenza dei pronostici (vedi requisiti non funzionali).

### Sharding e replica (bozza)
- **Replica set Mongo con 3 nodi**, priorità diverse; letture `nearest` per le analytics, `majority` per i pronostici. Neo4j: una istanza sul cluster virtuale, su una VM diversa dal server applicativo e dal primario Mongo.
- **Sharding:** `telemetry` e `laps`: shard key con hash su (gara, pilota) per evitare che tutte le scritture del weekend vadano sullo stesso shard (il rischio è proprio un **hot spot sulla gara corrente**). `predictions`: hash sull'identificativo dell'utente. Collezioni piccole (piloti, scuderie, circuiti): nessuno sharding, motivato.
- **Test di performance da fare:** scrittura massiva a picchi con il replay di una gara; letture della classifica; tempi con e senza indici; confronto fra `w=1` e `w=majority` sui pronostici.

### Mock-up da preparare
Home con feed, pagina gara (risultati, strategie, commenti, voto), pagina pilota/scuderia (statistiche, follow), pronostico (form con scadenza), classifica e leghe, profilo utente, esplora la rete dei piloti (grafo), area amministratore (import, calcolo punteggi). Ogni schermata corrisponde a una o più query: servono come lista di access pattern.

### Classi UML (bozza)
`User`, `Driver`, `Constructor`, `Circuit`, `Race` (con `Session`), `Lap`, `PitStop`, `TelemetrySample`, `Prediction`, `League`, `Comment`, `Rating`, `Follow`. Relazioni solo per le query dei requisiti, come da regola degli appunti.

### Rischi e punti da decidere
1. **Licenza OpenF1 (CC BY-NC-SA)**: l'uso didattico è compatibile, ma lo share-alike si applica al dump consegnato; verificare con il prof (domanda 3).
2. **Volume:** senza telemetria l'idea è sotto soglia; con la telemetria diventa grande e va scelto quanta importare (es. alcune gare o una stagione).
3. **Limiti di richieste:** OpenF1 30 al minuto, Jolpica 500 all'ora: l'estrazione richiede script con attesa e ripresa; per Jolpica conviene il dump ritardato.
4. **Dati dal vivo a pagamento:** con il piano gratuito il dato arriva almeno 30 minuti dopo la sessione; la demo "live" si fa con il **replay** di una gara. Da dichiarare.
5. **Variety moderata:** due fonti dello stesso dominio; si può rafforzare con Wikidata (da verificare).
6. **Utenti sintetici:** va spiegato chiaramente come sono generati e perché.
7. **Qualità dell'unione fra fonti:** `driverId` e `driver_number` non coincidono; serve una tabella di corrispondenza per stagione.
8. **Marchi:** nome neutro, nessun logo ufficiale.
9. **Key-Value:** se lo scegliamo, costo in più per le tre repliche sul cluster virtuale.
10. **Grafo piloti piccolo** (884 + 214 nodi): il grafo sociale (follow e leghe) cresce con i dati sintetici ed è quello che lo rende credibile.

### Domande per il prof (idea 13)
1. **Dati sintetici:** per una parte sociale con utenti, follow, pronostici e commenti generati, serve altro oltre alla dichiarazione nella documentazione?
2. **Variety:** due API dello stesso dominio (Jolpica e OpenF1) bastano come variety, o la velocity ciclica dei weekend di gara è sufficiente?
3. **Licenza CC BY-NC-SA:** il dump consegnato deve avere la stessa licenza: va bene?
4. **Volume:** è accettabile una telemetria importata solo per un sottoinsieme di gare per superare la soglia?
5. **Dati in diretta a pagamento:** va bene simulare il tempo reale con il replay di una gara?
6. **Key-Value:** vale la pena per classifica e sessioni, considerando le tre repliche sul cluster virtuale?

### Prossimi passi per chiuderla
- [ ] Scaricare il dump CSV ritardato di Jolpica: schema delle tabelle, righe, MB decompressi.
- [ ] Misurare le dimensioni reali di OpenF1 per una gara (tutti gli endpoint, tutti i piloti) e per una stagione; verificare i limiti in pratica.
- [ ] Decidere quanta telemetria importare.
- [ ] Definire la tabella di corrispondenza fra `driverId` (Jolpica) e `driver_number` (OpenF1).
- [ ] Verificare Wikidata come fonte aggiuntiva (biografie, circuiti).
- [ ] Definire il modello dei dati sintetici (dimensioni, distribuzioni) e i punteggi dei pronostici.
- [ ] Decidere se includere il Key-Value.

---

## 7. Stato del lavoro

Checklist (aggiornare man mano):

- [x] Scelta dell'idea / dominio applicativo (social per tifosi di F1, 2026-11-06)
- [ ] Scelta del dataset (sorgenti, volume ≥ 50/100 MB, variety e/o velocity)
- [ ] Scelta delle architetture NoSQL (Document DB + almeno un altro)
- [ ] Attori e requisiti funzionali / non funzionali
- [ ] Mock-up
- [ ] UML Analysis Classes
- [ ] Slide di presentazione dell'idea
- [ ] Presentazione dell'idea **approvata** dai docenti
- [ ] Design collezioni / chiavi / grafo
- [ ] Design replica set e sharding
- [ ] Richiesta accesso al Virtual Lab UNIPI
- [ ] Raccolta/scraping del dataset e import
- [ ] Implementazione CRUD + API REST
- [ ] 3+ pipeline di aggregazione, indici validati
- [ ] Deploy cluster locale + virtuale, test CAP/performance
- [ ] Documentazione finale + repo git + dump
- [ ] Consegna su Google Classroom

### Decisioni prese
- **2026-11-06 — Idea scelta dal gruppo: social network per tifosi di Formula 1** (idea 13, recap in sezione 6g; nome di lavoro "Paddock", senza marchi ufficiali). Le idee 1-12 restano come archivio sintetico (sezione 6b), non più in gara.
  - Fonti previste: Jolpica (storico dal 1950, dump CSV) e OpenF1 (giri, posizioni, meteo e telemetria dal 2023). Utenti e interazioni sintetici.
  - **Non ancora deciso:** quanta telemetria importare (serve per superare la soglia di volume), se includere il Key-Value (tre repliche sul cluster virtuale), perimetro dei dati sintetici, tabella di corrispondenza fra `driverId` e `driver_number`.
  - **Non ancora fatto:** presentazione dell'idea al prof e relativa approvazione. Le domande da portare sono in fondo alla sezione 6g.

### Registro attività
| Data | Cosa è stato fatto |
|---|---|
| 2026-11-03 | Caricato il materiale del prof in `project/`; letto e riassunto in questo file. |
| 2026-11-05 | Aggiunte 6 nuove idee candidate (7-12, sezione 6d) con confronto; idea 12 (parlamento: voti e coalizioni) sviluppata in dettaglio. Dataset non ancora verificati. |
| 2026-11-06 | Condensate le idee 1-12 in un archivio breve (sezione 6b), rimossi ampliamenti, verifiche e domande per il prof sull'idea 12. I riferimenti alle sezioni 6c-6f nelle righe precedenti si riferiscono a contenuti ormai rimossi. |
| 2026-11-06 | Il gruppo ha scelto definitivamente l'idea 13 (social per tifosi di F1). Registrata in "Decisioni prese" e nella checklist; restano aperti volume della telemetria, Key-Value e approvazione del prof. |
| 2026-11-05 | Aggiunta l'idea 13 (social per tifosi di F1) con recap completo nella sezione 6g; dati verificati su Jolpica e OpenF1 (volumi, limiti, licenze) e misure dirette. |
| 2026-11-05 | Aggiunta la sezione 6f con 8 domande per il prof sull'idea 12, con spazio per annotare le risposte. |
| 2026-11-05 | Riscritta l'idea 12 (sezione 6d) sul Parlamento europeo con eurodeputati italiani (142 nodi, 1,95M righe di voto misurate); variety a tre livelli con API del Parlamento (join diretto sull'ID) e incontri con i lobbisti; perimetro di MongoDB da decidere. Il limite di 100 MB è indicativo. |
| 2026-11-05 | Verifica dataset idee 7 e 12 (sezione 6e), con misure dirette: la Camera non ha voti nominali per deputato (solo totali), HowTheyVote sì (18,2M righe, ODbL); idea 7 fattibile ma il grafo di dipendenze (deps.dev) è per singola versione. |
| 2026-11-05 | Sviluppata in dettaglio anche l'idea 7 (supply chain: vulnerabilità e dipendenze), nella sezione 6d. |
| 2026-11-03 | Verifica dataset idee 2 e 5 su pagine ufficiali (sezione 6c): OpenAlex confermato, Yelp con licenza e `friends` da verificare, variety dell'idea 5 debole. |
| 2026-11-03 | Brainstorming di 6 idee candidate con dataset, confronto e domande aperte (sezione 6b), da discutere col gruppo. |
| 2026-11-03 | Letti gli appunti dello studente in `cambria_notes/` (conoscenza finora); definita la pipeline operativa (sezione 6). |
| 2026-11-03 | Letto il README di AIDE-unipi e la documentazione completa di BeatBuddy (riferimento principale); riassunto nella sezione 5. Rechype letto solo in parte (codice, non il PDF). |
