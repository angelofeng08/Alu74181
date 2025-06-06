#### CAPITOLO 1 – Introduzione
Il lavoro presentato in questo documento descrive la realizzazione di un simulatore software in linguaggio C di un sistema logico-aritmetico sincronizzato a 32 bit, basato sull’utilizzo dell’unità ALU (Arithmetic Logic Unit) modello 74181 e registri PIPO (Parallel Input - Parallel Output). Questo tipo di sistema rappresenta la base fondamentale di ogni unità centrale di elaborazione (CPU), poiché permette l’esecuzione di operazioni aritmetiche e logiche su dati binari (0 o 1). Tali operazioni costituiscono il nucleo del funzionamento di tutti i dispositivi informatici moderni.
Nel seguente elaborato si farà riferimento al registro 74198 anche se non si è utilizzato, al suo posto è stato implementato un registro formato da flip flop singoli. Nel corso del report verrà comunque identificato come 74198 per semplicità.
L’ALU 74181 è uno dei primi esempi di unità logico-aritmetica realizzata come singolo circuito integrato TTL (Transistor-Transistor Logic). Progettata per operare su parole di 4 bit, questa ALU permette l’esecuzione di ben 48 diverse operazioni, tra cui somme, sottrazioni, operazioni logiche (AND, OR, XOR, NOT, ecc.) e altre manipolazioni complesse. La sua versatilità e modularità la rendono ideale per essere estesa a sistemi a più bit collegando in cascata più unità. In questo progetto, sono state utilizzate otto ALU 74181 per creare un sistema completo a 32 bit, con ciascuna ALU che gestisce un gruppo di 4 bit.
Per garantire una corretta gestione dei dati in ingresso e in uscita, vengono utilizzati registri di tipo PIPO. Questi registri permettono di caricare e scaricare simultaneamente tutti i bit di un dato, grazie all’utilizzo di un segnale di clock che sincronizza le operazioni. Rispetto ad altre tipologie di registro, i PIPO garantiscono una maggiore velocità di accesso ai dati, risultando quindi particolarmente adatti sia come registri di input per gli operandi, sia come registri di output per memorizzare temporaneamente il risultato delle operazioni.
Il processo di esecuzione di un’operazione parte dall’inserimento manuale o dal caricamento da file degli operandi e dei segnali di controllo. I valori vengono immessi nei registri di input, successivamente inviati alle ALU dove viene applicata l’operazione selezionata dall’utente, e infine memorizzati nel registro di output. Il risultato può essere visualizzato direttamente tramite un’interfaccia terminale oppure registrato in un file di log per successive analisi.
L’obiettivo principale del progetto è fornire uno strumento didattico per comprendere il funzionamento interno di un sistema di calcolo digitale. Attraverso la simulazione software, si riproduce fedelmente il comportamento reale di un sistema hardware digitale, evidenziando l’interazione tra le componenti logiche fondamentali come ALU e registri, nonché il ruolo della sincronizzazione nel corretto flusso dei dati.
Questo tipo di approccio consente agli studenti di acquisire una solida comprensione del funzionamento delle unità ALU, dei registri e del sistema di calcolo in generale, preparandoli alla progettazione e all’analisi di architetture hardware più complesse.

#### CAPITOLO 2 – Analisi dell’unità logico-aritmetica ALU 74181
L'unità logico-aritmetica (ALU) modello 74181 è uno dei primi circuiti integrati TTL progettati per eseguire operazioni aritmetiche e logiche su dati a 4 bit. Questo componente ha rappresentato un punto di svolta nell’architettura dei primi sistemi informatici, permettendo l’esecuzione di calcoli complessi all’interno di un singolo chip. L’ALU 74181 è stata ampiamente utilizzata nei primi microprocessori e continua oggi a essere studiata per il suo valore didattico e storico.
La 74181 riceve due operandi a 4 bit, indicati come A (A0–A3) e B (B0–B3) , e genera in uscita un risultato anch’esso a 4 bit (F0–F3 ). Il tipo di operazione effettuata dipende da diversi segnali di controllo: quattro linee di selezione (S0–S3 ), una linea di modalità (M ) che distingue tra operazioni logiche e aritmetiche, e una linea per il riporto in ingresso (Cn ). Inoltre, l’ALU produce un riporto in uscita (Cn+4) e due segnali ausiliari P (Propagate Carry) e G (Generate Carry) , utilizzati per migliorare la velocità delle somme multi-bit grazie al sistema lookahead carry.
Funzioni supportate
La versatilità della 74181 si basa sulla sua capacità di eseguire ben 48 operazioni differenti , suddivise come segue:
16 operazioni logiche , attivate quando il segnale di modalità M = 1
16 operazioni aritmetiche senza carry-in , attivate con M = 0 e Cn = 0
16 operazioni aritmetiche con carry-in , attivate con M = 0 e Cn = 1
Le operazioni logiche includono funzioni booleane fondamentali come AND, OR, XOR, NOT, NAND, NOR e altre combinazioni. Le operazioni aritmetiche comprendono somma, sottrazione, incremento, decremento e altre manipolazioni numeriche, anche con gestione avanzata del riporto.
Struttura interna
All’interno del chip, la 74181 integra una rete complessa di porte logiche, full adder e multiplexer, progettati per selezionare e calcolare l’operazione richiesta in base ai segnali di controllo. La struttura combinatoria consente di generare simultaneamente tutti i bit del risultato, garantendo una risposta rapida e precisa alle operazioni richieste.
Ogni ALU opera in modo indipendente ma può essere collegata facilmente in cascata con altre unità grazie ai segnali Cn e Cn+4. Questa modularità rende possibile l’estensione del sistema a parole di dimensione maggiore, ad esempio fino a 32 bit utilizzando otto ALU collegate in parallelo.
Utilizzo nel simulatore software
Nel contesto del simulatore sviluppato in linguaggio C, l’ALU 74181 viene riprodotta in ambiente software mediante funzioni logiche implementate senza gli operatori del C, come AND (&), OR (|), NOT (!) e XOR (^). Ogni operazione prevista dall’ALU viene replicata attraverso opportune combinazioni di queste operazioni, garantendo fedeltà alla descrizione hardware originale.
Inoltre, vengono emulati i segnali di controllo S0–S3, M e Cn, permettendo all’utente di selezionare l’operazione desiderata tramite interfaccia terminale o file di configurazione. Il risultato dell’operazione viene visualizzato direttamente a schermo oppure registrato in un file di log per successive analisi.
L’utilizzo della 74181 nel simulatore offre un approccio pratico per comprendere il funzionamento delle ALU reali, nonché un’opportunità per studiare il comportamento delle operazioni logiche e aritmetiche su dati binari, inclusa la propagazione del riporto e le tecniche di ottimizzazione come il look ahead carry.

#### CAPITOLO 3 – Funzioni Logiche Supportate
L’unità logico-aritmetica (ALU) modello 74181 è in grado di eseguire ben 16 operazioni logiche distinte, attivabili impostando il segnale di modalità M = 1. Queste operazioni agiscono direttamente sui singoli bit degli operandi A e B senza coinvolgere il riporto (carry-in Cn) e sono utilizzate per manipolare i dati a livello binario.
Le funzioni logiche implementate dall’ALU 74181 permettono di effettuare operazioni fondamentali come AND, OR, EXOR, NOT, NAND, NOR e altre combinazioni booleane personalizzate. Ogni operazione è selezionata attraverso i segnali di controllo S0–S3, che definiscono l’operazione da applicare su ciascuna coppia di bit (A_i, B_i).
Elenco delle principali operazioni logiche
Di seguito un elenco schematico delle operazioni logiche supportate, con indicazione del valore dei segnali S0–S3 secondo la tavola di verità “Active-high Data”
Queste operazioni rappresentano le basi della logica digitale e vengono utilizzate frequentemente nei sistemi di elaborazione dati per effettuare confronti, mascheramenti, rotazioni, inversioni di bit e altre manipolazioni avanzate.
Applicazioni pratiche delle operazioni logiche
- AND: utile per il mascheramento di bit, ovvero per isolare determinati bit all’interno di un dato.
- OR: permette di impostare specifici bit a 1 senza modificare gli altri.
- EXOR: usato per invertire selettivamente bit o per effettuare somme senza carry.
- NOT: nega tutti i bit di un registro, utile per calcolare il complemento a uno.
Simulazione software
Nel contesto del simulatore sviluppato in linguaggio C, le operazioni logiche dell’ALU 74181 vengono riprodotte senza l’utilizzo degli operatori logici fondamentali del C:
- (a * b) per l’AND al posto di &
- (a + b - a * b) per l’OR al posto di |
- (a + b - 2 * a * b) per l’EXOR al posto di ^
- (1 - a) per il NOT al posto di ~
Ogni operazione viene emulata in base ai valori dei segnali di controllo S0–S3 e M forniti dall’utente tramite interfaccia terminale o file di configurazione.
Il risultato dell’operazione logica viene visualizzato sia a schermo che in un file di log, garantendo una tracciabilità completa delle operazioni effettuate.
Le operazioni logiche supportate dall’ALU 74181 rappresentano un insieme molto ricco e versatile di strumenti per la manipolazione diretta dei dati binari. La loro corretta simulazione nel codice software consente di replicare fedelmente il comportamento di un sistema hardware reale, rendendo il progetto non solo un valido strumento didattico ma anche un ambiente di test per futuri sviluppi hardware/software.

#### CAPITOLO 4 – Operazioni Aritmetiche Supportate
L’unità logico-aritmetica (ALU) modello 74181 è in grado di eseguire ben 32 operazioni aritmetiche, suddivise in due gruppi principali:
- 16 operazioni aritmetiche senza carry-in, attivabili impostando il segnale di modalità M = 0 e il riporto in ingresso Cn = 0
- 16 operazioni aritmetiche con carry-in, attivabili sempre con M = 0, ma con Cn = 1
Queste operazioni permettono di effettuare calcoli fondamentali come somma, sottrazione, incremento e decremento su operandi binari a 4 bit. L’utilizzo del segnale Cn (Carry-in) consente di collegare più ALU tra loro per estendere il sistema a parole di dati più lunghe (es. 32 bit), gestendo correttamente il riporto tra i vari blocchi.
Elenco delle principali operazioni aritmetiche
Di seguito un elenco schematico delle operazioni aritmetiche supportate, con indicazione del valore dei segnali di controllo S0–S3, M e Cn:
Le operazioni mostrate rappresentano la base del calcolo digitale e sono utilizzate frequentemente nei sistemi di elaborazione dati per effettuare somme, sottrazioni, confronti, scalamenti e altre manipolazioni matematiche avanzate.
Applicazioni pratiche delle operazioni aritmetiche
- Somma: utilizzata per l’addizione diretta di due valori binari
- Sottrazione: realizzata tramite somma col complemento a due
- Incremento/Decremento: utile per contatori o indirizzamento dinamico
- Complemento a uno/due: usato per invertire i valori binari o preparare operandi negativi
- Somma con NOT: impiegata in operazioni condizionali e test logici
Simulazione software
Nel contesto del simulatore sviluppato in linguaggio C, le operazioni aritmetiche dell’ALU 74181 vengono riprodotte utilizzando funzioni implementate con operatori logici fondamentali del C:
- + per la somma
- - per la sottrazione
- ~ per il complemento a uno
- ! per la negazione logica

Ogni operazione viene emulata in base ai valori dei segnali di controllo S0–S3, M e Cn forniti dall’utente tramite interfaccia terminale o file di configurazione.
Il risultato dell’operazione aritmetica viene visualizzato sia a schermo che in un file di log, garantendo una tracciabilità completa delle operazioni effettuate.
Il ruolo del Carry-in (Cn)
Il segnale Cn (carry-in) gioca un ruolo fondamentale nelle operazioni aritmetiche, poiché permette di propagare il riporto da un’ALU precedente. Questo è essenziale quando si collegano più ALU per formare un sistema a 32 bit. Senza il corretto uso del carry, non sarebbe possibile effettuare somme e sottrazioni su numeri composti da più di 4 bit.
Inoltre, alcune operazioni dell’ALU 74181 utilizzano il carry per generare risultati diversi: ad esempio, la stessa combinazione di segnali S0–S3 può produrre una somma semplice oppure una somma con carry, a seconda dello stato del segnale Cn.
Le operazioni aritmetiche supportate dall’ALU 74181 rappresentano un insieme molto ricco e versatile di strumenti per la manipolazione matematica dei dati binari. La loro corretta simulazione nel codice software consente di replicare fedelmente il comportamento di un sistema hardware reale, rendendo il progetto non solo un valido strumento didattico ma anche un ambiente di test per futuri sviluppi hardware/software.

#### CAPITOLO 5 – Tabella delle Funzioni (Truth Table)
La ALU 74181 è un componente logico altamente versatile, in grado di eseguire ben 48 operazioni distinte, selezionabili attraverso i segnali di controllo S0–S3, la modalità M e il riporto in ingresso Cn. La corretta comprensione del comportamento dell’unità richiede l’utilizzo di una truth table (tabella della verità) che permette di visualizzare chiaramente quale operazione viene eseguita per ogni combinazione dei segnali di controllo.
Struttura della Truth Table
La truth table dell’ALU 74181 può essere suddivisa in tre sezioni principali:
1. Operazioni Logiche (M = 1)  
   In questa configurazione, l’ALU esegue operazioni booleane dirette su ciascun bit degli operandi A e B, senza coinvolgere il carry-in. Queste operazioni sono utili per manipolazioni a livello di singoli bit, come mascheramenti o inversioni.
2. Operazioni Aritmetiche senza Carry-in (M = 0, Cn = 0)  
   L’ALU esegue somme, sottrazioni e altre operazioni matematiche su valori binari, assumendo che non vi sia alcun riporto iniziale. Queste operazioni sono utilizzate principalmente in contesti dove non è necessario propagare il carry da ALU precedenti.
3. Operazioni Aritmetiche con Carry-in (M = 0, Cn = 1)  
   Simile alla categoria precedente, ma include il contributo del carry-in, fondamentale per collegare più ALU tra loro e gestire operazioni su dati di dimensioni superiori ai 4 bit.
Esempio di Truth Table semplificata
Di seguito una tabella schematica che mostra alcuni esempi rappresentativi delle operazioni supportate:

| M | S3 | S2 | S1 | S0 | Cn | Operazione | 
|---|----|----|----|----|----|------------|
| 1 | 0  | 0  | 0  | 0  | X  | F = NOT(A)|
| 1 | 0  | 0  | 0  | 1  | X  | F = NOT(A + B) |
| 1 | 0  | 0  | 1  | 0  | X  | F = NOT(A)*B |
| 1 | 0  | 0  | 1  | 1  | X  | F = 0 |
| 1 | 0  | 1  | 0  | 0  | X  | F = NOT(AB) |
| 1 | 0  | 1  | 0  | 1  | X  | F = NOT(B) |
| 1 | 0  | 1  | 1  | 0  | X  | F = A ⊕ B |
| 1 | 0  | 1  | 1  | 1  | X  | F = A*NOT(B) |
| 1 | 1  | 0  | 0  | 0  | X  | F = NOT(A)+B |
| 1 | 1  | 0  | 0  | 1  | X  | F = NOT(A ⊕ B) |
| 1 | 1  | 0  | 1  | 0  | X  | F = B |
| 1 | 1  | 0  | 1  | 1  | X  | F = AB |
| 1 | 1  | 1  | 0  | 0  | X  | F = 1 |
| 1 | 1  | 1  | 0  | 1  | X  | F = A + NOT(B) |
| 1 | 1  | 1  | 1  | 0  | X  | F = A + B |
| 1 | 1  | 1  | 1  | 1  | X  | F = A |
| 0 | 0  | 0  | 0  | 0  | 0  | F = A PLUS 1 |
| 0 | 0  | 0  | 0  | 1  | 0  | F = (A + B) PLUS 1 |
| 0 | 0  | 0  | 1  | 0  | 0  | F = (A + NOT(B)) PLUS 1 |
| 0 | 0  | 0  | 1  | 1  | 0  | F = 0 |
| 0 | 0  | 1  | 0  | 0  | 0  | F = A PLUS (A*NOT(B)) PLUS 1 |
| 0 | 0  | 1  | 0  | 1  | 0  | F = (A+B) PLUS A*NOT(B) PLUS 1 |
| 0 | 0  | 1  | 1  | 0  | 0  | F = A MINUS B |
| 0 | 0  | 1  | 1  | 1  | 0  | F = A*NOT(B) |
| 0 | 1  | 0  | 0  | 0  | 0  | F = A PLUS AB PLUS 1 |
| 0 | 1  | 0  | 0  | 1  | 0  | F = A PLUS B PLUS 1 |
| 0 | 1  | 0  | 1  | 0  | 0  | F = (A+NOT(B)) PLUS AB PLUS 1 |
| 0 | 1  | 0  | 1  | 1  | 0  | F = AB |
| 0 | 1  | 1  | 0  | 0  | 0  | F = A PLUS A PLUS 1 |
| 0 | 1  | 1  | 0  | 1  | 0  | F = (A+B) PLUS A PLUS 1 |
| 0 | 1  | 1  | 1  | 0  | 0  | F = (A+NOT(B)) PLUS A PLUS 1 |
| 0 | 1  | 1  | 1  | 1  | 0  | F = A |
| 0 | 0  | 0  | 0  | 0  | 1  | F = A |
| 0 | 0  | 0  | 0  | 1  | 1  | F = A + B |
| 0 | 0  | 0  | 1  | 0  | 1  | F = A + NOT(B) |
| 0 | 0  | 0  | 1  | 1  | 1  | F = -1 |
| 0 | 0  | 1  | 0  | 0  | 1  | F = A PLUS A*NOT(B) |
| 0 | 0  | 1  | 0  | 1  | 1  | F = (A+B) PLUS A*NOT(B) |
| 0 | 0  | 1  | 1  | 0  | 1  | F = A MINUS B MINUS 1 |
| 0 | 0  | 1  | 1  | 1  | 1  | F = A*NOT(B) MINUS 1 |
| 0 | 1  | 0  | 0  | 0  | 1  | F = A PLUS AB |
| 0 | 1  | 0  | 0  | 1  | 1  | F = A PLUS B |
| 0 | 1  | 0  | 1  | 0  | 1  | F = (A+NOT(B)) PLUS AB|
| 0 | 1  | 0  | 1  | 1  | 1  | F = AB MINUS 1 |
| 0 | 1  | 1  | 0  | 0  | 1  | F = A PLUS A |
| 0 | 1  | 1  | 0  | 1  | 1  | F = (A+B) PLUS A |
| 0 | 1  | 1  | 1  | 0  | 1  | F = (A+NOT(B) PLUS A |
| 0 | 1  | 1  | 1  | 1  | 1  | F = A MINUS 1 |


> Nota: “X” indica valore indifferente
> Nella tabella seguente, AND è indicato come prodotto, OR con il segno +, XOR con ⊕, e più e meno aritmetici utilizzando le parole PLUS e MINUS.

Utilità della Truth Table nel simulatore software
Nel contesto del simulatore sviluppato in linguaggio C, la truth table è stata utilizzata come base per implementare la logica di selezione dell’operazione desiderata. Ogni volta che l’utente inserisce manualmente i valori di S0–S3, M e Cn, il programma consulta internamente una struttura simile alla truth table per determinare quale operazione deve essere eseguita.
Inoltre, il simulatore permette di caricare automaticamente i valori da file di configurazione, garantendo flessibilità nell’esecuzione ripetuta delle stesse operazioni.
Visualizzazione grafica
Per rendere più intuitiva la comprensione del funzionamento dell’ALU, è possibile integrare nel report una versione grafica della truth table, eventualmente generata dinamicamente dal programma stesso o tramite script esterni. Tuttavia, in assenza di supporto grafico, la rappresentazione testuale rimane efficace e precisa.
La truth table dell’ALU 74181 è uno strumento essenziale per comprendere il comportamento dell’unità logico-aritmetica e per implementarla correttamente in ambiente software. Essa consente di mappare ogni combinazione di segnali di controllo all’operazione corrispondente, facilitando la simulazione fedele del sistema reale. Nel progetto descritto, essa è stata utilizzata come base per la selezione e l’esecuzione delle operazioni aritmetiche e logiche, rendendo il simulatore uno strumento didattico completo e preciso.

#### CAPITOLO 6 – Gestione del Carry-in e Carry-out
Uno degli aspetti fondamentali del funzionamento dell’unità logico-aritmetica (ALU) modello 74181 è la corretta gestione del carry-in (Cn) e del carry-out (Cn+4) . Questi segnali rappresentano i bit di riporto che permettono di collegare più ALU tra loro, estendendo il sistema originale a parole di dati più lunghe, come quelle da 32 bit .
Il ruolo del Carry nella somma e sottrazione
Il carry-in (Cn) indica se esiste un riporto proveniente da un'unità ALU precedente. È utilizzato principalmente durante le operazioni aritmetiche come la somma e la sottrazione, dove il risultato può generare un overflow rispetto ai 4 bit disponibili.
Quando si effettua una somma semplice , Cn viene impostato a 0
Quando si effettua una somma con riporto , Cn viene impostato a 1
Un esempio pratico:
Somma senza carry: A + B → Cn = 0
Somma con carry: A + B + 1 → Cn = 1
La stessa logica si applica alla sottrazione, dove il carry viene interpretato come un "prestito" per garantire la correttezza del calcolo.
Funzionamento del Carry-out (Cn+4)
Il carry-out (Cn+4) rappresenta il riporto generato dall’ALU al termine dell’operazione. Questo valore viene trasmesso all’ALU successiva, permettendo l’esecuzione sequenziale delle operazioni su parole di dimensione maggiore.
Ad esempio, quando si collegano otto ALU 74181 per formare un sistema a 32 bit:
La prima ALU elabora i bit 0–3 e genera un carry-out
Questo carry-out diventa il carry-in della seconda ALU , che elabora i bit 4–7
Il processo continua fino all’ultima ALU (bit 28–31)
Questa architettura consente di eseguire operazioni aritmetiche complesse su numeri a 32 bit, mantenendo una struttura modulare e scalabile.
Simulazione software del Carry
Nel contesto del simulatore sviluppato in linguaggio C, la gestione del carry avviene attraverso una serie di funzioni dedicate che emulano fedelmente il comportamento dell’hardware reale.
Ogni volta che viene eseguita un’operazione aritmetica, il programma:
Riceve il valore del carry-in (Cn) fornito dall’utente o letto da file
Esegue l’operazione richiesta
Genera il risultato a 4 bit
Calcola il carry-out (Cn+4) in base al risultato e al tipo di operazione
Lo passa all’ALU successiva
L’implementazione tiene conto sia della propagazione del carry, sia della sua generazione locale grazie ai segnali ausiliari P (Propagate Carry) e G (Generate Carry) , utilizzati nel sistema lookahead carry generator .
Segnali P (Propagate Carry) e G (Generate Carry)
Oltre al semplice riporto, l’ALU 74181 fornisce due segnali aggiuntivi:
P (Propagate Carry): indica che il riporto ricevuto deve essere propagato all’ALU successiva
G (Generate Carry): indica che il riporto è stato generato localmente
Questi segnali sono utilizzati in combinazione per implementare un sistema avanzato di lookahead carry , che riduce drasticamente il tempo necessario per calcolare il riporto finale in somme multi-bit.
In termini matematici:
Se G = 1 , il riporto viene generato indipendentemente dal valore del carry-in
Se P = 1 , il riporto dipenderà dal carry-in
Quindi:
Cn+4 = G + (P * Cn)
Questa formula permette di calcolare anticipatamente il valore del carry-out, evitando il ritardo causato dalla propagazione seriale tradizionale.
Applicazioni pratiche
La corretta gestione del carry è essenziale in diversi scenari:
Somma e sottrazione multi-bit
Operazioni condizionali basate sul riporto
Implementazione di unità aritmetiche avanzate
Calcoli con operandi negativi (complemento a due)
Senza una corretta gestione del carry, non sarebbe possibile estendere il sistema oltre i 4 bit né effettuare calcoli complessi in modo affidabile.
La gestione del carry-in e del carry-out rappresenta uno dei punti chiave nell’utilizzo dell’ALU 74181. Essa permette di collegare più ALU tra loro, creando sistemi sincronizzati in grado di elaborare dati di grandi dimensioni. Nel simulatore software sviluppato in C, questa logica è stata riprodotta fedelmente, garantendo una corretta propagazione del riporto tra le ALU e un comportamento conforme a quanto avverrebbe in un sistema hardware reale. L’utilizzo dei segnali P e G ha permesso inoltre di implementare tecniche avanzate come il lookahead carry, migliorando notevolmente l’efficienza del sistema simulato.

#### CAPITOLO 7 – Lookahead Carry Generator
Uno dei limiti principali della somma binaria tradizionale, soprattutto quando si lavora con operandi composti da più bit, è il ritardo nella propagazione del carry . In un sistema sequenziale, ogni ALU attende il risultato dell’unità precedente prima di poter calcolare il proprio risultato e generare il riporto successivo. Questo processo seriale rallenta notevolmente l’esecuzione di operazioni aritmetiche su dati a lunghezza maggiore (es. 32 bit), riducendone l’efficienza.
Per superare questo limite, l’ALU 74181 implementa una logica avanzata chiamata Lookahead Carry Generator , che permette di calcolare anticipatamente i valori del riporto per tutte le ALU coinvolte, eliminando la necessità di aspettare la propagazione seriale del carry tra le unità.
Principio di funzionamento
Il Lookahead Carry Generator utilizza due segnali ausiliari forniti dall’ALU:
P (Propagate Carry) : indica che il riporto ricevuto deve essere trasmesso all’ALU successiva
G (Generate Carry) : indica che il riporto viene generato localmente, indipendentemente dal valore del carry-in
Questi segnali vengono combinati matematicamente per calcolare direttamente il valore del carry-out (Cn+4) di ciascuna ALU senza dover attendere il risultato delle unità precedenti.
La formula base per il calcolo del carry è:
Cn+4 = G + (P × Cn)
Dove:
G rappresenta il carry generato localmente
P rappresenta la possibilità di propagare il carry in ingresso
Cn è il carry in ingresso
Questa equazione consente di determinare anticipatamente il valore del riporto per ogni ALU, evitando il ritardo causato dalla propagazione seriale tipica dei sistemi tradizionali.
Vantaggi del Lookahead Carry
L’utilizzo del Lookahead Carry Generator offre diversi vantaggi:
Riduzione del tempo di calcolo : poiché i riporti vengono calcolati in parallelo, il tempo totale necessario per completare una somma multi-bit non dipende più dal numero di bit coinvolti.
Maggiore efficienza nei sistemi a 32 bit : collegando otto ALU 74181, il Lookahead Carry elimina il collo di bottiglia della propagazione seriale del carry.
Migliore scalabilità : il sistema può essere esteso a parole di dimensione maggiore senza penalità significative sulle prestazioni.
Implementazione software nel simulatore
Nel contesto del simulatore sviluppato in linguaggio C, il Lookahead Carry Generator è stato emulato attraverso funzioni dedicate che replicano fedelmente il comportamento hardware reale.
Ogni volta che viene eseguita un’operazione aritmetica:
L’ALU calcola i valori di P e G
Il programma applica la formula del Lookahead Carry per determinare il carry-out
Il risultato del carry-out viene passato all’ALU successiva come carry-in
Questo approccio garantisce un alto livello di accuratezza nella simulazione del sistema reale e permette di visualizzare chiaramente il comportamento del carry durante le operazioni aritmetiche complesse.
Esempio pratico di Lookahead Carry
Supponiamo di voler sommare due numeri a 8 bit utilizzando due ALU 74181 collegate in cascata:
Prima ALU (bit 0–3): genera i segnali P0 e G0
Seconda ALU (bit 4–7): genera P1 e G1
Utilizzando la logica del Lookahead Carry, possiamo calcolare direttamente i carry-out di entrambe le ALU:
C0 = Cn
C1 = G0 + P0 × C0
C2 = G1 + P1 × C1 = G1 + P1 × (G0 + P0 × C0)
In questo modo, il carry finale C2 può essere calcolato prima ancora che le ALU abbiano completato la somma , accelerando drasticamente l’intera operazione.
Applicazioni pratiche
Il Lookahead Carry Generator è fondamentale in molteplici scenari:
Somme rapide su grandi operandi
Sistema di elaborazione a virgola mobile
Unità FPU (Floating Point Unit)
Calcoli critici dove la velocità è essenziale
Senza questa logica, molti sistemi informatici moderni soffrirebbero di gravi ritardi nelle operazioni matematiche di base.
Il Lookahead Carry Generator rappresenta una soluzione ingegnosa al problema della propagazione seriale del carry. La sua integrazione nell’ALU 74181 ha reso possibile la realizzazione di sistemi sincroni ad alte prestazioni, capaci di gestire operazioni aritmetiche complesse in tempi ridotti.
Nel simulatore software sviluppato in C, questa logica è stata correttamente implementata grazie alla gestione avanzata dei segnali P e G , garantendo una rappresentazione precisa del comportamento reale dell’hardware originale. L’utilizzo di questa tecnica rende il sistema non solo più veloce, ma anche più affidabile e scalabile, aprendo la strada a futuri miglioramenti e ottimizzazioni.

#### CAPITOLO 8 – Schema Interno dell’ALU 74181
L’unità logico-aritmetica (ALU) modello 74181 è un circuito integrato TTL che integra al suo interno una complessa rete logica progettata per eseguire operazioni aritmetiche e logiche su dati a 4 bit. La sua architettura interna si basa su una combinazione di porte logiche fondamentali, multiplexer e full adder, che permettono di selezionare e calcolare l’operazione richiesta in base ai segnali di controllo forniti dall’esterno.
Struttura logica principale
La ALU 74181 riceve in ingresso due operandi a 4 bit, indicati come A0–A3 e B0–B3 , e genera in uscita un risultato anch’esso a 4 bit (F0–F3 ). Il tipo di operazione eseguita dipende da diversi segnali esterni:
S0–S3 : segnali di selezione dell’operazione
M : modalità (logica o aritmetica)
Cn : carry-in (riporto in ingresso)
Ogni bit del risultato viene calcolato attraverso una funzione combinatoria che coinvolge i corrispondenti bit degli operandi e i segnali di controllo. Queste funzioni sono implementate utilizzando operatori logici fondamentali come AND, OR, NOT e XOR, opportunamente combinati.
Un elemento chiave della struttura interna è la presenza di un multiplexer a 4 vie per ogni bit di output, che seleziona tra le diverse operazioni disponibili in base ai valori dei segnali S0–S3. In parallelo, vengono calcolati anche i segnali ausiliari P (Propagate Carry) e G (Generate Carry) , utilizzati nel sistema lookahead carry generator per migliorare la velocità delle somme multi-bit.
Componenti principali
All’interno dell’ALU 74181 possiamo identificare i seguenti componenti chiave:
1. Multiplexer
I multiplexer sono utilizzati per selezionare quale operazione deve essere applicata ai bit di input. Ogni bit del risultato F_i dipende dal valore di A_i, B_i e dai segnali di controllo S0–S3. I multiplexer scelgono la giusta combinazione in base alla truth table definita per l’ALU.
2. Porte logiche fondamentali
AND, OR, NOT, XOR e altre porte logiche vengono utilizzate per generare i segnali intermedi necessari alle operazioni. Queste porte lavorano in parallelo per garantire una risposta rapida e precisa.
3. Full Adder
Per le operazioni aritmetiche, l’ALU utilizza dei full adder , ciascuno dedicato a un singolo bit. I full adder calcolano la somma tra A_i, B_i e il riporto entrante Cn, producendo il bit di risultato F_i e il riporto uscente Cn+4.
4. Generatori di P e G
Due circuiti separati calcolano i segnali P (Propagate Carry) e G (Generate Carry) . Questi segnali sono essenziali per il sistema lookahead carry, poiché permettono di determinare anticipatamente il valore del carry-out senza dover attendere la propagazione seriale.
5. Segnale M (Modalità)
Il segnale M agisce come un selettore tra le operazioni logiche (M = 1) e quelle aritmetiche (M = 0). Quando M = 1, il carry non viene considerato e le operazioni avvengono in maniera indipendente sui singoli bit. Quando M = 0, invece, si attiva il sistema di propagazione del riporto.
Funzionamento dettagliato di un bit
Analizziamo il comportamento del circuito relativo al singolo bit, ad esempio il bit 0:
Vengono letti i valori di A0 , B0 e il segnale Cn
I segnali S0–S3 e M vengono interpretati e decodificati
Un multiplexer seleziona la funzione da applicare a seconda della combinazione
Il risultato F0 viene generato e inviato all’uscita
Viene calcolato il valore del carry-out (Cn+4) , insieme ai segnali P e G
Questo processo si ripete in parallelo per tutti e quattro i bit elaborati da ogni ALU.
Simulazione software
Nel contesto del simulatore sviluppato in linguaggio C, la struttura interna dell’ALU 74181 è stata emulata mediante l’utilizzo di funzioni dedicate che replicano fedelmente il comportamento delle porte logiche e dei multiplexer. Inoltre, il programma simula il comportamento del multiplexer utilizzando strutture condizionali (if, else if) o tabelle di lookup per scegliere l’operazione desiderata in base ai valori di S0–S3 e M.
Esempio di funzione logica
Un esempio semplificato di calcolo del risultato per un singolo bit può essere dato da.
Questa funzione rappresenta in modo astratto il comportamento di un singolo bit dell’ALU, mostrando come venga selezionata l’operazione in base al segnale M .
Applicazioni pratiche dello schema interno
La comprensione dello schema interno dell’ALU 74181 è fondamentale per:
Progetti di simulazione software
Studio del funzionamento delle unità ALU
Progettazione hardware digitale
Implementazione di sistemi sincronizzati a più bit
Analisi avanzata del ritardo logico e della propagazione del carry
Lo schema interno dell’ALU 74181 rappresenta un esempio classico di come possono essere integrate le operazioni logiche e aritmetiche all’interno di un unico chip. L’utilizzo di porte logiche, multiplexer e full adder consente di realizzare un componente altamente versatile, capace di supportare ben 48 operazioni distinte. La sua simulazione software, pur mantenendo un certo livello di astrazione, riesce a riprodurre fedelmente il comportamento originale, rendendo possibile lo test e l’apprendimento avanzato del funzionamento delle ALU.

#### CAPITOLO 9 – Espansione a 32 bit tramite 8 ALU 74181
Una delle caratteristiche più interessanti dell’unità logico-aritmetica (ALU) modello 74181 è la sua capacità di essere facilmente espansa per operare su parole di dati più lunghe. Pur essendo progettata per elaborare operandi a 4 bit , questa ALU può essere collegata in cascata con altre unità simili per costruire sistemi sincronizzati a 8, 16 o addirittura 32 bit . Questo tipo di architettura modulare è stato ampiamente utilizzato nei primi computer e rimane oggi uno strumento didattico fondamentale per comprendere i principi alla base del calcolo digitale.
Architettura modulare a 32 bit
Per realizzare un sistema a 32 bit , sono necessarie otto ALU 74181 , ciascuna delle quali elabora un gruppo di 4 bit . Ogni ALU riceve i propri operandi A e B, mentre tutti condividono gli stessi segnali di controllo S0–S3 e M , che determinano l’operazione da eseguire. Il riporto (Cn ) viene propagato da un’ALU all’altra: il carry-out (Cn+4) dell’unità precedente diventa il carry-in (Cn) dell’unità successiva.
L’architettura si presenta come segue:
[ALU 0] → [ALU 1] → [ALU 2] → ... → [ALU 7]
(bit 0-3)   (bit 4-7)   (bit 8-11)        (bit 28-31)
Questo schema permette di effettuare operazioni aritmetiche e logiche su interi a 32 bit in maniera efficiente, grazie alla configurazione parallela che garantisce l’esecuzione simultanea su tutti i bit.
Funzionamento del sistema a 32 bit
Il processo di esecuzione di un’operazione inizia con l’inserimento dei valori in ingresso, che vengono caricati nei registri di input. Successivamente, i dati vengono inviati alle ALU dove viene applicata l’operazione selezionata (logica o aritmetica), scelta dall’utente attraverso i segnali di controllo. Il risultato finale viene memorizzato nel registro di output e reso disponibile all’utente attraverso un’interfaccia terminale o registrato in un file di log.
Ogni ALU opera in parallelo sugli stessi bit corrispondenti dei due operandi. Il carry-out generato da un’ALU viene passato al carry-in dell’ALU successiva, garantendo una corretta gestione del riporto tra le varie unità.
Collegamento tra ALU e propagazione del carry
Il collegamento tra le ALU avviene principalmente attraverso i segnali Cn (Carry-in) e Cn+4 (Carry-out) . In particolare:
La prima ALU riceve un carry-in iniziale (Cn) , impostabile a 0 o 1.
Il carry-out della prima ALU viene collegato al carry-in della seconda ALU.
Il processo continua fino all’ultima ALU, che genera il carry-out finale.
Questa catena consente di effettuare somme e sottrazioni su numeri a 32 bit, mantenendo la correttezza del risultato anche in presenza di overflow.
Un'ottimizzazione avanzata prevede l’utilizzo del sistema lookahead carry generator , che calcola anticipatamente i riporti evitando la propagazione seriale tradizionale. Questo approccio riduce drasticamente il ritardo nella somma multi-bit e aumenta l’efficienza complessiva del sistema.
Simulazione software in linguaggio C
Nel contesto del simulatore sviluppato in linguaggio C, l’espansione a 32 bit è stata implementata collegando otto istanze della funzione n_ALU74181(), ciascuna dedicata a un blocco di 4 bit. L’input è fornito sotto forma di due operandi a 32 bit, che vengono suddivisi in blocchi da 4 bit e assegnati alle rispettive ALU.
In questo modo, ogni ALU elabora il proprio segmento di dati e il carry-out viene passato all’unità successiva, replicando fedelmente il comportamento reale del sistema hardware.
Utilizzo dei registri PIPO.
I registri di tipo PIPO (Parallel Input - Parallel Output)giocano un ruolo chiave nell’implementazione del sistema a 32 bit. Essi consentono di caricare e scaricare tutti i bit in parallelo, grazie a un segnale di clock che sincronizza il trasferimento.
Ogni registro è formato da 32 bit , suddivisi in 8 gruppi da 4 bit, sincronizzati con lo stesso segnale di clock. I registri vengono utilizzati sia per immagazzinare gli operandi in input, sia per conservare il risultato dell’operazione eseguita dalle ALU.
Vantaggi dell’espansione a 32 bit
La possibilità di espandere l’ALU 74181 a 32 bit offre diversi vantaggi:
Maggiore precisione nei calcoli : consente di manipolare numeri interi molto grandi
Architettura modulare e scalabile : può essere estesa a 64 bit o oltre
Velocità di calcolo migliorata : grazie al parallelismo tra le ALU
Applicabilità pratica : simula l’approccio usato nei primi microprocessori
Applicazioni pratiche
Un sistema basato su 8 ALU 74181 e registri PIPO può essere utilizzato in diversi scenari:
Calcolo matematico avanzato
Simulazione di CPU semplici
Progetti di laboratorio didattico
Analisi di sistemi hardware digitali
L’utilizzo di ALU multiple consente inoltre di studiare fenomeni come il ritardo di propagazione del carry e la sincronizzazione tra componenti logiche.
L’espansione a 32 bit mediante 8 ALU 74181 rappresenta un esempio classico di come si possa costruire un sistema di calcolo digitale partendo da componenti elementari. L’uso combinato di ALU e registri PIPO rende il sistema non solo funzionalmente completo, ma anche efficiente e ben strutturato. Nel simulatore software sviluppato in C, questa architettura è stata riprodotta fedelmente, permettendo di testare e analizzare il comportamento del sistema in modo realistico e flessibile.

#### CAPITOLO 10 – Registri
I registri PIPO (Parallel Input - Parallel Output) rappresentano una tipologia fondamentale di registro utilizzata nei sistemi digitali sincroni per la memorizzazione temporanea di dati binari. Nel contesto del simulatore software sviluppato in linguaggio C, non è stato utilizzato il circuito integrato 74198, bensì è stato implementato un registro equivalente a 32 bit mediante l’uso di array di interi e funzioni basate su flip-flop SR a comportamento sincrono. Nonostante nel report si faccia riferimento al 74198 per semplicità, si sottolinea che la sua struttura con segnali di shift non è stata riprodotta; si è invece realizzato un modello fedele al comportamento sincrono parallelo tramite clock.
Funzionamento del registro simulato Il registro simulato è composto da 32 flip-flop SR, ciascuno implementato tramite una funzione n_SR_FLIP_FLOP() che riceve in ingresso i segnali D (bit da memorizzare), S_reg e R_reg (controllo di set e reset), il segnale di clock CLK, e un puntatore allo stato del clock precedente prev_CLK, oltre che puntatori ai valori di uscita Q e Q_bar. L’aggiornamento del contenuto avviene solo al fronte positivo del clock (CLK in salita).
La funzione n_PIPO74198() gestisce un gruppo di 8 flip-flop SR in parallelo, mentre la funzione reg_PIPO32() unisce quattro di questi gruppi per formare un registro completo a 32 bit. Questo approccio consente un caricamento parallelo di tutti i bit sincronizzato con il clock, riproducendo il comportamento caratteristico dei registri PIPO senza introdurre logica di shift.
Caratteristiche principali del registro simulato:
Input parallelo: 32 bit distribuiti su array D[]
Set/Reset: controllati da array S_reg[] e R_reg[]
Clock sincrono: variabile CLK condivisa e array prev_CLK[] per il rilevamento del fronte positivo
Output: array Q[] e Q_bar[] che rappresentano rispettivamente il valore memorizzato e il suo complemento logico
Simulazione del clock La sincronizzazione del sistema è gestita dalla funzione clock_step(), che simula il passaggio del tempo mediante delay() e inverte periodicamente il valore di CLK. Ogni fronte positivo attiva l’aggiornamento del registro tramite le funzioni flip-flop, rendendo la temporizzazione coerente con i sistemi hardware reali.
Applicazioni nel simulatore I registri simulati sono impiegati per:
Caricare in parallelo gli operandi A e B
Memorizzare i risultati prodotti dall’ALU a 32 bit
Sincronizzare tutte le operazioni con il segnale di clock
L’utilizzo di array e funzioni modulari permette un’architettura scalabile e fedele, pur non seguendo esattamente lo schema elettrico del 74198. Questa scelta consente una maggiore flessibilità nella simulazione e una migliore aderenza al comportamento logico desiderato.

#### CAPITOLO 11 – Funzionamento del Registro PIPO
Nel simulatore software sviluppato in linguaggio C, i registri PIPO non sono basati sul circuito integrato 74198, ma su una struttura personalizzata che riproduce le funzionalità fondamentali dei registri paralleli sincronizzati, senza includere la logica di shift o i segnali S e CLR. Il registro è realizzato mediante 32 flip-flop SR controllati da funzioni software e sincronizzati tramite clock.
Struttura e comportamento Ogni bit del registro è gestito da una funzione n_SR_FLIP_FLOP() che consente di aggiornare il valore memorizzato (Q) al fronte positivo del segnale di clock. I dati di input vengono memorizzati negli array D[], mentre gli array Q[] e Q_bar[] rappresentano lo stato attuale del registro e il suo complemento.
L'aggiornamento avviene solo quando il segnale CLK transita da 0 a 1, comportamento verificato dal confronto con prev_CLK[]. Il registro mantiene i dati finché non riceve un nuovo fronte positivo.
Caratteristiche operative:
Caricamento parallelo dei dati su 32 bit
Sincronizzazione con segnale di clock gestito da clock_step()
Mantenimento del valore finché il clock non cambia
Nessun supporto a funzioni seriali o segnali di shift/reset
Utilizzo nel simulatore I registri vengono utilizzati per:
Caricare gli operandi A e B nei registri di input
Memorizzare il risultato dell’ALU nel registro di output
Mantenere sincronizzato il trasferimento dati grazie alla temporizzazione


#### CAPITOLO 12 – Uso dei Registri come Input/Output
Nel sistema logico-aritmetico simulato, i registri PIPO realizzati con flip-flop SR svolgono un ruolo essenziale come unità di input e output. Ogni registro è composto da 32 bit gestiti in gruppi da 8 mediante n_PIPO74198(), e tutti i gruppi sono sincronizzati con lo stesso segnale di clock.
Ruolo nel sistema
Registro A: contiene il primo operando
Registro B: contiene il secondo operando
Registro F: contiene il risultato dell’ALU
Questi registri sono aggiornati al fronte positivo del clock, garantendo la coerenza del trasferimento dati. L’inserimento dei dati può avvenire manualmente o da file, e i valori binari vengono caricati nei registri tramite i rispettivi array.
Reset e mantenimento dello stato Il simulatore non prevede segnali espliciti di reset; il reset è implementato manualmente azzerando gli array Q[] e impostando Q_bar[] a uno. In assenza di fronte di clock, il contenuto dei registri rimane invariato.
Memorizzazione dei risultati Dopo l’esecuzione di un’operazione ALU, il risultato è scritto nel registro F. Questo valore può essere visualizzato a terminale o salvato su file. La funzione salva_in_memoria() permette di archiviare temporaneamente i risultati per analisi successive.
Conclusione L’implementazione dei registri PIPO nel simulatore C offre un’interfaccia sincrona affidabile e coerente, fondamentale per garantire il corretto funzionamento del sistema logico-aritmetico simulato. Pur non riproducendo esattamente il comportamento del 74198, la soluzione adottata risulta funzionalmente efficace e didatticamente chiara

#### CAPITOLO 13 – Simulazione Software del Sistema
La realizzazione di un simulatore software per un sistema logico-aritmetico sincronizzato a 32 bit, basato su ALU 74181 e registri PIPO 74198, richiede un’implementazione precisa delle operazioni logiche e aritmetiche, la corretta gestione dei segnali di controllo, la propagazione del carry tra le unità e l’uso di registri sincronizzati per il trasferimento dati. Il progetto è stato interamente sviluppato in linguaggio C, offrendo una rappresentazione fedele del comportamento del circuito hardware.
L’obiettivo è simulare il funzionamento di un sistema digitale sincronizzato composto da otto ALU 74181 collegate in cascata, registri PIPO per la gestione degli operandi e dei risultati, un sistema di controllo tramite segnali S0–S3, M e Cn, e un clock software per garantire la sincronizzazione dei componenti. Il simulatore consente sia l’inserimento manuale dei dati sia la lettura da file di configurazione, offrendo grande flessibilità d’uso.
Il codice è organizzato in moduli che implementano le operazioni logiche di base come AND, OR, NOT, XOR e varianti a più ingressi. Viene simulata una singola ALU 74181 a 4 bit, poi estesa a 32 bit mediante otto istanze collegate in sequenza. Ogni ALU elabora un segmento di 4 bit e condivide i segnali di controllo, mentre il carry-out di ciascuna è utilizzato come carry-in per la successiva. Questo schema assicura una corretta propagazione del riporto su tutta la lunghezza dei dati.
I registri PIPO sono emulati tramite array di interi, e il comportamento sincrono è riprodotto con l’uso di una variabile di clock e una funzione dedicata che simula il fronte positivo del segnale, permettendo il caricamento dei dati in modo sincronizzato.
L’interfaccia a terminale permette all’utente di inserire manualmente i valori di A, B, Cn, M e S0–S3, selezionare l’operazione desiderata, visualizzare il risultato e salvarlo in un file di log. Il programma include controlli di validazione per prevenire errori di input e garantire la correttezza delle operazioni simulate.
La lettura e la scrittura da file sono gestite tramite file di input predefiniti (come input_alu.txt) e file di output (risultati_alu32.txt). Se i file non sono presenti, vengono generati automaticamente. I risultati vengono salvati sia su file sia in una struttura di memoria temporanea che consente l’utilizzo dei dati per operazioni successive senza doverli reinserire manualmente.
La funzione di clock, richiamata a ogni aggiornamento, genera un impulso con ritardo programmabile e consente il trasferimento simultaneo dei dati tra le componenti del sistema. In un tipico ciclo di esecuzione, l’utente immette i valori, i dati vengono caricati nei registri di input, inviati alle ALU al fronte positivo del clock, elaborati, memorizzati nel registro di output e infine visualizzati o salvati.
Questa simulazione software rappresenta un’efficace dimostrazione pratica di come sia possibile replicare in ambiente virtuale il comportamento di un sistema hardware reale. Grazie all’impiego di funzioni logiche fondamentali, registri sincronizzati e un modello accurato dell’ALU 74181, il programma si rivela utile sia per finalità didattiche sia come base per futuri sviluppi verso architetture hardware/software più complesse.

#### CAPITOLO 14 – Struttura del Codice C
Il simulatore software descritto in questo progetto è stato realizzato in linguaggio C, scelto per la sua capacità di emulare con precisione il comportamento logico dei circuiti digitali. Il codice riproduce fedelmente l’architettura basata su ALU 74181 e registri PIPO, con particolare attenzione alla sincronizzazione, alla propagazione del carry e alla gestione degli input/output. L’intero programma è strutturato in più file e moduli, ciascuno con un compito specifico: uno dedicato alle funzioni logiche di base come AND, OR, NOT e XOR, uno per la simulazione della singola ALU 74181, uno per la gestione dei registri PIPO, uno per la logica generale del sistema e uno per utility come conversioni, memoria e logging. Questa struttura modulare rende il codice ordinato, leggibile e facilmente manutenibile.

Le operazioni logiche sono implementate tramite funzioni semplici ma precise, che riproducono il comportamento delle porte logiche dell’hardware originale. Tali funzioni vengono poi combinate per creare operazioni più articolate come porta_and_3(), porta_or_4() o porta_exor_5(), utilizzate per replicare la logica interna dell’ALU. La funzione principale che simula l’ALU 74181, denominata n_ALU74181(), riceve in ingresso il riporto iniziale, la modalità operativa, gli operandi a 4 bit e i segnali di selezione, e restituisce il risultato a 4 bit, i segnali ausiliari P, G, Cn+4 e un flag che segnala se i due operandi sono uguali. Questo schema consente di emulare in modo accurato la truth table dell’ALU reale.

Per estendere il sistema a 32 bit, sono state utilizzate otto istanze della funzione ALU, ognuna responsabile dell’elaborazione di 4 bit. Tutte le ALU condividono i segnali di controllo, mentre il carry-out di ciascuna viene passato alla successiva come carry-in, garantendo una propagazione corretta del riporto lungo l’intera parola. I registri PIPO sono stati simulati tramite array di interi, con comportamento sincrono riprodotto mediante una variabile di clock e una funzione che ne simula il fronte positivo. Quest’ultima consente il caricamento dei dati solo in momenti specifici, assicurando la sincronizzazione tra i vari componenti del sistema.

L’interfaccia a terminale permette all’utente di inserire manualmente i valori di A, B, Cn, M e S0–S3, selezionare l’operazione da eseguire, visualizzare il risultato e salvarlo in un file di log. Sono presenti controlli per evitare errori di inserimento e garantire che i valori rispettino i limiti del modello hardware simulato. Il simulatore supporta inoltre l’uso di file di configurazione e di output. I dati possono essere letti, ad esempio, dal file input_alu.txt e i risultati scritti nel file risultati_alu32.txt, entrambi generati automaticamente in assenza di file esistenti, con valori di default.

Dopo ogni esecuzione, il risultato viene mostrato a schermo e registrato in un file di log. Una funzione apposita consente di memorizzare i risultati anche in un array dinamico, rendendoli disponibili per analisi successive. Per aumentare la flessibilità, il programma prevede una memoria temporanea dove vengono conservati i risultati intermedi, permettendo di eseguire operazioni successive senza reinserire manualmente i dati. Il comportamento sincrono dell’intero sistema è gestito attraverso la funzione clock_step(), che genera impulsi di clock con un ritardo programmabile. Questa funzione viene attivata ogni volta che è necessario aggiornare lo stato del sistema, assicurando il trasferimento simultaneo dei dati.

Un ciclo di esecuzione tipico prevede l’inserimento dei valori degli operandi, il caricamento nei registri di input, l’invio alle ALU al fronte positivo del clock, l’elaborazione dell’operazione selezionata, la memorizzazione del risultato e infine la visualizzazione o il salvataggio a seguito di un nuovo impulso di clock. Questo progetto dimostra come sia possibile simulare in ambiente virtuale il comportamento di un sistema hardware reale, sfruttando un’accurata modellazione dell’ALU 74181, l’uso di registri sincronizzati e funzioni logiche fondamentali. Il simulatore risulta uno strumento utile sia in ambito didattico che per lo sviluppo di architetture hardware/software più complesse in futuro.

#### CAPITOLO 15 - Funzioni di Input Utente
Nel simulatore software del sistema logico-aritmetico sincronizzato a 32 bit, la gestione dell’input utente rappresenta una componente fondamentale. Essa permette all’utente di fornire operandi, segnali di controllo e configurazioni operative necessarie per eseguire le operazioni desiderate. Il programma è stato progettato per accettare sia input manuale da terminale che lettura automatica da file di configurazione , garantendo flessibilità e facilità d’uso.
Obiettivo della funzione di input
L’obiettivo principale della funzione di input è:
Raccogliere i valori degli operandi A e B
Leggere i segnali di controllo S0–S3, M e Cn
Gestire eventuali errori nell’inserimento dei dati
Fornire un’interfaccia intuitiva e interattiva
Ogni valore immesso viene validato prima di essere utilizzato, per evitare comportamenti anomali o risultati errati causati da dati non conformi al formato richiesto (es. valori diversi da 0 o 1).
Interfaccia terminale e menu principale
Il programma presenta un menù interattivo dove l’utente può scegliere tra diverse opzioni:
Eseguire un’operazione con ALU
Caricare i dati da file
Visualizzare i risultati
Uscire dal programma
Un esempio di menù principale è dato da:

**MENU PRINCIPALE**
[1] Inserisci operandi manualmente
[2] Leggi da file di input
[3] Visualizza risultati
[4] Salva risultati su file
[5] Convertitore Binario-Decimale
[6] Simula ALU 32 bit
[7] Ripeti ultima operazione
[8] Stampa memoria temporanea
[9] Misura ciclo clock
[0] Esci
Una volta selezionata un’opzione, il programma procede alla raccolta dei dati necessari per completare l’operazione scelta.
Lettura manuale dei dati
Quando l’utente sceglie di immettere i dati manualmente, il programma richiede in sequenza i seguenti parametri:
Parametri richiesti:
Cn (Carry-in) : 0 o 1
M (Modalità) : 0 = aritmetica, 1 = logica
A0–A3 : i 4 bit del primo operando A
B0–B3 : i 4 bit del secondo operando B
S0–S3 : i 4 segnali di selezione dell’operazione
Ogni valore viene letto tramite scanf() e immediatamente validato.
Questo approccio garantisce che solo valori validi vengano passati alle ALU.
Validazione dei dati
La correttezza dei dati immessi è essenziale per il corretto funzionamento del simulatore. Per ogni valore inserito, viene effettuato un controllo rigoroso:
I bit devono essere 0 o 1
I segnali di controllo devono appartenere ai valori ammessi
I numeri decimali devono essere compresi nel range supportato (0–4294967295)
In caso di errore, il programma visualizza un messaggio chiaro e impedisce l’esecuzione dell’operazione fino a che non vengono corretti i dati.
Lettura da file di configurazione
Per rendere più agevole l’utilizzo ripetuto del simulatore, il programma supporta anche la lettura da file di configurazione. L’utente può creare un file come input_alu.txt contenente tutti i variabili necessari:
Cn;M;A0-A3;B0-B3;S0-S3.
Se il file non esiste, il programma lo genera automaticamente con valori predefiniti, fornendo all’utente un template da completare.
Dopo la lettura, i valori vengono utilizzati direttamente per eseguire l’operazione richiesta.
Poiché il sistema opera in binario, i valori decimali immessi dall’utente vengono convertiti automaticamente in formato binario, distribuiti su 32 bit e divisi in gruppi da 4 bit per ciascuna ALU.
Questa funzione consente di trasformare un numero intero in un array di bit, compatibile con il formato richiesto dalle ALU.
Il simulatore permette di eseguire operazioni su più operandi consecutivi, memorizzando i risultati intermedi in una memoria temporanea . Questo consente di effettuare calcoli concatenati senza dover reinserire i dati ogni volta.
La memoria è implementata come un array dinamico.
L’utente può poi richiamare i risultati precedenti per effettuare nuove operazioni.
Dopo l’inserimento dei dati, l’utente riceve una conferma visiva sulle informazioni immesse.I dati possono essere anche registrati in un file di log per successive analisi.
La funzione di input utente gioca un ruolo chiave nella fruibilità e affidabilità del simulatore. Grazie alla combinazione di inserimento manuale e caricamento da file, il programma offre un alto livello di flessibilità, permettendo sia test rapidi che operazioni ripetute con configurazioni predeterminate. La validazione accurata dei dati assicura che il simulatore mantenga un comportamento coerente e fedele al sistema hardware reale. La possibilità di salvare i risultati in memoria e registrare i dati in formato leggibile rende il programma uno strumento completo per l’apprendimento e l’analisi del funzionamento delle ALU e dei registri sincronizzati.

#### CAPITOLO 16 – Gestione degli Errori
Nel simulatore software del sistema logico-aritmetico sincronizzato a 32 bit, la gestione degli errori è fondamentale per garantire il corretto funzionamento del programma. Poiché l’obiettivo è replicare fedelmente il comportamento di un circuito digitale reale, ogni valore immesso dall’utente deve rispettare vincoli precisi, come l’uso esclusivo di bit binari (0 o 1), l’aderenza ai limiti numerici previsti e il corretto formato dei file. Per evitare risultati errati o malfunzionamenti, sono state adottate strategie di validazione e controllo che intervengono in tutte le fasi dell’esecuzione.

Il programma verifica costantemente la correttezza dei dati inseriti, prevenendo l’uso di input non conformi, rilevando eventuali anomalie durante l’esecuzione e fornendo all’utente messaggi chiari per agevolare la correzione. Gli errori più comuni includono valori non binari nei segnali di controllo e negli operandi, file di configurazione inesistenti o malformati, operazioni aritmetiche non valide come la divisione per zero, overflow o underflow nei calcoli, problemi di accesso ai file o mancanza di memoria disponibile in fase di esecuzione.

Ogni input binario è sottoposto a un controllo immediato: valori diversi da 0 o 1 non vengono accettati e il programma restituisce un messaggio d’errore specifico, impedendo l’uso del dato scorretto. Durante l’inserimento manuale dei valori, viene verificato che i dati rientrino nel range ammesso, che non contengano caratteri non numerici e che siano compatibili con il formato richiesto. Questo evita che l’utente possa inserire lettere o simboli che porterebbero a un crash.

Quando il simulatore legge dati da file come input_alu.txt, vengono controllati sia l’esistenza del file sia il contenuto. Se il file è mancante, vuoto o contiene valori errati, viene generato automaticamente con parametri predefiniti o viene mostrato un avviso. Questo previene l’interruzione del programma a causa di una configurazione errata. Analogamente, per operazioni matematiche critiche come somme, sottrazioni o divisioni, il simulatore integra controlli preventivi per evitare errori come l’overflow o la divisione per zero.

Durante l’uso prolungato, il programma può allocare una quantità crescente di memoria per salvare risultati intermedi. In caso di superamento della capacità disponibile, la memoria viene espansa dinamicamente tramite realloc() e l’utente viene avvisato, assicurando la stabilità del sistema anche in situazioni più complesse.

Tutti gli errori rilevati sono registrati in un file di log (log_errore.txt), utile per eventuali analisi successive o debugging. Oltre al logging, il programma mostra i messaggi d’errore direttamente a schermo in formato leggibile e ben formattato. Ad esempio, se l’utente immette un valore errato per A2, il programma segnala chiaramente “ERRORE: A2 deve essere 0 o 1”, facilitando l’identificazione e la correzione del problema.

Oltre ai controlli sull’input, il programma verifica anche la coerenza interna dei dati elaborati. Durante l’esecuzione delle ALU, vengono effettuate verifiche sulla corretta propagazione del carry e sulla sincronizzazione tra i registri. Se, ad esempio, un registro non è stato aggiornato al momento giusto, viene emesso un messaggio di warning per segnalare possibili problemi di temporizzazione.

A supporto del debugging, è presente anche una modalità di test automatico, in cui il simulatore esegue operazioni predefinite con valori noti e confronta i risultati ottenuti con quelli attesi. Questo consente di identificare velocemente eventuali discrepanze nel comportamento del programma.

Grazie a questa articolata gestione degli errori, il simulatore garantisce affidabilità e robustezza anche in presenza di input errati o condizioni anomale. I controlli sull’input, la gestione sicura dei file, il monitoraggio dinamico della memoria e il logging sistematico rendono il programma uno strumento solido sia per scopi didattici che per possibili estensioni professionali. La combinazione di validazione automatica, feedback chiari e tracciamento continuo degli errori consente di esplorare in modo sicuro il comportamento del sistema hardware originale in un ambiente software controllato.

#### CAPITOLO 17 – Simulazione della Singola ALU
La simulazione software dell’unità logico-aritmetica 74181 costituisce uno degli elementi centrali del progetto. Questo componente, nato come circuito TTL a 4 bit, è stato fedelmente riprodotto in ambiente software utilizzando le funzioni logiche fondamentali del linguaggio C, rendendo possibile l’esecuzione di operazioni logiche e aritmetiche su operandi binari. L’obiettivo di questa simulazione è quello di replicare il comportamento reale dell’ALU, accettando due operandi a 4 bit (A0–A3 e B0–B3), ricevendo in ingresso i segnali di controllo S0–S3, la modalità operativa M e il segnale di carry-in Cn, e producendo come uscita un risultato a 4 bit (F0–F3) insieme ai segnali ausiliari P (Propagate), G (Generate) e Cn+4 (carry-out).

La logica interna dell’ALU 74181 è caratterizzata da una rete combinatoria di porte logiche che seleziona l’operazione desiderata in base ai segnali di controllo. Questa struttura è stata ricreata nel programma tramite l’uso degli operatori logici standard del C, come & per AND, | per OR, ! per NOT e ^ per XOR, combinati all’interno di funzioni ausiliarie come porta_and_3(), porta_or_4() e porta_exor_5(), necessarie per simulare il funzionamento della rete logica interna in modo fedele. Ogni bit dell’uscita F viene calcolato in base agli ingressi e ai segnali di controllo, replicando la logica originale del circuito.

La funzione principale che implementa l’ALU è n_ALU74181(), definita in modo da ricevere come parametri il carry-in, la modalità, i due operandi a 4 bit, i segnali S0–S3 e, tramite puntatori, restituire il risultato F[4], il flag A_uguale_B che segnala l’uguaglianza tra A e B, i segnali P e G, e il carry-out Cn+4. Questa struttura consente l’integrazione della funzione in sistemi più complessi e ne facilita il riutilizzo. Un aspetto cruciale dell’implementazione è la gestione avanzata del carry: i segnali P e G permettono di implementare il sistema di carry lookahead, e il carry-out viene calcolato secondo la formula *Cn_piu_4 = G + (P * Cn), consentendo una propagazione efficiente nei sistemi multi-bit.

Per supportare questa simulazione, sono state sviluppate funzioni logiche ausiliarie che consentono di replicare con precisione il comportamento delle porte multiple presenti nel circuito fisico. Prima di ogni esecuzione, il programma verifica la validità dei dati inseriti, assicurandosi che tutti i bit siano effettivamente 0 o 1 e che i segnali di controllo rientrino nel range previsto. In presenza di dati errati, l’esecuzione viene bloccata e viene mostrato un messaggio di errore all’utente.

Dopo l’elaborazione, il risultato viene visualizzato sia in formato binario che decimale, con la possibilità di salvarlo in un file di log per ulteriori analisi. Un tipico ciclo di utilizzo prevede l’inserimento manuale dei valori di A, B, Cn, M e S0–S3, la validazione e il caricamento nei registri di input, l’invio di un impulso di clock, l’elaborazione tramite n_ALU74181(), e infine la visualizzazione e memorizzazione del risultato. Il carry-out può essere poi trasferito a un’unità ALU successiva, supportando l’espansione a 32 bit.

Per semplificare l’utilizzo ripetuto del simulatore, è stata prevista la possibilità di leggere i parametri da un file di configurazione come input_alu.txt e di salvare i risultati in output, ad esempio in risultati_alu.txt. Se i file non sono presenti, il programma li genera automaticamente con valori predefiniti. Tutti i risultati vengono inoltre memorizzati in una memoria temporanea per supportare operazioni concatenate senza la necessità di reinserire i dati.

Il simulatore registra costantemente le informazioni relative alle operazioni eseguite, includendo gli operandi, i segnali di controllo, il risultato e lo stato del carry-out, per garantire una completa tracciabilità. Questa memoria, utile anche per il debug, consente di verificare il corretto funzionamento del sistema nel tempo. La simulazione della ALU 74181 rappresenta quindi un elemento chiave per comprendere il comportamento logico-aritmetico a livello hardware, fornendo una base solida per l’estensione a sistemi multi-ALU sincronizzati a 32 bit. Grazie all’utilizzo di operatori logici elementari e a una struttura modulare, il simulatore offre uno strumento completo per l’apprendimento, l’analisi e lo sviluppo di architetture digitali.

#### CAPITOLO 18 – Simulazione Multi-ALU (32 bit)
La simulazione software di un sistema logico-aritmetico sincronizzato a 32 bit, basato su otto unità ALU 74181 collegate in cascata, costituisce uno degli aspetti più avanzati del progetto. Questa architettura modulare permette di estendere le capacità computazionali della singola ALU a 4 bit fino a operare su parole da 32 bit, assicurando risultati coerenti con il comportamento atteso di un sistema hardware reale. Nonostante l’ALU 74181 sia progettata per operazioni a 4 bit, grazie alla presenza del segnale di carry-out e al supporto del lookahead carry generator, può essere facilmente connessa in serie con altre unità per formare un sistema capace di elaborare dati di lunghezza maggiore.

Nel simulatore, sviluppato in linguaggio C, vengono utilizzate otto istanze della funzione n_ALU74181(), ognuna delle quali gestisce un gruppo di 4 bit. Gli operandi A e B, rispettivamente da 32 bit, sono divisi in otto nibble da 4 bit ciascuno e distribuiti alle singole ALU. Tutte le unità operano sotto gli stessi segnali di controllo, S0–S3 e M, che determinano l’operazione da eseguire e la modalità (logica o aritmetica). Il carry-out generato da ciascuna ALU viene passato come carry-in all’unità successiva, garantendo una propagazione ordinata e corretta del riporto.

Il sistema funziona secondo una logica a blocchi, dove ogni ALU gestisce una porzione specifica dei bit, dal blocco meno significativo a quello più significativo. I dati in ingresso, forniti in formato decimale, vengono convertiti automaticamente in binario e caricati nei registri di input. La propagazione del clock consente di trasferire i dati nei registri PIPO, simulati tramite array, e da lì alle ALU. Ogni ciclo di operazione si compone di più fasi: caricamento parallelo dei dati, invio del clock, elaborazione sincrona nelle ALU, combinazione dei risultati parziali e memorizzazione finale nel registro di output.

La funzione di simulazione multi-ALU è gestita tramite un ciclo for che scorre su ciascun nibble, invocando l’ALU appropriata e passando i dati necessari. Questo approccio simula accuratamente il comportamento del sistema reale, in cui il carry e gli altri segnali ausiliari vengono gestiti in modo sequenziale e coerente. Oltre alla semplice trasmissione del carry, il sistema genera anche i segnali P e G, che permettono di calcolare anticipatamente il riporto grazie alla formula *Cn_piu_4 = G + (P * Cn), migliorando significativamente l’efficienza rispetto alla propagazione seriale pura.

Il simulatore integra una memoria temporanea che conserva i risultati intermedi e li rende disponibili per visualizzazioni o operazioni successive. Ogni risultato ottenuto viene anche salvato in un file di log specifico, facilitando l’analisi dei dati elaborati. L’interfaccia del programma è interattiva e consente all’utente di scegliere tra diverse opzioni operative, come l’inserimento manuale degli operandi, la lettura da file, la visualizzazione dei risultati, la conversione binario-decimale o la ripetizione dell’ultima operazione. Se si seleziona la simulazione a 32 bit, il programma richiede i valori di A, B, Cn, M e S0–S3 e procede con l’esecuzione dell’operazione su tutte e otto le ALU, rispettando l’ordine dei bit e la sincronizzazione con il clock.

Ogni input fornito dall’utente è sottoposto a controlli rigorosi per verificarne la validità e garantire l’affidabilità dell’esecuzione. I file di configurazione come input_alu32.txt possono essere utilizzati per caricare parametri di test, mentre i risultati vengono registrati automaticamente in file di output come risultati_alu32.txt. Se i file non esistono, il programma li crea con valori di default. I valori immessi vengono convertiti da decimale a binario e distribuiti nei registri di input, con l’output binario risultante che viene poi riconvertito in decimale per la visualizzazione all’utente.

I registri PIPO 74198 sono emulati tramite array che rappresentano i bit immagazzinati. La sincronizzazione dei dati avviene tramite una funzione clock_step() che simula la generazione di impulsi regolari, garantendo che i dati vengano trasferiti nei momenti corretti. Al fronte positivo del clock, i dati passano dai registri di input alle ALU, e successivamente al registro di output, replicando fedelmente il comportamento sincrono del sistema digitale.

Un’esecuzione tipica prevede che l’utente immetta i valori desiderati, che questi vengano validati e caricati nei registri, e che al primo impulso di clock i dati vengano elaborati. Al secondo impulso, i risultati vengono memorizzati e visualizzati. L’intero processo garantisce precisione, coerenza e riproducibilità delle operazioni logico-aritmetiche in un contesto software, emulando in maniera efficace l’architettura di un sistema digitale complesso. Grazie all’integrazione tra ALU 74181 e registri PIPO, e alla gestione avanzata della propagazione del carry, il simulatore rappresenta uno strumento potente e didatticamente valido per comprendere il funzionamento interno di sistemi sincroni e per porre le basi per ulteriori evoluzioni hardware o software.

#### CAPITOLO 19 – Interfaccia Utente (Menu)
L’interfaccia utente del simulatore software del sistema logico-aritmetico sincronizzato a 32 bit costituisce un elemento essenziale per la fruibilità del programma. È stata progettata per garantire un’interazione intuitiva con il sistema, permettendo all’utente di selezionare rapidamente le operazioni desiderate, immettere operandi e segnali di controllo, visualizzare i risultati in modo leggibile e salvare i dati elaborati. Il menù principale, mostrato all’avvio, offre un accesso centralizzato a tutte le funzionalità disponibili, organizzate in modo semplice e diretto. Ogni opzione può essere selezionata tramite l’inserimento di un numero corrispondente, rendendo il sistema accessibile anche a chi non ha grande familiarità con strumenti simili.

Quando l’utente sceglie di inserire i dati manualmente, il programma guida passo dopo passo nella digitazione dei valori decimali o binari per gli operandi A e B, del riporto iniziale Cn, della modalità M (indicante se si tratta di operazione logica o aritmetica) e dei segnali di selezione S0–S3. Ogni valore immesso viene sottoposto a un controllo di validità per garantire che rientri nei limiti consentiti e che la rappresentazione binaria sia corretta. In alternativa, è possibile caricare tutti questi parametri da un file di configurazione, generalmente denominato input_alu.txt, il quale può essere facilmente modificato per effettuare più test senza la necessità di reinserire ogni dato manualmente.

Dopo l’elaborazione, i risultati vengono presentati sia in formato binario sia decimale, rendendo più semplice l’interpretazione dell’output, anche per utenti meno esperti. Inoltre, tutti i risultati, insieme ai dati di input e alle informazioni relative all’operazione svolta, vengono automaticamente registrati in un file di log, risultati_alu32.txt, utile per successive verifiche o confronti. Il livello di logging può essere configurato per includere ulteriori dettagli, come lo stato dei registri prima e dopo l’esecuzione o il tempo impiegato dall’operazione.

Per aiutare l’utente nella conversione tra rappresentazioni numeriche, è inclusa una funzione dedicata alla trasformazione di valori binari in decimali e viceversa. Questa funzione semplifica la preparazione di input complessi e migliora la comprensione del funzionamento interno del sistema. Un altro componente fondamentale dell’interfaccia è la gestione della memoria temporanea, che conserva i risultati intermedi delle operazioni svolte. Grazie a questa struttura, è possibile eseguire operazioni concatenate senza reinserire ogni volta gli stessi dati. In qualsiasi momento, l’utente può stampare a schermo il contenuto della memoria per consultare lo storico delle operazioni.

Uno dei punti centrali dell’interfaccia è la possibilità di simulare il sistema a 32 bit composto da otto ALU 74181 collegate in cascata. Attraverso una singola opzione nel menù, è possibile fornire due operandi a 32 bit, selezionare i segnali di controllo e avviare la simulazione, che distribuisce i dati alle singole ALU, gestisce automaticamente i carry tra le unità e restituisce un risultato finale preciso. Una volta completata un’operazione, l’utente ha la possibilità di ripeterla immediatamente senza dover reinserire i dati: il programma memorizza l’ultima configurazione utilizzata e permette di rieseguire il calcolo con un solo comando. Questa funzionalità è particolarmente utile per effettuare test ripetuti e confrontare risultati in modo rapido.

Il contenuto della memoria temporanea può essere ispezionato in qualunque momento tramite una funzione dedicata. Vengono mostrati i risultati memorizzati, identificati da indici, facilitando l’analisi del comportamento del sistema nel tempo. Un’altra funzione disponibile consente di simulare un ciclo completo del sistema e misurare il tempo medio richiesto per completare un’operazione, utile per analisi prestazionali o per identificare eventuali colli di bottiglia nel codice.

Ogni input fornito dall’utente viene rigorosamente validato. I bit devono essere 0 o 1, gli operandi devono rientrare nel range supportato (da 0 a 4294967295), e tutti i segnali di controllo devono rispettare i vincoli logici e strutturali dell’ALU originale. In caso di errore, l’utente riceve immediatamente un messaggio chiaro, e l’esecuzione viene interrotta per evitare risultati incoerenti. Ogni errore viene anche salvato in un file di log separato, log_errore.txt, che registra data, ora e descrizione dell’errore riscontrato, facilitando notevolmente il debug.

Infine, per migliorare la leggibilità dei dati visualizzati, l’interfaccia utilizza una formattazione grafica avanzata. Questa scelta stilistica consente di identificare facilmente sia i risultati ottenuti che eventuali problemi, rendendo l’interazione con il simulatore non solo più funzionale ma anche più gradevole. L’interfaccia utente, quindi, svolge un ruolo chiave nel successo e nell’utilità del simulatore, integrando semplicità d’uso, robustezza tecnica e funzionalità complete in un unico ambiente operativo, adatto tanto per la formazione quanto per lo sviluppo e il testing di architetture logico-aritmetiche.

#### CAPITOLO 20 – Gestione del Clock nel Codice
La corretta gestione del segnale di clock è fondamentale per riprodurre fedelmente il comportamento sincrono del sistema logico-aritmetico descritto. Il simulatore software implementa la sincronizzazione tra ALU e registri PIPO attraverso una serie di funzioni dedicate che permettono di generare impulsi regolari, emulando il ciclo di clock tipico dei circuiti digitali.

Obiettivo della gestione del clock
L’obiettivo principale della simulazione del clock è:

Sincronizzare i trasferimenti di dati tra registri e ALU
Simulare il fronte positivo del clock , necessario per aggiornare i valori nei registri
Gestire il ritardo tra le operazioni , replicando il comportamento reale del sistema hardware
Fornire feedback visivo all’utente sullo stato del sistema
Il programma utilizza una variabile globale CLK per rappresentare lo stato del clock (0 o 1) e una variabile prev_CLK per registrare il valore precedente, utile per rilevare il fronte positivo.

Funzione principale: clock_step()
La funzione principale per la gestione del clock si chiama clock_step() ed è definita come segue. CLK : puntatore al valore attuale del clock
prev_CLK : puntatore al valore precedente del clock
milliseconds : durata del ritardo prima dell’aggiornamento
Questa funzione genera un ritardo programmabile e aggiorna lo stato del clock, permettendo di controllare la velocità di esecuzione del sistema.

Ogni volta che viene chiamata questa funzione, il programma attende per il tempo indicato (espresso in millisecondi), memorizza il valore precedente del clock e lo inverte.

Simulazione del ciclo completo del clock
Per generare un ciclo completo del clock (da 0 a 1 e poi da 1 a 0), è necessario chiamare la funzione due volte.
Questo approccio consente di simulare il comportamento reale del sistema digitale sincronizzato, dove i dati vengono caricati solo al fronte positivo del clock.

Utilizzo del clock nei registri PIPO
I registri PIPO sono emulati tramite array di interi e vengono aggiornati solo quando il clock passa da 0 a 1. Questo garantisce una perfetta sincronizzazione tra le componenti del sistema.

All’interno della funzione reg_PIPO32(), il caricamento avviene solo se si verifica un fronte positivo.
Questa struttura garantisce che i dati vengano trasferiti solo quando effettivamente richiesto dal sistema.

Controllo avanzato del clock
In alcuni casi, può essere utile monitorare il clock e misurarlo in tempo reale. Per questo motivo, il programma include una funzione dedicata alla misurazione del ciclo clock.
Questa funzione simula 100 cicli di clock e calcola il tempo medio impiegato, utile per analisi di prestazioni o debugging avanzato.

Output grafico migliorato
Per rendere più visibile il cambiamento dello stato del clock, il programma visualizza un output grafico ogni volta che avviene un aggiornamento.
Questa interfaccia migliorata aiuta l’utente a comprendere il comportamento sincrono del sistema.

Configurazione del ritardo del clock
Il programma permette di configurare la durata del ciclo clock, fornendo flessibilità nella simulazione. L’utente può scegliere tra diverse opzioni:

Ciclo rapido (10–100 ms)
Ciclo normale (500 ms)
Ciclo lento (1000 ms)
Logging del clock
Tutti i cicli di clock vengono registrati in un file di log (log_clock.txt) per successive analisi. Ogni riga contiene:

Data e ora del ciclo
Stato del clock
Durata del ciclo in millisecondi.
Questo tipo di registrazione è molto utile per tracciare eventuali problemi di sincronizzazione.

Esempio di sincronizzazione completa
Un tipico ciclo di sincronizzazione nel simulatore procede così:

I dati vengono immessi dall’utente o letti da file
I bit vengono distribuiti nei registri di input
Viene invocata la funzione clock_step() per generare un fronte positivo
I dati vengono trasferiti alle ALU
Le ALU eseguono l’operazione selezionata
Il risultato viene caricato nel registro di output
Un nuovo ciclo di clock rende il risultato disponibile all’esterno
La gestione del clock rappresenta uno degli elementi chiave per garantire la correttezza del comportamento del sistema simulato. Grazie alla funzione clock_step(), al controllo avanzato del ritardo e alla registrazione dettagliata delle transizioni, il programma riesce a riprodurre fedelmente il comportamento sincrono di un sistema digitale basato su ALU 74181 e registri PIPO. Questa capacità non solo migliora l’accuratezza del simulatore, ma lo rende anche uno strumento didattico efficace per comprendere il ruolo del clock nei sistemi logici sincronizzati.

#### CAPITOLO 21 – Logging dei Risultati
La registrazione (logging) dei risultati rappresenta una componente essenziale del simulatore software del sistema logico-aritmetico sincronizzato a 32 bit. Essa permette di memorizzare l’intera cronologia delle operazioni eseguite , fornendo all’utente uno strumento utile per analisi successive, debugging e validazione dei dati. Il programma implementa una gestione avanzata del logging che include sia la memorizzazione temporanea in memoria che la registrazione permanente su file.

Obiettivo del logging
L’obiettivo principale della funzione di logging è:

Tracciare tutte le operazioni effettuate
Memorizzare i valori degli operandi A e B
Registrare i segnali di controllo utilizzati (S0–S3, M, Cn)
Conservare il risultato dell’operazione in formato binario e decimale
Fornire un output leggibile e consultabile
Il logging consente di ripercorrere l’intera storia del lavoro svolto nel simulatore, facilitando il confronto tra i risultati attesi e quelli ottenuti.

Struttura della memoria temporanea
Per rendere disponibili i risultati intermedi, il programma implementa una memoria temporanea dinamica dove vengono conservati i risultati delle operazioni eseguite. Questo consente di effettuare calcoli concatenati senza dover reinserire i dati ogni volta.

Ogni volta che si esegue un’operazione, il risultato viene aggiunto alla memoria.

Funzione stampa_memoria()
La funzione stampa_memoria() permette di visualizzare il contenuto della memoria temporanea direttamente a schermo.
Questa funzione è particolarmente utile per il debug e per verificare il corretto funzionamento del sistema dopo molteplici esecuzioni consecutive.

Registrazione su file di log
Tutti i risultati vengono registrati in un file di log (risultati_alu32.txt) per garantire tracciabilità completa. Ogni volta che viene eseguita un’operazione, il programma salva:

I valori di A e B
I segnali di controllo S0–S3, M e Cn
Il risultato in formato decimale e binario
Lo stato del carry-out.
Questo tipo di logging garantisce una documentazione completa del comportamento del sistema.

Output grafico migliorato
Per rendere immediatamente visibili i risultati, il programma utilizza una formattazione grafica avanzata.
Questa interfaccia migliorata aumenta la leggibilità del risultato e semplifica l’interpretazione da parte dell’utente.

Lettura e scrittura da file
Il simulatore supporta anche la lettura da file di configurazione (input_alu.txt, input_alu32.txt, ecc.) e la scrittura su file di log (risultati_alu.txt, log_clock.txt, log_errore.txt).

Se il file non esiste, viene creato automaticamente con valori predefiniti.

Logging avanzato
Il programma include diverse opzioni di logging avanzato:

Log completo : registra tutti i dati dell’operazione (input, risultato, tempo)
Log minimo : registra solo il risultato finale
Log dettagliato : include informazioni sulle ALU coinvolte, i registri e lo stato del clock.
Questa funzionalità è molto utile per analisi approfondite o debugging.

Gestione degli errori durante il logging
Durante la scrittura su file, possono verificarsi problemi come:

File non trovato
Permessi insufficienti
Errore di accesso al disco
Per evitare crash del programma, sono stati implementati controlli specifici. Inoltre, se il file non può essere aperto, il programma notifica l’utente e continua comunque l’esecuzione.

Logging del ciclo clock
Ogni ciclo del clock viene registrato in un file dedicato (log_clock.txt) per tracciare eventuali problemi di sincronizzazione.vQuesto tipo di registro aiuta a monitorare la frequenza del clock e a identificare eventuali ritardi anomali.

Visualizzazione del log
Il programma include una funzione per visualizzare il contenuto del file di log direttamente a schermo.
Questa funzione permette di esaminare rapidamente le operazioni eseguite senza dover aprire manualmente il file.
La funzione di logging rappresenta uno strumento fondamentale per comprendere il comportamento del sistema simulato. Grazie alla combinazione di memorizzazione temporanea, registrazione su file e output grafico migliorato, il programma offre una tracciabilità completa di ogni operazione effettuata. Questo tipo di approccio non solo migliora l’affidabilità del simulatore, ma lo rende anche uno strumento efficace per scopi didattici e analisi avanzate del sistema hardware originale.

#### CAPITOLO 22 – Convertitore Binario-Decimale
La conversione tra i sistemi numerici binario e decimale rappresenta un’operazione fondamentale nel contesto del simulatore software del sistema logico-aritmetico sincronizzato a 32 bit. Essa consente di trasformare i valori immessi dall’utente (che spesso sono forniti in formato decimale) in dati binari utilizzabili dalle ALU, e viceversa, permettendo all’utente di interpretare facilmente i risultati prodotti dal sistema.

Obiettivo della funzione di conversione
L’obiettivo principale del convertitore è:

Convertire i valori decimali immessi dall’utente in formato binario per renderli compatibili con l’ALU
Trasformare i risultati binari ottenuti dopo le operazioni in formato decimale leggibile
Fornire un output chiaro e comprensibile
Semplificare l’inserimento dei dati da parte dell’utente
Il programma include due funzioni principali:

DEC_BIN_CODER(int dec): converte un numero decimale in una stringa binaria a 32 bit
BIN_DEC_CODER(char *bin): converte una stringa binaria in un numero decimale
Queste funzioni vengono utilizzate sia durante la fase di input che durante quella di output.

Funzione DEC_BIN_CODER: Decimale → Binario
Questa funzione riceve un numero intero positivo (compreso tra 0 e 4294967295, ovvero il massimo valore rappresentabile su 32 bit) e lo trasforma in una stringa binaria a 32 bit.

Questa funzione viene utilizzata ogni volta che l’utente immette operandi A o B in formato decimale, prima di distribuirli nei registri di input.

Funzione BIN_DEC_CODER: Binario → Decimale
Questa funzione riceve una stringa binaria (es. "01111011") e restituisce il suo equivalente in formato decimale.

Questa funzione è utile soprattutto nella visualizzazione del risultato finale delle operazioni eseguite dalle ALU, dove il risultato è in formato binario ma deve essere mostrato in decimale all’utente.

Integrazione con il menù principale
Il convertitore è integrato direttamente nel menù principale del programma, rendendolo accessibile come opzione dedicata.
Oltre alla semplice conversione, il programma integra il convertitore anche in altre funzioni chiave:

1. Inserimento manuale degli operandi
Quando l’utente inserisce operandi in formato decimale, essi vengono automaticamente convertiti in binario e distribuiti nei registri di input
2. Visualizzazione del risultato
Dopo l’esecuzione di un’operazione, il risultato binario memorizzato nei registri viene convertito in formato decimale e mostrato all’utente
3. Logging del risultato
I risultati vengono registrati in formato decimale e binario nei file di log per garantire maggiore completezza.
Validazione dei dati
Per evitare errori durante la conversione, il programma effettua diversi controlli sui dati immessi:

I numeri decimali devono essere compresi tra 0 e 4294967295
Le stringhe binarie devono contenere solo caratteri 0 o 1

Output grafico migliorato
Per rendere più leggibile il risultato, il programma utilizza una formattazione visiva avanzata.
Questa interfaccia aumenta la chiarezza e l’usabilità del programma.

Lettura e scrittura da file
Il convertitore supporta anche la lettura da file di configurazione (input_bin.txt, input_dec.txt) e la scrittura su file di output (output_bin.txt, output_dec.txt), aumentando la flessibilità d’uso.

Se il file non esiste, viene creato automaticamente con valori predefiniti.

Logging delle conversioni
Tutte le conversioni vengono registrate in un file di log (log_conversioni.txt) per successive analisi.
Questo tipo di registrazione è molto utile per tracciare eventuali anomalie o comportamenti errati.

Memoria temporanea per risultati intermedi
Il programma include una memoria temporanea dove vengono conservati i risultati delle conversioni, consentendo all’utente di effettuare ulteriori operazioni senza dover reinserire manualmente i dati.
La funzione stampa_memoria() permette di visualizzare tutti i risultati archiviati.
Questa funzione mostra sia il valore decimale che la sua rappresentazione binaria, facilitando l’analisi.

Gestione avanzata del ciclo clock
La funzione clock_step() simula il comportamento sincrono del sistema e viene utilizzata anche durante la conversione quando si desidera osservarne l’effetto.
Durante la conversione, il programma può mostrare progressivamente i bit calcolati al fronte positivo del clock.

Visualizzazione grafica del risultato
Il programma include una funzione per mostrare il risultato in una tabella ben formattata.
Questa interfaccia migliorata aiuta l’utente a comprendere immediatamente il risultato ottenuto.

Applicazioni pratiche del convertitore
Il convertitore binario-decimale ha diverse applicazioni nel progetto:

Immissione di operandi in formato decimale
Visualizzazione del risultato in entrambi i formati
Debugging e validazione dei dati
Registrazione completa del lavoro svolto
Utilizzo avanzato con memoria e file
Grazie a questa funzionalità, il programma diventa uno strumento completo per comprendere e testare la rappresentazione numerica in sistemi digitali.

Esempio di ciclo completo
Un tipico ciclo di conversione procede così:

L’utente seleziona l’opzione “Da decimale a binario”
Immette il valore 123
Il programma chiama DEC_BIN_CODER(123) e ottiene "01111011"
Mostra il risultato a schermo e lo registra in un file di log
Il valore viene salvato in memoria per uso successivo
Analogamente, il processo inverso avviene quando si converte un risultato binario in formato decimale.
La funzione di conversione tra binario e decimale rappresenta un elemento chiave nel simulatore software del sistema logico-aritmetico sincronizzato a 32 bit. Essa consente di tradurre i valori immessi dall’utente in un formato interpretabile dal sistema e di restituire il risultato in una forma leggibile. La combinazione di conversione precisa, logging avanzato, gestione della memoria e output grafico migliora notevolmente l’usabilità del programma, rendendolo uno strumento efficace sia per scopi didattici che per analisi tecniche.

#### CAPITOLO 23 – Simulazione del Clock e Sincronizzazione
La sincronizzazione rappresenta uno dei concetti fondamentali nell’architettura digitale. Essa garantisce che tutti i componenti del sistema operino in modo coordinato, evitando problemi come dati incompleti, ritardi irregolari o risultati inconsistenti. Nel simulatore software sviluppato in linguaggio C, la sincronizzazione è realizzata attraverso l’utilizzo di un segnale di clock simulato , che coordina i trasferimenti tra registri PIPO e le ALU 74181.

Obiettivo della sincronizzazione
L’obiettivo principale della simulazione del clock è:

Sincronizzare il trasferimento dei dati tra i registri e le ALU
Simulare il comportamento reale del segnale di clock nei circuiti digitali
Gestire correttamente il fronte positivo del clock , necessario per caricare nuovi dati
Fornire feedback visivo all’utente sullo stato del sistema
Il programma utilizza una variabile globale CLK per rappresentare lo stato del clock (0 o 1) e una variabile prev_CLK per registrare il valore precedente, utile per rilevare il fronte positivo (da 0 a 1), che attiva il trasferimento dei dati.

Funzione principale: clock_step()
La funzione principale per la gestione del clock si chiama clock_step() ed è definita come segue
void clock_step(int *CLK, int *prev_CLK, int milliseconds)
Dove:

CLK : puntatore al valore attuale del clock
prev_CLK : puntatore al valore precedente del clock
milliseconds : durata del ritardo prima dell’aggiornamento
Questa funzione genera un ritardo programmabile e aggiorna lo stato del clock, permettendo di controllare la velocità di esecuzione del sistema.

Ogni volta che viene chiamata questa funzione, il programma attende per il tempo indicato (espresso in millisecondi), memorizza il valore precedente del clock e lo inverte.

Simulazione del ciclo completo del clock
Per generare un ciclo completo del clock (da 0 a 1 e poi da 1 a 0), è necessario chiamare la funzione due volte.
Questo approccio consente di simulare il comportamento reale del sistema digitale sincronizzato, dove i dati vengono caricati solo al fronte positivo del clock.

Utilizzo del clock nei registri PIPO
I registri PIPO sono emulati tramite array di interi e vengono aggiornati solo quando il clock passa da 0 a 1. Questo garantisce una perfetta sincronizzazione tra le componenti del sistema.


All’interno della funzione reg_PIPO32(), il caricamento avviene solo se si verifica un fronte positivo.
Questa struttura garantisce che i dati vengano trasferiti solo quando effettivamente richiesto dal sistema.

Controllo avanzato del clock
In alcuni casi, può essere utile monitorare il clock e misurarlo in tempo reale. Per questo motivo, il programma include una funzione dedicata alla misurazione del ciclo clock.
Questa funzione simula 100 cicli di clock e calcola il tempo medio impiegato, utile per analisi di prestazioni o debugging avanzato.

Output grafico migliorato
Per rendere più visibile il cambiamento dello stato del clock, il programma visualizza un output grafico ogni volta che avviene un aggiornamento:

Questa interfaccia migliorata aiuta l’utente a comprendere il comportamento sincrono del sistema.

Configurazione del ritardo del clock
Il programma permette di configurare la durata del ciclo clock, fornendo flessibilità nella simulazione. L’utente può scegliere tra diverse opzioni:

Ciclo rapido (10–100 ms)
Ciclo normale (500 ms)
Ciclo lento (1000 ms)

Questo tipo di registrazione è molto utile per tracciare eventuali problemi di sincronizzazione.

Esempio di sincronizzazione completa
Un tipico ciclo di sincronizzazione nel simulatore procede così:

I dati vengono immessi dall’utente o letti da file
I bit vengono distribuiti nei registri di input
Viene invocata la funzione clock_step() per generare un fronte positivo
I dati vengono trasferiti alle ALU
Le ALU eseguono l’operazione selezionata
Il risultato viene accumulato in un array
Al secondo impulso di clock, il risultato viene inviato al registro di output
Il risultato finale viene mostrato a schermo e registrato in memoria
La gestione del clock rappresenta uno degli elementi chiave per garantire la correttezza del comportamento del sistema simulato. Grazie alla funzione clock_step(), al controllo avanzato del ritardo e alla registrazione dettagliata delle transizioni, il programma riesce a riprodurre fedelmente il comportamento sincrono di un sistema digitale basato su ALU 74181 e registri PIPO. Questa capacità non solo migliora l’accuratezza del simulatore, ma lo rende anche uno strumento didattico efficace per comprendere il ruolo del clock nei sistemi logici sincronizzati.

#### CAPITOLO 24 – Test Case e Validazione del Sistema
La validazione del sistema logico-aritmetico simulato è un passo fondamentale per garantire che l’implementazione software riproduca fedelmente il comportamento dell’hardware originale. A tal fine, sono stati definiti diversi test case rappresentativi, basati su operazioni aritmetiche e logiche comuni, utilizzando combinazioni note di operandi e segnali di controllo. Ogni test è stato eseguito sia in ambiente manuale che da file, e i risultati ottenuti sono stati confrontati con quelli attesi.

Obiettivo dei test case
L’obiettivo principale della fase di testing è:

Verificare la correttezza delle operazioni implementate
Controllare la propagazione del carry tra le ALU
Validare la sincronizzazione tramite clock
Testare il funzionamento completo del sistema a 32 bit
Confrontare i risultati con valori noti o calcolati manualmente
I test case sono stati progettati per coprire tutte le principali funzioni supportate dall’ALU 74181, inclusi AND, OR, XOR, NOT, somma, sottrazione, incremento e decremento, sia in modalità logica (M=1) che aritmetica (M=0), con e senza carry-in.

Struttura dei test
Ogni test case include:

I valori degli operandi A e B
I segnali di controllo S0–S3
Il valore del carry-in Cn
La modalità M (logica o aritmetica)
Il risultato previsto
Il risultato effettivamente ottenuto
Eventuali errori osservati
Il programma ha eseguito correttamente la somma su 32 bit, confermando la capacità del simulatore di emulare un sistema modulare complesso.

Implementazione dei test nel codice
Per rendere automatica la fase di testing, è stata creata una funzione dedicata esegui_test() che legge i parametri da un file (test_alu.txt) e confronta i risultati.


La funzione stampa_memoria() permette di visualizzare tutti i risultati archiviati:
Il programma utilizza una tabella ben formattata per mostrare i risultati del test.
Questa struttura aiuta l’utente a comprendere velocemente il risultato del test.

Funzione di debug avanzato
In caso di fallimento, il programma fornisce informazioni dettagliate sulle ALU coinvolte, i registri, il carry-out e i segnali di controllo, utile per il debugging.

Applicazioni pratiche dei test
I test case hanno diverse applicazioni:

Verifica del comportamento reale dell’ALU
Debugging del codice e individuazione di errori
Validazione della propagazione del carry
Controllo della sincronizzazione tra ALU e registri
Analisi prestazioni e accuratezza del simulatore
Essi costituiscono un elemento chiave per assicurare che il sistema software mantenga un comportamento conforme a quanto avverrebbe in un sistema hardware reale.
La definizione e l’esecuzione di test case rappresentano una parte essenziale nella realizzazione del simulatore. Essi consentono di verificare la correttezza delle operazioni aritmetiche e logiche, la gestione del carry e la sincronizzazione tra ALU e registri PIPO. Grazie a una struttura modulare e a una registrazione completa dei risultati, il programma offre una valida base per l’apprendimento e la validazione avanzata del sistema digitale simulato. L’utilizzo di test automatizzati e memorizzazione dei risultati migliora ulteriormente la qualità del progetto e ne garantisce affidabilità e precisione.

#### CAPITOLO 25 – Criticità e Limiti del Simulatore
Sebbene il simulatore software del sistema logico-aritmetico sincronizzato a 32 bit basato su ALU 74181 e registri PIPO rappresenti uno strumento efficace per comprendere il comportamento reale dei componenti digitali, esso presenta alcune criticità e limitazioni che derivano sia dalla natura stessa della simulazione software che dalle caratteristiche dell’hardware originale.

Obiettivo dell’analisi critica
L’obiettivo principale di questo capitolo è fornire una panoramica dettagliata delle seguenti aree:

Limiti tecnici e prestazionali
Problemi di accuratezza nella simulazione
Vincoli legati alla precisione del modello hardware
Complessità di implementazione software
Eventuali errori o discrepanze tra simulazione e realtà
Questo tipo di analisi consente di individuare le aree di miglioramento e di valutare l’applicabilità del simulatore in contesti avanzati o reali.

1. Limiti di precisione nella simulazione dell’ALU
La funzione n_ALU74181() riproduce fedelmente la truth table dell’unità ALU 74181 utilizzando porte logiche fondamentali del linguaggio C. Tuttavia, poiché non si tratta di un emulatore a livello gate ma di una simulazione logica astratta, possono verificarsi piccole discrepanze rispetto al comportamento reale del chip fisico, soprattutto in termini di timing e ritardi di propagazione.

Ad esempio, il calcolo del carry-out avviene in modo deterministico e istantaneo nel codice, mentre in un sistema hardware reale potrebbe dipendere da fattori come:

Ritardo di gate
Tempo di setup e hold
Condizioni di temporizzazione del clock
Questi aspetti non sono stati completamente replicati nel simulatore, rendendo impossibile testare situazioni di clock skew , setup violation o altre problematiche hardware reali.

2. Gestione del clock e sincronizzazione
Il programma simula il segnale di clock attraverso una funzione clock_step() che introduce un ritardo programmabile e inverte lo stato del clock. Sebbene questa implementazione permetta di riprodurre il comportamento sincrono dei registri PIPO, essa rimane pur sempre una semplificazione software.

Criticità:

Il ciclo del clock è rigidamente sequenziale e non parallelo
Non è possibile simulare glitch o impulsi anomali del clock
I registri vengono aggiornati tutti nello stesso istante , senza considerare eventuali differenze di tempo di arrivo del segnale
Un miglioramento futuro potrebbe includere una gestione del clock distribuita e un sistema di logging avanzato per analizzare i tempi di trasferimento.

3. Complessità nell’implementazione logica interna
La struttura interna dell’ALU 74181 è estremamente complessa, con una combinazione di multiplexer, full adder e reti combinatorie. Nel simulatore, questa complessità è stata tradotta in una serie di operazioni logiche annidate, implementate tramite funzioni come porta_and_3(), porta_or_4() e porta_exor_5().

Tuttavia, questa implementazione può risultare difficile da leggere, debuggare e mantenere nel tempo, specialmente quando si deve effettuare una modifica o correggere un errore.

Questo tipo di formula, se mal implementato, può portare a risultati errati o difficili da interpretare.

4. Vincoli sull’input utente
Il simulatore richiede che ogni bit immesso dall’utente sia esclusivamente 0 o 1 , così come i segnali di controllo S0–S3 , M e Cn . Sebbene il programma includa controlli rigorosi, l’inserimento manuale di dati binari a 4 o 32 bit può essere soggetto a errori umani .

Inoltre, il processo di conversione decimale → binario e viceversa è limitato ai soli numeri interi positivi (da 0 a 4294967295). Non è prevista la gestione di operandi negativi in formato complemento a due se non indirettamente tramite operazioni aritmetiche come la sottrazione.

5. Overflow e underflow durante le operazioni
Nonostante il programma abbia controlli sul valore massimo inseribile, le operazioni aritmetiche possono comunque causare overflow o underflow , specialmente nelle somme o sottrazioni che coinvolgono operandi molto grandi o con carry-in attivo.

Senza una gestione avanzata del wrapping o della saturazione, il risultato potrebbe non riflettere fedelmente il comportamento dell’hardware reale.

6. Velocità di esecuzione
Poiché il simulatore è realizzato in linguaggio C e gira in ambiente software, il tempo di esecuzione di operazioni su 32 bit non è paragonabile a quello di un sistema hardware reale. Ogni ciclo di clock è accompagnato da un ritardo artificiale introdotto tramite delay(), necessario per visualizzare il comportamento sincrono, ma che rallenta notevolmente il sistema.

In un contesto didattico, questa lentezza è accettabile e anzi auspicabile per osservare passo-passo il funzionamento del sistema. In un contesto pratico o di testing automatizzato, invece, sarebbe preferibile una versione più veloce, magari priva del delay visivo.

7. Memoria dinamica e scalabilità
La memoria temporanea del programma è stata implementata con realloc() per supportare memorizzazione dinamica dei risultati. Tuttavia, in alcuni casi, la gestione della memoria può diventare inefficiente, specialmente in presenza di molti risultati archiviati o in operazioni ripetute.

Un miglioramento possibile è l’utilizzo di strutture dati più avanzate, come liste collegate o buffer circolari, per ottimizzare l’uso della memoria.

8. Logging e output grafico migliorato
Sebbene il programma utilizzi un output grafico migliorato per mostrare gli errori o i risultati, la formattazione ASCII può non essere compatibile con tutti i terminali o sistemi operativi. Alcuni ambienti non supportano i caratteri Unicode o i box drawing characters, causando problemi di visualizzazione.

Per ovviare a questo limite, si potrebbe prevedere un'opzione di output semplice e testuale, disattivabile all’avvio del programma.

9. File system e accesso ai dati
Il simulatore supporta la lettura e scrittura da file (input_alu.txt, risultati_alu32.txt, ecc.), ma in alcuni casi può verificarsi:

File non trovato
Formato del file errato
Accesso al file bloccato da altro processo
Scrittura non consentita
Questi problemi, pur non compromettendo il core del sistema, possono ostacolare l’uso avanzato del programma, specialmente in ambienti multiutente o server.

Tuttavia, non è prevista una gestione avanzata degli errori di permessi o accesso.

10. Precisione del convertitore binario-decimale
Le funzioni DEC_BIN_CODER() e BIN_DEC_CODER() forniscono una buona approssimazione della conversione tra i due sistemi numerici, ma presentano alcune limitazioni:

Non supportano numeri negativi
Non gestiscono correttamente numeri con virgola
La lunghezza fissa a 32 bit può generare ambiguità
Un miglioramento possibile sarebbe l’integrazione di una libreria dedicata o la gestione avanzata del formato IEEE 754 per supportare anche operazioni in virgola mobile.

11. Scalabilità del sistema
Il sistema descritto opera su parole a 32 bit, ma non è facilmente espandibile a 64 bit o oltre senza modificare manualmente il codice. L’espansione richiede cambiamenti strutturali nei cicli di elaborazione, nella definizione delle ALU e nella gestione del carry-out.

Una futura evoluzione del progetto potrebbe prevedere una funzione parametrizzata che permette di impostare la dimensione desiderata a runtime:

12. Compatibilità cross-platform
Il simulatore è stato sviluppato in linguaggio C e testato principalmente su sistemi Unix-like (Linux e macOS), dove la funzione clock() e i ritardi in millisecondi funzionano correttamente. Su Windows, tuttavia, il comportamento del clock potrebbe variare, causando discrepanze nella durata dei cicli.

Un approccio migliore sarebbe l’utilizzo di librerie standardizzate come std::chrono in C++ o time.h con configurazioni più precise per garantire uniformità tra i sistemi operativi.

13. Manca l’interfaccia grafica
Il programma attualmente offre solo un’interfaccia a terminale, che, pur essendo funzionale, può risultare poco intuitiva per utenti meno esperti. Una futura evoluzione del progetto potrebbe includere un’interfaccia grafica (ad esempio con GTK, SDL o Qt) per visualizzare:

Lo stato del clock
I dati nei registri
Il flusso delle ALU
La propagazione del carry
Le operazioni selezionate
Questo tipo di interfaccia aumenterebbe notevolmente l’usabilità del simulatore, specialmente in ambito educativo.

14. Difficoltà nell’esecuzione automatica
Sebbene il programma supporti l’esecuzione da file di configurazione, non include una modalità batch completa o scriptable. Questo rende difficoltoso l’utilizzo avanzato del simulatore per integrazioni automatizzate o test ripetuti.

Una possibile soluzione sarebbe l’aggiunta di una funzione --batch che legge un file e genera un output strutturato (es. JSON o CSV), utile per l’analisi automatica.

15. Assenza di supporto per operazioni condizionali
Attualmente, il simulatore non prevede alcun supporto per operazioni condizionali (es. jump, branch, if-then-else), né per la gestione di flag come Z (zero), N (negativo) o V (overflow). Queste informazioni sono spesso utili in contesti avanzati come la simulazione di CPU complete.

Questa funzionalità aprirebbe la strada a simulazioni di architetture CPU più avanzate.

16. Precisione del lookahead carry generator
Nell’ALU reale, il sistema lookahead carry calcola anticipatamente i segnali P (Propagate) e G (Generate) per velocizzare la somma multi-bit. Nel simulatore, questa logica è stata riprodotta fedelmente, ma il calcolo avviene in modo seriale e non parallelo.

*Cn_piu_4 = G + (P * Cn);
Anche se preciso, questo approccio è computazionalmente costoso e non riesce a replicare il vero parallelismo del sistema hardware.

17. Utilizzo avanzato della memoria
La memoria temporanea del programma, pur essendo dinamica, non supporta ancora la possibilità di sovrascrivere dati specifici o di cancellarne alcuni. Un’estensione futura potrebbe includere:

Funzioni di edit in memoria
Supporto per salvataggio permanente
Compressione e pulizia della memoria
Tutto ciò aumenterebbe la versatilità del sistema, rendendolo utile anche in contesti di simulazione avanzata.

18. Limiti di usabilità
L’interfaccia a terminale, pur essendo sufficiente per scopi didattici, presenta alcuni limiti:

Non permette editing in tempo reale
Non supporta la correzione di dati dopo l’inserimento
Richiede reinserimento completo in caso di errore
Un miglioramento consisterebbe nell’aggiungere una funzione di “modifica ultima riga” o un sistema di history che consente di recuperare i dati immessi.

19. Test e validazione limitata
I test case definiti nel programma coprono molte delle operazioni principali dell’ALU, ma non sono esaustivi. Alcune operazioni complesse come il complemento a due o il confronto tra operandi non sono state testate in profondità.

Un piano di testing più avanzato potrebbe includere:

Script di test automatizzati
Input randomizzati entro un range
Confronto con simulatori hardware
20. Differenze tra simulazione e realtà
Diversi sono i punti in cui la simulazione si discosta dal comportamento reale:

Velocità di esecuzione : software vs hardware
Tempo di propagazione : assente nel simulatore
Gestione dell’alimentazione : non presente
Effetti collaterali del clock : non simulati
Stabilità del segnale : non considerata
Questi aspetti, pur non essenziali per il funzionamento base del simulatore, rappresentano criticità importanti per chi volesse usarlo in contesti di ricerca o progettazione hardware avanzata.
Il simulatore software del sistema logico-aritmetico a 32 bit basato su ALU 74181 e registri PIPO 74198 rappresenta uno strumento efficace per comprendere il funzionamento di un sistema digitale sincrono. Tuttavia, presenta alcune criticità e limiti legati alla natura software, alle scelte di progettazione e alla complessità del modello hardware originale.

Tra le principali criticità emergono:

Limiti nella precisione del modello ALU
Simulazione del clock non completamente conforme
Difficoltà nell’emulare il parallelismo hardware
Problemi di usabilità e compatibilità cross-platform
Questi punti offrono ampio margine per ulteriori sviluppi e ottimizzazioni future, che potrebbero includere interfacce grafiche, simulazioni a livello gate e test più avanzati.

#### CAPITOLO 26 – Applicazioni Didattiche e Valore Formativo
Il simulatore software del sistema logico-aritmetico sincronizzato a 32 bit, basato su ALU 74181 e registri PIPO, rappresenta uno strumento didattico estremamente valido per l’apprendimento delle architetture hardware digitali. Esso permette di esplorare i fondamenti della logica digitale, il funzionamento delle unità ALU, la gestione dei segnali di controllo e la sincronizzazione tra componenti in un ambiente accessibile e controllabile.

Obiettivi formativi del progetto
L’obiettivo principale del progetto è fornire agli studenti:

Una comprensione pratica del funzionamento delle ALU
Un approccio concreto alla logica combinatoria e sequenziale
La capacità di implementare concetti hardware in ambiente software
L’esperienza nella simulazione di sistemi sincronizzati
Competenze avanzate nella programmazione in C e nell’emulazione digitale
Questo tipo di simulazione consente di studiare le operazioni aritmetiche e logiche fondamentali utilizzate in ogni processore moderno, ma con l’approccio semplificato e modulare che rende il tutto più accessibile.

Simulazione come strumento educativo
La simulazione software offre diversi vantaggi rispetto all’utilizzo diretto di hardware reale:

Accessibilità : non richiede componenti fisici o laboratori attrezzati
Ripetibilità : permette di effettuare test multipli senza danneggiare il sistema
Visualizzazione chiara : mostra passo dopo passo il comportamento del sistema
Debugging avanzato : permette di tracciare errori e comprendere il flusso logico
Flessibilità : consente di modificare facilmente operandi, segnali e configurazioni
Grazie a queste caratteristiche, il programma diventa uno strumento ideale per laboratori universitari, corsi di architettura dei calcolatori e attività di apprendimento autonomo.

Studio dell'ALU 74181
L’unità ALU 74181 è uno dei primi esempi di unità aritmetico-logica realizzata come singolo chip TTL. Essa è capace di eseguire ben 48 operazioni distinte , selezionabili tramite quattro segnali di controllo (S0–S3), un segnale di modalità (M) e un riporto in ingresso (Cn).

Nel simulatore, l’ALU viene riprodotta fedelmente attraverso funzioni logiche fondamentali del linguaggio C:

AND (porta_and)
OR (porta_or)
NOT (porta_not)
EXOR (porta_exor)
Ogni operazione è replicata seguendo la truth table originale, permettendo allo studente di vedere direttamente come si traduce in codice il comportamento dell’hardware.

Queste funzioni sono poi utilizzate per costruire operazioni più complesse come porta_and_3() o porta_or_5(), necessarie per emulare correttamente la struttura interna dell’ALU.

Analisi dettagliata del carry
Uno degli aspetti chiave insegnati attraverso il simulatore è la gestione del carry durante le operazioni aritmetiche. Il sistema supporta sia somme semplici che con carry-in, e consente di collegare otto ALU in cascata per formare un sistema a 32 bit.

I segnali P (Propagate Carry) e G (Generate Carry) vengono calcolati e visualizzati, mostrando come avviene la propagazione anticipata del riporto grazie al sistema lookahead carry generator .

Questa formula, derivata direttamente dall’hardware originale, permette di accelerare la somma multi-bit, riducendo il ritardo tipico della propagazione seriale.

Utilizzo dei registri PIPO
I registri PIPO sono anch’essi parte integrante del progetto didattico. Essi consentono di caricare e scaricare dati in parallelo, sincronizzati da un segnale di clock. Questo tipo di registro è essenziale per comprendere il ruolo della temporizzazione nel trasferimento dei dati.

All’interno del simulatore, i registri vengono emulati tramite array di interi e una variabile CLK che simula il segnale di clock. Ogni volta che il clock passa da 0 a 1, i dati vengono trasferiti:

Questo meccanismo aiuta gli studenti a comprendere il concetto di fronte positivo , sincronizzazione e trasferimento parallelo .

Architettura modulare e scalabilità
L’utilizzo di otto ALU collegate in cascata per formare un sistema a 32 bit rappresenta un esempio pratico di come si possa espandere un componente elementare fino a creare un sistema complesso.

Ogni ALU elabora 4 bit e tutti condividono lo stesso insieme di segnali di controllo. Il carry-out di un’ALU vifene collegato al carry-in della successiva, garantendo una corretta propagazione del riporto lungo tutta la catena.

Questa struttura modulare introduce il concetto di scalabilità , fondamentale nei sistemi informatici reali.

Interfaccia utente e menù interattivo
Il programma include un menù interattivo dove l’utente può scegliere tra diverse opzioni operative
**MENU PRINCIPALE**
[1] Inserisci operandi manualmente
[2] Leggi da file di input
[3] Visualizza risultati
[4] Salva risultati su file
[5] Convertitore Binario-Decimale
[6] Simula ALU 32 bit
[7] Ripeti ultima operazione
[8] Stampa memoria temporanea
[9] Misura ciclo clock
[0] Esci
Selezionando un’opzione, l’utente viene guidato nell’inserimento dei dati, offrendo un’esperienza pratica e coinvolgente.

Conversione binario-decimale
Una delle funzionalità centrali del simulatore è la conversione tra numeri binari e decimali, necessaria per permettere all’utente di lavorare con valori familiari e contemporaneamente comprendere la rappresentazione a livello di bit.

Le due funzioni principali sono:

DEC_BIN_CODER(int dec) – converte un numero decimale in stringa binaria
BIN_DEC_CODER(char *bin) – converte una stringa binaria in numero decimale

Questo consente di effettuare operazioni concatenate e di analizzare i risultati storici.

Esempio di utilizzo in classe
Il simulatore può essere utilizzato in un contesto accademico come parte di un laboratorio di architettura dei calcolatori. Ad esempio, un docente potrebbe chiedere agli studenti di:

Implementare una nuova ALU
Estendere il sistema a 64 bit
Aggiungere nuove operazioni
Ottimizzare la gestione della memoria
Realizzare un’interfaccia grafica
Gli utenti devono verificare che il programma restituisca correttamente il risultato e comprendere il ruolo del carry-in.

Approccio modulare al codice
Il programma è stato sviluppato in modo modulare , con funzioni dedicate a porte logiche, ALU, registri, logging e interfaccia utente. Questo approccio facilita la comprensione del codice e permette di isolare eventuali bug o aree di miglioramento.

Struttura del progetto:

funzioni_logiche.c: operatori AND, OR, NOT, XOR
alu_74181.c: simulazione dell’ALU
registri_pipo.c: emulazione dei registri PIPO
main.c: coordinazione generale del sistema
utils.c: convertitori, memoria e logging
Ogni modulo può essere studiato separatamente e adattato a nuove esigenze.

Test automatizzati e validazione
Il simulatore include un sistema di test automatizzati che legge da file (test_alu.txt) e confronta i risultati ottenuti con quelli attesi. Questo consente di valutare la correttezza del codice e di introdurre il concetto di testing automatico .

Questa funzionalità aiuta gli studenti a familiarizzare con il concetto di testing e debugging.

Introduzione alle operazioni condizionate
Il programma include anche funzioni base per la gestione di operazioni condizionate, come somma, sottrazione, incremento e decremento. Queste operazioni introducono i fondamenti della programmazione a basso livello, dove il risultato dipende dai segnali di controllo e dallo stato del sistema.


Questo tipo di struttura introduce il concetto di branching e di controllo del flusso .

Supporto per l’uso avanzato
Per gli studenti più avanzati, il simulatore può essere esteso per includere:

Interfaccia grafica
Supporto per virgola mobile
Simulazione di CPU complete
Integrazione con altre componenti (RAM, I/O, ecc.)
Tutto ciò rende il progetto flessibile e adatto a essere utilizzato in corsi di livello crescente.

Criticità e limiti didattici
Nonostante i numerosi vantaggi, il simulatore presenta alcune criticità da un punto di vista educativo:

Non replica esattamente il comportamento temporale delle ALU fisiche
Non tiene conto di fattori reali come il clock skew o i tempi di setup/hold
Non include il concetto di pipeline o stall in contesti avanzati
Tuttavia, questi limiti possono essere discussi in classe come opportunità di approfondimento.

Applicazioni pratiche
Il simulatore ha diverse applicazioni concrete in ambito didattico:

Laboratorio di architettura dei calcolatori
Corso introduttivo alla logica digitale
Progetti di gruppo su sistemi sincroni
Attività di reverse engineering di circuiti TTL
Dimostrazioni live di operazioni ALU
Inoltre, il programma può essere utilizzato per:

Verificare la correttezza delle operazioni
Capire la differenza tra logica combinazionale e sequenziale
Studiare il comportamento del carry e del lookahead carry generator
Approfondire il funzionamento dei registri sincroni
Il simulatore software del sistema logico-aritmetico sincronizzato a 32 bit costituisce uno strumento educativo efficace per comprendere i fondamenti dell’architettura digitale. Grazie alla sua struttura modulare, alla possibilità di inserire dati manualmente o da file, alla registrazione completa dei risultati e alla simulazione precisa del comportamento dell’ALU e dei registri, esso offre una base solida per l’apprendimento di concetti avanzati come la sincronizzazione, la propagazione del carry e l’esecuzione parallela delle operazioni.

L’utilizzo del linguaggio C rende il progetto accessibile a studenti di informatica, ingegneria elettronica e scienze dei materiali. Inoltre, la natura open-source del codice permette di modificarlo, migliorarlo e personalizzarlo in base alle esigenze didattiche specifiche.

Questo tipo di simulazione non solo migliora la comprensione teorica dei sistemi digitali, ma fornisce un’esperienza pratica che prepara gli studenti alla progettazione hardware/software futura.


#### CAPITOLO 27 – Sicurezza del Codice
Nel contesto dello sviluppo software, la sicurezza del codice rappresenta uno dei punti chiave per garantire che il programma funzioni correttamente, eviti comportamenti anomali e non esponga l’utente a rischi legati alla gestione errata della memoria o al parsing di input malevoli. Nel simulatore del sistema logico-aritmetico sincronizzato a 32 bit basato su ALU 74181 e registri PIPO, sono state implementate diverse strategie per migliorare la robustezza del codice e prevenire errori comuni durante l’esecuzione.

Obiettivi di sicurezza
L’obiettivo principale della gestione della sicurezza nel simulatore è:

Prevenire buffer overflow e vulnerabilità nella lettura da input
Evitare accessi illegali alla memoria
Gestire correttamente l’allocazione dinamica
Proteggere il sistema da valori errati o malevoli
Fornire feedback chiaro all’utente in caso di errore
Sebbene il programma sia orientato alla didattica e non richieda livelli di sicurezza estremamente avanzati come quelli utilizzati in applicazioni critiche o commerciali, alcune pratiche fondamentali sono state integrate per rendere il codice più solido e affidabile.

Controllo rigoroso dell’input utente
Il simulatore richiede frequentemente dati binari (0 o 1) e segnali di controllo validi (S0–S3, M, Cn). Per evitare problemi causati da inserimenti errati, ogni valore immesso dall’utente viene immediatamente validato prima di essere utilizzato.

Questo tipo di validazione garantisce che solo valori conformi vengano passati alle ALU, prevenendo risultati errati o crash improvvisi.

Gestione avanzata del parsing da file
Il programma supporta la lettura di operandi e configurazioni da file (input_alu.txt, input_alu32.txt). Tuttavia, se il file non esiste o è malformato, il programma lo crea automaticamente con valori predefiniti e notifica l’utente.

Inoltre, il parsing dei dati letti dal file include controlli completi sul formato.
Questi controlli impediscono errori gravi causati da dati fuori formato o manomessi.

Prevenzione del buffer overflow
Per evitare problemi di buffer overflow durante l’inserimento manuale di dati, il programma utilizza funzioni sicure per la lettura degli input, come fgets() invece di gets(), e limita la dimensione massima delle stringhe.

Questa pratica riduce drasticamente i rischi legati alla manipolazione di dati malevoli.

Validazione completa dei dati binari
I dati binari, sia immessi manualmente che letti da file, vengono sempre verificati per assicurarsi che siano composti solo da 0 e 1. Questo avviene tramite cicli di controllo.
Tale controllo previene eventuali malfunzionamenti dovuti a dati non validi.

Protezione dalla divisione per zero
Alcune operazioni aritmetiche coinvolgono la divisione, che può generare errori runtime se il denominatore è zero. Per evitarlo, il programma effettua controlli preventivi.
Questa protezione evita crash improvvisi e consente all’utente di comprendere dove si trova il problema.

Gestione sicura della memoria dinamica
La memoria temporanea utilizzata per conservare i risultati intermedi è stata implementata con attenzione per evitare perdite di memoria (memory leak) o accessi illegittimi.

Ogni volta che la memoria viene espansa, il programma verifica che l’operazione abbia successo e, in caso negativo, libera la memoria esistente per evitare perdite.

Logging e tracciabilità degli errori
Ogni errore rilevato durante l’esecuzione viene registrato in un file di log (log_errore.txt).

Questa interfaccia aumenta la leggibilità e semplifica l’interpretazione da parte dell’utente.

Verifica dei limiti di input
Il programma effettua diversi controlli sui dati immessi:

I bit devono essere 0 o 1
Gli operandi devono appartenere al range consentito (0–4294967295)
I segnali di controllo S0–S3, M e Cn devono rispettare i vincoli hardware
In caso di errore, l’utente riceve un feedback immediato e l’esecuzione viene bloccata.

Evitare problemi di accesso ai file
Durante la lettura e scrittura da file, possono verificarsi problemi come:

File non trovato
Accesso negato
File danneggiato
Permessi insufficienti
Questo tipo di registrazione è molto utile per monitorare la frequenza del clock e identificare eventuali ritardi anomali.

Esempio di ciclo completo
Un tipico ciclo di esecuzione procede così:

L’utente immette i valori degli operandi A e B
I dati vengono caricati nei registri di input
Al fronte positivo del clock, i dati vengono inviati alle ALU
Le ALU eseguono l’operazione selezionata
Il risultato viene accumulato in memoria
Al nuovo impulso di clock, il risultato viene memorizzato nel registro di output
Il risultato finale viene visualizzato e registrato in un file
Ogni passo include controlli precisi per evitare errori.
La sicurezza del codice gioca un ruolo fondamentale nella stabilità e nell’affidabilità del simulatore. Grazie a controlli rigorosi sugli input, alla gestione avanzata dei file, al logging degli errori e alla protezione dalle operazioni matematiche critiche, il programma offre un ambiente sicuro e robusto per l’apprendimento e l’analisi del sistema digitale originale.

La combinazione di validazione precisa, feedback visivo e registrazione completa degli errori rende il simulatore uno strumento efficace sia per scopi didattici che per futuri sviluppi professionali.

#### CAPITOLO 28 – Portabilità del Programma
La portabilità rappresenta una caratteristica fondamentale per qualsiasi applicativo software, poiché determina la capacità del programma di funzionare correttamente su diverse piattaforme hardware e sistemi operativi senza modifiche sostanziali al codice. Il simulatore del sistema logico-aritmetico sincronizzato a 32 bit basato su ALU 74181 e registri PIPO è stato sviluppato in linguaggio C, una scelta che favorisce la portabilità grazie alla sua ampia diffusione e alla presenza di compilatori disponibili su quasi tutte le piattaforme moderne.

Obiettivo della portabilità
L’obiettivo principale della progettazione del programma è stato realizzare un’applicazione:

Compatibile con diversi sistemi operativi : Windows, Linux, macOS
Indipendente dall’architettura hardware
Facilmente comprensibile e modificabile
Compilabile con standard comuni (C89, C99, C11)
Questa flessibilità rende il simulatore uno strumento utile non solo per l’apprendimento, ma anche per la sperimentazione avanzata in contesti accademici e professionali.

Linguaggio C e compilazione cross-platform
Il programma è stato scritto esclusivamente in linguaggio C ANSI standard , evitando l’utilizzo di librerie o funzioni specifiche di un singolo sistema operativo. Questo consente di utilizzare lo stesso codice su ogni piattaforma, purché si utilizzi un compilatore conforme allo standard C.

Differenze nei ritardi temporali
Il simulatore include una funzione delay(int milliseconds) per introdurre pause programmabili tra i cicli del clock, replicando fedelmente il comportamento sincrono del sistema digitale.

Senza queste considerazioni, il programma potrebbe non funzionare correttamente su sistemi diversi da quello di origine.

Compatibilità dei file system
Il simulatore supporta la lettura e la scrittura di operandi e risultati da file (input_alu.txt, risultati_alu32.txt, ecc.). Tuttavia, il comportamento relativo ai file può variare leggermente tra i sistemi operativi, specialmente per quanto riguarda i percorsi assoluti/relativi, i permessi di accesso e il formato di fine riga (\n vs \r\n).

Per ovviare a questo limite, il programma utilizza sempre path relativi e gestisce eventuali errori di apertura con messaggi chiari.
Inoltre, il programma ignora eventuali discrepanze nel formato delle nuove linee durante la lettura da file.

Gestione della memoria dinamica
La memoria temporanea utilizzata per conservare i risultati intermedi è stata implementata con malloc() e realloc(), funzioni parte dello standard C ANSI e quindi disponibili su tutte le piattaforme.
Tuttavia, alcune implementazioni di realloc() possono comportarsi in modo leggermente diverso su sistemi diversi, per cui è necessario testare il programma su più ambienti prima del rilascio.

Output grafico migliorato e compatibilità terminale
Il simulatore utilizza una formattazione visiva avanzata basata su caratteri Unicode e box drawing characters per mostrare risultati, errori e menu interattivi.
Tuttavia, questa formattazione non è universalmente supportata. Alcuni terminali Windows o shell legacy non visualizzano correttamente i caratteri Unicode, causando problemi di leggibilità.

Supporto per sistemi embedded e minimali
Il simulatore è stato pensato principalmente per sistemi desktop, ma potrebbe essere adattato a dispositivi embedded o sistemi minimali come Raspberry Pi o microcontrollori ARM, a patto di:

Rimuovere eventuali dipendenze grafiche
Ridurre l’uso della memoria dinamica
Utilizzare librerie lightweight

Differenze tra sistemi operativi
Alcune funzioni sono implementate in modo differente a seconda del sistema operativo. Ad esempio:

La funzione sleep() è disponibile in Windows come Sleep(1000) (con maiuscola), mentre in Unix è sleep(1)
I caratteri Unicode richiedono impostazioni specifiche del terminale in Windows
L’accesso ai file di sistema come /proc/cpuinfo è disponibile solo su Linux
Compatibilità tra compilatori
Il codice è stato testato con successo sui seguenti compilatori:

GCC (GNU Compiler Collection)
Clang (su macOS e Linux)
MSVC (Microsoft Visual Studio C)
MinGW (Minimalist GNU for Windows)
Sebbene il programma mantenga un comportamento coerente, alcune ottimizzazioni o estensioni non standard (come _Bool in MSVC) richiedono adattamenti per garantire la massima compatibilità.

Testing automatizzato e continuous integration
Una futura estensione del progetto potrebbe includere un sistema di testing automatizzato e integrazione continua, utilizzando strumenti come:

GitHub Actions
Travis CI
Docker
Questi strumenti permetterebbero di verificare automaticamente il comportamento del programma su diverse architetture e configurazioni, aumentando il livello di affidabilità del codice.
Questo tipo di automazione garantisce che il programma venga testato regolarmente su tutti i sistemi.

Uso di librerie standard
Il simulatore utilizza esclusivamente librerie del linguaggio C standard:

<stdio.h> – Input/output
<stdlib.h> – Allocazione dinamica
<string.h> – Manipolazione stringhe
<time.h> – Timing e logging
<ctype.h> – Controllo dei caratteri
L’assenza di librerie esterne ne aumenta notevolmente la portabilità e riduce le dipendenze esterne.

La portabilità del programma è stata una priorità fin dalla fase iniziale di progettazione. Grazie all’utilizzo del linguaggio C standard, alle direttive precompilatore e alla gestione cross-platform di funzioni come il clock e la grafica ASCII, il simulatore riesce a girare correttamente su diverse piattaforme, inclusi Windows, Linux e macOS .

Nonostante alcune piccole discrepanze nell’output grafico e nella gestione del tempo, il programma mantiene una logica uniforme e una struttura modulare che facilita l’adattamento a nuovi ambienti. Inoltre, l’assenza di librerie esterne e l’uso controllato della memoria dinamica contribuiscono a una maggiore stabilità del codice.

Con opportuni aggiustamenti e l’implementazione di opzioni come --no-graphic o --batch, il programma può essere reso ancora più versatile, adatto non solo a scopi didattici ma anche a integrazioni automatizzate, testing cross-sistema e utilizzo su dispositivi embedded.

#### CAPITOLO 29 – Configurazione Iniziale da File
La possibilità di caricare i parametri del sistema logico-aritmetico sincronizzato a 32 bit da un file di configurazione rappresenta una funzionalità avanzata del simulatore software. Questo tipo di approccio permette all’utente di preparare un set di dati predefiniti per gli operandi A e B, i segnali di controllo S0–S3, la modalità M e il riporto iniziale Cn, e di utilizzarli ripetutamente senza dover reinserire manualmente ogni valore.

Obiettivo della configurazione da file
L’obiettivo principale della funzione di lettura da file è:

Automatizzare l’inserimento dei dati
Fornire una base stabile per test ripetuti
Rendere il programma più flessibile e scriptabile
Permettere la condivisione delle configurazioni tra utenti diversi
Il programma supporta sia la lettura da file singoli che multipli, e include controlli rigorosi sul formato e sui valori contenuti, garantendo che solo dati conformi vengano utilizzati.

Se il file non esiste o non è completo, viene creato automaticamente con valori predefiniti:

Questo garantisce che l’utente abbia sempre un punto di partenza definito.

Parsing del file di configurazione
Dopo aver aperto il file, il programma effettua il parsing delle informazioni utilizzando sscanf() e controlla attentamente il formato.
In questo modo, il programma estrae solo i valori numerici all’interno delle parentesi e li assegna alle variabili corrette.

Gestione avanzata degli errori durante la lettura
Durante la lettura del file, possono verificarsi diversi tipi di errore:

File non trovato
Formato errato
Valori fuori range
Campi mancanti
Caratteri non validi
Per gestire queste situazioni, il programma implementa una serie di controlli preventivi:
Se una riga non è presente o non può essere letta, il programma emette un messaggio d’errore e termina l’esecuzione per evitare risultati inconsistenti.

Validazione dei dati letti
Dopo la lettura dei dati dal file, vengono effettuati ulteriori controlli sulla correttezza dei valori:

I bit devono essere 0 o 1
Gli operandi devono appartenere al range consentito (0–4294967295)
I segnali di controllo S0–S3, M e Cn devono rispettare i vincoli hardware
Anche se i dati provengono da file, vengono comunque validati prima dell’utilizzo.

Esempio di lettura completa
Un esempio completo di lettura da file procede così:


Questa registrazione consente di tracciare eventuali problemi nella configurazione e di riprodurre facilmente i test.

Utilizzo avanzato del file di configurazione
Il file di input può essere utilizzato anche in contesti avanzati come:

Testing automatizzato
Debugging di errori ricorrenti
Simulazione batch di operazioni multiple
Confronto tra diverse configurazioni
Un miglioramento futuro potrebbe includere l’utilizzo di un formato strutturato come JSON o XML per rendere il file più leggibile da altri programmi.
Output grafico migliorato
Per rendere immediatamente visibili i dati letti, il programma utilizza una formattazione grafica avanzata.
Questa interfaccia aumenta la chiarezza e semplifica l’interpretazione da parte dell’utente.

Simulazione multipla con stesso input
La lettura da file permette di eseguire operazioni multiple con lo stesso input, facilitando il confronto tra risultati ottenuti in condizioni diverse.
Questo tipo di ciclo è molto utile per test ripetuti o per generare output comparativi.

Memoria temporanea e logging avanzato
I risultati vengono conservati in una memoria dinamica e registrati in un file (risultati_alu32.txt), insieme ai valori di input utilizzati.
}
Questa struttura consente di effettuare operazioni concatenate senza dover reinserire manualmente i dati.

Questa funzione mostra tutti i risultati archiviati, inclusi quelli derivati da file di input.
La capacità di caricare la configurazione iniziale da file rappresenta uno strumento essenziale per il simulatore software del sistema logico-aritmetico a 32 bit basato su ALU 74181 e registri PIPO. Essa consente di velocizzare l’inserimento dei dati, ridurre il rischio di errori umani e ripetere le operazioni in modo deterministico.

Grazie a questa funzione, il programma diventa uno strumento utile non solo per scopi didattici ma anche per test ripetuti, debug avanzato e integrazione con altri sistemi. La combinazione di parsing accurato, validazione rigorosa e logging completo rende il simulatore uno strumento affidabile per comprendere il comportamento reale del sistema digitale originale.

#### CAPITOLO 30 – Output Formattato e Visualizzazione del Risultato
La rappresentazione grafica e la formattazione dell’output giocano un ruolo fondamentale nel simulatore software del sistema logico-aritmetico sincronizzato a 32 bit basato su ALU 74181 e registri. Essa permette all’utente di comprendere immediatamente:

Il risultato delle operazioni eseguite
Lo stato dei segnali di controllo
La propagazione del carry tra le ALU
L’effetto delle operazioni logiche e aritmetiche
Un output ben strutturato aumenta l’usabilità del programma, specialmente in contesti didattici o di debugging.

Obiettivo della visualizzazione
L’obiettivo principale del modulo di output è fornire all’utente un feedback chiaro e organizzato dopo ogni operazione effettuata. Questo include:

Visualizzazione binaria e decimale del risultato
Stampa dei valori degli operandi A e B
Mostra i segnali S0–S3, M e Cn
Indicazione dello stato del carry-out (Cn+4)
Output grafico migliorato con cornici e tabelle
Questo tipo di interfaccia rende il programma più leggibile e intuitivo, facilitando l’apprendimento e l’utilizzo avanzato.

Funzione stampa_risultato()
La funzione principale per mostrare i dati elaborati si chiama stampa_risultato() ed è definita come segue.
Essa riceve tutti i parametri utilizzati durante l’esecuzione dell’operazione e genera un output strutturato e facilmente interpretabile.
Questa funzione fornisce una panoramica completa del lavoro svolto e facilita l’analisi da parte dell’utente.

Output testuale semplice
Per garantire compatibilità con terminali meno avanzati o sistemi minimali, il programma include un'opzione --no-graphic che attiva un output lineare e privo di box drawing characters:
RISULTATO
Operand A: 123
Operand B: 45
Segnali: S0=0, S1=1, S2=1, S3=0
Modalità: Aritmetica (0)
Carry-in: 0
Risultato: 78
Carry-out: 0
Questa modalità è utile per script automatizzati, logging avanzato o esecuzioni batch.

Output grafico migliorato
Il programma utilizza caratteri Unicode e box drawing characters per creare una tabella ben formattata.

Questa interfaccia grafica migliorata rende immediatamente visibili gli errori e i risultati ottenuti, aumentando la chiarezza del programma.

Logging su file con formato coerente
Ogni volta che viene eseguita un’operazione, il programma registra i risultati in un file (risultati_alu32.txt) con lo stesso formato usato nell’output terminale.

Questo garantisce tracciabilità completa e aiuta nella validazione delle operazioni.

Supporto per formati strutturati (JSON)
Un miglioramento futuro potrebbe includere la possibilità di generare output in formato JSON , utile per analisi automatiche o integrazione con altri programmi.
Questa estensione richiederebbe l’utilizzo di librerie come jansson o json-c.

Output dettagliato del registro ALU
Il programma mostra anche il contenuto completo del registro di output (F0–F31), in modo da dare all’utente una visione precisa dei singoli bit calcolati
Questa funzionalità è particolarmente utile per verificare il comportamento reale dei singoli bit e per confrontarlo con la truth table dell’ALU.

Visualizzazione grafica dell’errore
Quando si verifica un errore, il programma emette un messaggio strutturato e facilmente interpretabile:
Output binario del risultato
Il programma mostra il risultato sia in formato decimale che in formato binario grazie alla funzione DEC_BIN_CODER(result)

Funzione stampa_memoria()
Il programma include una memoria temporanea dove vengono conservati i risultati intermedi. La funzione stampa_memoria() permette di visualizzare tutti i risultati archiviatiQuesta funzione mostra sia il valore decimale che la sua rappresentazione binaria, rendendo facile il recupero di dati precedenti.

Il menù principale del programma è anch’esso migliorato graficamente per aumentare la leggibilità e l’esperienza utente. 
Sebbene questa versione non utilizzi caratteri Unicode, essa è comunque ben strutturata e leggibile.
Questa formattazione migliora notevolmente l’aspetto visivo del programma.

Output dinamico durante l’esecuzione
Durante l’esecuzione di un’operazione, il programma mostra passo dopo passo il flusso del dato e la sincronizzazione con il clock.
Questo tipo di output aiuta l’utente a comprendere il comportamento sincrono del sistema.

Logging avanzato con timestamp
Tutti i risultati vengono registrati in un file con data e ora per consentire successive analisi.
I log possono poi essere importati in fogli di calcolo o analizzati con script per identificare pattern o anomalie.

Output per il convertitore binario-decimale
Il programma include una funzione dedicata al convertitore numerico, che mostra contemporaneamente il valore immesso e il risultato della conversione:


Output avanzato per il debug
In fase di sviluppo e testing, è stata aggiunta una funzione debug_ALU() che mostra i valori interni dell’unità ALU, utili per individuare bug o discrepanze:

Questa funzione è accessibile solo attivando una modalità speciale di debug, disattivata di default.
La capacità di fornire un output formattato e ben strutturato rappresenta uno strumento essenziale per rendere il simulatore software del sistema logico-aritmetico a 32 bit basato su ALU 74181 e registri PIPO uno strumento efficace per scopi didattici e di ricerca. Grazie alla combinazione di output grafico migliorato, logging avanzato, supporto per il convertitore binario-decimale e visualizzazione del registro di output, il programma offre una base solida per comprendere il comportamento reale del sistema digitale originale.

La scelta di mantenere un output modulare e flessibile, con opzioni per disattivare la grafica avanzata, aumenta ulteriormente la portabilità e l’utilità del programma in diversi contesti, dall’apprendimento universitario al testing automatizzato.

#### CONCLUSIONE
Il simulatore software del sistema logico-aritmetico sincronizzato a 32 bit basato su ALU 74181 e registri PIPO rappresenta un esempio pratico e didatticamente utile di come i fondamenti dell’architettura digitale possano essere replicati in ambiente virtuale. Attraverso l’utilizzo di strumenti semplici ma potenti come il linguaggio C, è stato possibile riprodurre fedelmente il comportamento di componenti hardware ormai storici, rendendoli accessibili anche a chi non dispone di un laboratorio fisico.

Lungo il percorso di sviluppo sono state affrontate numerose sfide:

La comprensione della truth table dell’ALU e la sua traduzione in codice
La gestione avanzata del carry tra le unità aritmetiche
La sincronizzazione tramite clock , con relativa emulazione del fronte positivo
L’implementazione modulare di registri e ALU multiple
La validazione dei dati immessi dall’utente , per garantire risultati coerenti
La registrazione completa delle operazioni tramite file di log e memoria temporanea
Questo tipo di progetto ha permesso di approfondire concetti fondamentali come:
La logica combinatoria e sequenziale
Il funzionamento delle ALU e dei registri sincroni
L’importanza della sincronizzazione nei sistemi digitali
La scalabilità dei circuiti logici verso parole più lunghe (es. da 4 bit a 32 bit)
Le problematiche legate all’input/output e alla precisione del calcolo


#### Ringraziamenti
Desideriamo ringraziare quanti hanno contribuito, direttamente o indirettamente, alla realizzazione di questo progetto:
I docenti e i ricercatori che ci hanno guidato nella comprensione dell’architettura digitale
Tutti coloro che hanno testato il programma e fornito feedback utili per migliorarlo
Gli sviluppatori open-source e le comunità tecnologiche che offrono strumenti e risorse gratuite per la sperimentazione
Chiunque utilizzerà questo simulatore per approfondire la conoscenza dei fondamenti del calcolo digitale
Note finali
Il simulatore si presenta come uno strumento educativo efficace, capace di trasformare concetti teorici in esperienze concrete. Pur essendo una rappresentazione astratta del sistema reale, esso mantiene un alto livello di fedeltà al comportamento originale, grazie alla precisa implementazione del funzionamento delle ALU e alla sincronizzazione con il clock.
Grazie della lettura
									
Leonardo Galli

Davide Ning Yu

Danilo Ambrogio Brusa

Zheming Feng

Oleksandr Pavlyk

Bohan Yang
