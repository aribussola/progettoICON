# INDICE
### <a href=#intro>1. INTRODUZIONE</a>
### <a href=#dati>2. DATI</a>
### <a href=#regioni>3. REGIONI</a>
### <a href=#regioni>4. PREVISIONE</a>
### <a href=#regioni>5. CLASSIFICAZIONE</a>
### <a href=#regioni>6. RICERCA PERCORSO</a>

<h2 id="intro"> 1. Introduzione </h2>
Il programma affronta diverse problematiche legate alla pandemia SARS-CoV-2.

Il particolare, la gestione degli spostamenti tra regioni in base alle eventuali restrizioni.

Le restrizioni per gli spostamenti tra regioni sono imposte dal colore con cui sono contrassegnate, che indica il grado di emergenza. Più precisamente, in ordine crescente di criticità: white, yellow, orange, red.

Per una regione contrassegnata con il colore "red" (*) non è possibile superarne i confini.
>*Il divieto è limitato alle sole zone rosse al fine di riuscire a mostrare al meglio le funzionalità dell’algoritmo di ricerca dei percorsi.

Per valutare la criticità dell'emergenza SARS-CoV-2 si utilizza un EPI, ricavato da un dataset contenente l'andamento dei contagi giornaliero per ogni regione.<br><br>
```EPI (Epidemics Progression Index): https://www.cdc.gov/foodsafety/outbreaks/investigating-outbreaks/epi-curves.html```<br>

In base agli EPI analizzati nel dataset, il programma effettua una previsione sull'andamento dell'epidemia per una settimana futura.
In seguito alla previsione, con l'ausilio di un ulteriore dataset che indica l'assegnamento delle restrizioni applicate nelle settimana precedenti, effettua una classificazione dei colori (restrizioni) per la settimana futura.

Infine, basandosi sui dati calcolati, una feature permette di ricercare il percorso (ove questo esista) per effettuare uno spostamento tra due regioni date in input con l'obiettivo di correre il minor rischio possibile.


<h2 id="dati"> 2. Dati </h2>

Il programma utilizza tre differenti tipologie di data set disponibili tramite file .csv presenti su repository github:<br><br>
2.1) **Dati sulle regioni**. In particolare un documento .csv per ogni regione in cui vengono indicati, suddivisi per giorni, i dati relativi ai casi totali per regione e numero di tamponi effettuati.<br>
Ad esempio la tabella della regione Puglia è così strutturata:<br><br>
![Help Example](/img/Immagine.PNG)<br><br>
Questi dati verranno utilizzati dal programma al fine del calcolo dell’EPI.<br><br>
2.2) Per il task della classificazione invece, viene utilizzata la tabella seguente, la quale contiene **aggiornamenti settimanali sui colori (restrizioni)** assegnati alle regioni.<br><br>
![Help Example](/img/TabellaColori.PNG)<br><br>
2.3) Dati inerenti la popolazione di ogni regione per effettuare con una maggiore precisione il task di classificazione<br><br>
![Help Example](/img/TabellaPopolazione.PNG)<br>

<h2 id="regioni"> 3. Regioni</h2>
Nel programma ogni regione è stata rappresentata mediante un'apposita classe (Regione.py), così definita:<br><br>
<table>
<tr><td>Attributi</td></tr>
<tr><td>name</td><td>Nome della regione</td></tr>
<tr><td>epi</td><td>EPI medio calcolato per la settimana futura rispetto agli utimi dati registati nel dataset</td></tr>
<tr><td>color</td><td>Colore ("white","yellow","orange","red") che indica il grado di criticità dei contagi</td></tr>
</table>
<table>
<tr><td>Metodi</td></tr>
<tr><td>avgEPIByDate (self, dataCalcolo)</td><td>Metodo per il calcolo dell'EPI medio, a partire da una settimana prima fino ad una data passata in input</td></tr>
<tr><td>avgEPI (self)</td><td>Metodo per il calcolo dell'EPI medio calcolato per la settimana futura rispetto agli utimi dati registati nel dataset</td></tr>
<tr><td>printGraphics(self)</td><td>Metodo per la visualizzazione dei grafici inenerenti l'andamento dei contagi registrati nelle ultime due settimane (prima della previsione) e la curva epidemiologica della previsione (con relativi margini di errore)</td></tr>
</table>


<h2 id="regioni"> 4. Previsione</h2>
L’applicazione, mediante l’accesso ad un data set, permette di effettuare delle predizioni sull’andamento del tasso di contagiosità (EPI) nella settimana seguente rispetto a quella dei dati di training.

In particolare, la previsione viene effettuata per ogni singola regione, mediante l’apposito metodo avgEPI della classe Regione. Per effettuare la previsione, il programma si basa sull’utilizzo di un **regressore linare** che opera con le informazioni relative agli ultimi 14 giorni presenti nel dataset mostrato precedentemente nella sezione Dati.

![Help Example](/img/EPI-all.png)

Il programma quindi, con l’ausilio del regressore lineare, calcola un ipotetico andamento della curva epidemiologica, che viene mostrato e visualizzato all’interno di grafici generati dal programma, affinchè si possa fornire una rappresentazione più intuitiva ai tassi di contagiosità calcolati per la settimana successiva.

Inoltre, il programma calcola anche l’**errore massimo e minimo** commesso dal modello nella predizione, e anch’esso viene mostrato nel grafico insieme all’andamento della curva previsto.

![Help Example](/img/EPI-prediction.png)

I **grafici** con tutte queste informazioni vengono mostrate richiamando sull’oggetto Regione d’interesse, il metodo printGraphics.

<h2 id="regioni"> 5. Classificazione</h2>

<h2 id="regioni"> 6. Ricerca Percorso</h2>
 
