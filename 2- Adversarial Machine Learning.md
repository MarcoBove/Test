---
number headings: auto, first-level 1, max 6, 1.1
---

Detta anche AML, risulta essere una disciplina che si pone esattamente al centro tra il **machine learning** ed la **cybersecurity**.
SI preoccupa di dover studiare dei **pattern** non contemplati, ovvero degli **attacchi**, che cercano di ingannare la rete neurale ed analogamente cerca di identificare delle **contromisure**, ovvero delle **difese**, per mitigare questi attacchi.
Questo approccio risulta essere molto simile al concetto di *data augmentation*  per rendere la rete neurale più robusta, ad eventuali modifiche, in realtà non risulta essere l'unico approccio in quanto quest'ultimo potrebbe portare anche un **peggioramento sulla rete**, per quei campioni puliti.

Il concetto di **adversarial examples** a reti neurali per portarle in errore, fu del tutto casuale, infatti fu scoperta durante uno studio nel 2014 su **rumore** ad 
**alta frequenza**.

# 1 Sicurezza Reattiva e Proattiva
---
La sicurezza in generale all'interno dei sistemi è sempre stata una corsa alle armi, in particolare gli attaccanti continuamente progettano **nuove** **minacce** per **ingannare** i progettisti software.

Nell' approccio **reattivo**, il progettista software adatta il suo comportamento in risposta ad un eventuale minaccia, **dopo** aver già **progettato** il **sistema**.
Questo approccio tradizionale vediamo come non può prevenire i **never-seen-before** attacks.

Oggigiorno invece si preferisce un approccio del tipo **security by design** nel quale vi è un comportamento **proattivo**, vengono infatti definite delle soluzioni **in fase di progettazione**, per contrastare eventuali possibili attacchi futuri.

# 2 Security by design
---
In questo tipo di approccio **proattivo**, vi sono tre regole principali da dover tener a mente:
1. **Conoscere** sempre l'**avversario**
2. Avere un **comportamento** **proattivo**
3. **Proteggersi**
## 2.1 Conoscenza dell' avversario

Si necessita quindi **comprendere** quali sono le **informazioni** e quale è la **potenza** dell' **avversario**, in modo tale da poter **valutare** la **sicurezza** di un determinato **sistema** in accordo alla **potenza computazionale** del suddetto **attaccante**.
Per poter fare ciò è necessario definire un **framework** nel quale vengono considerati gli **obbiettivi** dell' **attaccante**, la **conoscenza** che ha quest'ultimo del **bersaglio** ed infine la **capacità** di **manipolare** i dati in **input**.

Questi aspetti vengono quindi definiti in termini di :
- Tipologia di **security violation**.
- **Specificità** dell' **attacco**.
- **Specificità** dell' **errore**.

### 2.1.1 Security violation

L'attaccante può quindi avere diversi obbiettivi e può **mirare** a **causare**:
- **Integrity violation**, ovvero **eludere** il **sistema** **senza compromettere** le **normali** **operazioni** dello stesso, ha quindi l'obbiettivo di ingannare il sistema stesso.
- **Avability violation**, ovvero **compromettere** il **normale** **funzionamento** del sistema anche agli **utilizzatori** **legittimi**. In questo caso il sistema **non** risulta più **disponibile** per nessuno.
- **Privacy violation,** ovvero **ottenere** informazioni **private** sul **sistema**, sui suoi **utenti** o sui **dati** ed **attraverso** il **reverse-engineering** l'algoritmo di apprendimento.
  Questo attacco può anche precedere uno dei due citati sopra.

### 2.1.2 Specificità dell' attacco
In termini di specificità l'attacco può dividersi in **due** categorie a seconda dei campioni affetti:
- **Targeted**, nel caso in cui l'**attacco** sia **indirizzato** ad un **singolo** e **specifico** sample.
  *Ad esempio*, nel caso in cui si voglia ingannare un classificatore, si sceglie una classe di appartenenza che si vuole attaccare.
  
- **UnTargeted** o indiscriminato, quando l'attaccante mira ad **effettuare** una **miss-classification** su **qualunque** **sample**.
  
Ovviamente risulta essere molto più complesso un tipo di attacco **targeted** rispetto ad un attacco **untargeted**.
In quanto **non** è **possibile** **sfruttare** la **classe** che, di base, ha **prestazioni peggiori**, rispetto al sample che si vuole fare miss-classificare.

### 2.1.3 Specificità dell' errore

L'errore causato ad un sistema, **basato sull' apprendimento**, può categorizzarsi in due modi:
- **Specific**, nel caso in cui l'attaccante **mira** ad **avere** una **miss-classificazione** su una classe specifica, ovvero una classe **target**.
  Ovvero il sistema deve riconoscere il suddetto campione, come quello di una determinata classe
- **Generic,** se l'attaccante mira a far **miss-classificare** un **campione** con una qualsiasi delle **classi**, **diverse** da quella **vera**.
  Si necessita quindi che il sistema semplicemente accetta il sample, anziché rigettarlo.

Spesso in letteratura vi è la suddetta uguaglianza:
Specific = Targeted
Generic = UnTargeted
In realtà questi concetti risultano essere **differenti** in quanto nell' **attacco** si parla di **campioni**, mentre nell' **errore** di classe **target**.

### 2.1.4 Formalizzazione della conoscenza

In generale vediamo come l'attaccante può avere diversi livelli di conoscenza del sistema target questi possono includere:
1. D, i **dati di training**
2. X, l' insieme delle **features**
3. f, l'**algoritmo di apprendimento** insieme con la loss function L da dover minimizzare
4. w, gli **hyperparameters** e i **parametri** da dover addestrare.

Possiamo quindi identificare la **conoscenza** dell' **attaccante** in termini di uno **spazio** dove gli elementi sono **codificati** secondo le **componenti** dello spazio stesso come:
$$\Theta = (D,X,f,w)$$
Queste **informazioni** risultano essere **importanti** così da poter identificare una eventuale possibilità di **replicazione** e quindi **mitigazione** dell' attacco.

La **potenza** dell' **attaccante** è quindi tanto **maggiore** **quanto** è **maggiore** la **conoscenza** di queste **informazioni**.
Identifichiamo quindi i possibili attacchi sulla base di queste informazioni.

#### 2.1.4.1 White-box attacks
Viene anche chiamato **perfect-knowledge (PK) white-box attacks**, in questo scenario si assume che l'attaccante conosca tutto riguardante il sistema target.
$$\Theta_{PK} = (D,X,f,w)$$
Questo risulta essere l'attaccante **più forte** di tutti, spesso infatti è anche **associato** ad un **progettista** che simula l'attacco e conosce tutto il suo sistema.
Se ci si **difende** da questo **attacco**, ci si **difende** da **qualsiasi** tipo di **attaccante** **minore**.

Grazie a questa definizione, derivate dal concetto di **security by design**, risulta quindi possibile poter effettuare una **worst-case evaluation** della sicurezza dell' algoritmo di apprendimento.
Risulta infatti possibile fornire un **upper-bound empirico** sulla **degradazione** delle **performances** durante un eventuale attacco.

#### 2.1.4.2 Gray-box attacks
Chiamato anche  **limited-knowledge (LK) gray-box attacks,**  questo risulta un caso medio in cui l'attaccante conosce soltanto parzialmente le informazioni descritte precedentemente.

In generale si assume che l'attaccante sia ancora a conoscenza della X, ovvero **l'insieme delle features** e della f, l'**algoritmo di apprendimento**, ad esempio *Resnet oppure VGG16*.
Risultano invece ignoti gli **hyperparameters** w ed i dati di **training** D.
Sulla base di queste ultime risulta infatti possibile **definire** dei **casi** **notevoli**.


Ipotizzando che un **attaccante** sia in grado di **raccogliere** un set di **dati** **surrogati** D' da una fonte simile.
Successivamente può ottenere un **feedback** dal **classificatore** grazie decisioni in output.
Ciò consente all'attaccante di **stimare i parametri w' da D'**, avendo così una stessa architettura ma con un differente training set.

Questo caso viene chiamato **LK attacks with Surrogate Data (LK-SD)**.
$$\Theta_{LK-SD}= (D',X,f,w')$$



Ipotizzando invece che l'attaccante non conosce nemmeno f, ovvero **l'algoritmo di apprendimento**, definiamo **LK attacks with Surrogate Learners (LK-SL)**.
$$\Theta_{LK-SL}= (D',X,f',w') $$
Avendo quindi un altro grado di incertezza, deve essere ipotizzata anche l'algoritmo di apprendimento e la loss, per poter creare un **surrogato**.

La creazione di **surrogati**, risulta essere una procedura comunemente utilizzata anche per valutare la **trasferibilità** degli **attacchi** tra gli **algoritmi** di **apprendimento**.
In generale vediamo come un attacco può essere anche trasferibile su più classificatori.

#### 2.1.4.3 Black-box attacks
Chiamati anche **Zero-knowledge (ZK) black-box attacks**, vengono detti tali in quanto non si è a conoscenza di nessuno degli aspetti della quadrupla. 

In realtà alcune informazioni sono sempre note all' attaccante, come l' **obbiettivo** della rete neurale e spesso anche l'**output**.
In particolare quindi l'**attaccante** ha un idea di quali **trasformazioni** poter **applicare** per causare il **cambiamento** di alcune **features**.

Possiamo in conclusione formalizzare la quadrupla come segue:
$$\Theta_{ZK}= (D',X',f',w')$$
In questo caso, si può comunque apprendere un classificatore **surrogato** e successivamente si può verificare se gli attacchi al surrogato risultano essere **trasferibili** al classificatore **target**.

### 2.1.5 Capacità dell' attaccante
Analizziamo quindi quali possono essere le capacità dell' attaccante, sulla base di caratteristiche che dipendono da:
- Quanta **influenza** l'**attaccante** ha sui **dati** in **input**, sia del *training* che del *test* set.
  In particolare l'attaccante può avere la possibilità o meno di poter intervenire sui dati del training set oltre al test set.
  Nel primo caso si parla di **Poisoning** 
  ![[Pasted image 20220923153228.png|center|500]]
  
  Mentre nel secondo di **Evasion**.
  ![[Pasted image 20220923153403.png|center|500]]

- Quali sono i **vincoli** sulla **manipolazione** dei **dati**, ovvero cosa l'attaccante può fare con i dati.
  In particolar modo dipende molto dalla **presenza** di **vincoli** specifici dell'applicazione sulla **manipolazione** dei **dati**.
  In generale vi è sempre un massima percentuale di modifica da dover effettuare per evitare di compromettere il normale funzionamento.


> [!NOTE] Security Evaluation curves
> In base alla **capacità** di un **attaccante**, di manipolare i dati in ingresso, questa curva rappresenta la **robustezza** dell' algoritmo di **apprendimento**.
> 
> Visto sotto un altro punto, la curva rappresenta l'accuratezza dell' algoritmo rispetto alla capacità e conoscenza dell' attaccante.
> 
> 

Generalmente la curva viene utilizzata per poter **confrontare** più **algoritmi** di **apprendimento** tra di loro e sulla base del **punto di intersezione** tra questi ultimi e **rispetto** alle **conoscenze** e **possibilità** dell' attaccante
Può venire quindi scelto un sistema piuttosto che un altro, valutando le curve per la robustezza.

![[Pasted image 20220928141827.png|center|500]]

## 2.2 Comportamento proattivo
Avere un comportamento **proattivo**, ovvero considerare in fase di progetto eventuali attacchi fattibili al nostro sistema, necessita di **conoscere** gli attacchi che un attaccante potrebbe mettere in atto.

In particolar modo, durante la trattazione ci concentriamo sugli  **evasion attacks**.

### 2.2.1 Evasion attacks
Informalmente questa tipologia di attacchi prevede, in tempo di test,  che un attaccante può **manipolare** i **campioni** di **prova**.
I **sample** utilizzati per l'**attacco** possono essere **ottimizzati** uno alla volta, in modo **indipendente**, con l'**obiettivo** di **massimizzare** la **probabilità** di **errore** del **classificatore**, ovvero massimizzare la **confidence** del classificatore associata alla classe errata.

Identifichiamo con $f_i(x)$ il livello di **confidence** del classificatore, sul sample x per la classe i. 

Formalizziamo questo concetto **identificando** rispettivamente **due tipologie** di evasion attacks, **error-generic ed error-specific**.

#### 2.2.1.1 Error-generic evasion attacks
In questo caso, l'attaccante è interessato nell'**ottenere** una **classificazione** **errata**, **indipendentemente** dalla **classe** di **uscita** prevista dal **classificatore**.
$$
max_{x'}\mathcal{A}(x',\Theta) = \Omega(x') = max_{l\neq k}[f_l(x')-f_k(x')]
$$
L'attaccante vuole quindi **massimizzare**, dato $x'$ **campione** d'**ingresso** modificato, la **strategia** $\mathcal{A}(x',\Theta)$ per **attaccare** il sistema, ciò vuole massimizzare la differenza tra $k$ la classe **reale** del sample ed $l$ un' altra classe generica del sistema.
Questa massimizzazione può valere se e solo se:
$$
\begin{cases}
d(x,x')\leq d_{max}\\
x_{lb} \leq x' \leq x_{ub}
\end{cases}
$$
La **prima condizione** impone che la **differenza** tra $x \text{ ed } x'$ risulta essere al più un valore $d_{max}$ . 
La seconda condizione impone che $x'$ debba avere dei **vincoli** dati  dalla rappresentazione del dato del vettore delle features, *ad esempio* i pixels hanno dei valori di rappresentazione  $[0,255]$.

Questo tipo di attacco può avere dei **vincoli** di **manipolazione** rispetto ai **dati** in **ingresso** ad esso stesso, ad esempio nel caso in cui ci sia un altro controllo del sistema, i vincoli possono essere di due tipologie:

##### 2.2.1.1.1 Vincolo di distanza
 Viene impostato un **upper-bound** sulla **massima** **perturbazione** dell' input $x$ rispetto all' input $x'$ modificato.
 Esistono diverse tipologie di vincoli sulla distanza e si dividono secondo il seguente schema:
 - $l_0$ **norma**, ovvero il vincolo sul massimo numero di pixels che possono essere cambiati nell' immagine $x'$.
 - $l_1$ **norma**, la distanza di Manhattan ovvero:$$l_1 = |x_1-x_1'|+|x_2-x_2'|+\cdots+ |x_n-x_n'|$$
  - $l2$ **norma**, la distanza Euclidea oppure errore medio quadratico:$$
   l_2 = \sqrt{(x_1-x_1')^2+(x_2-x_2')^2+\cdots+ (x_n-x_n')^2}
   $$ $l_2$ e $l_1$, vincolano in media la massima variazione sui pixels
 - $l_\infty$ **norma**, vincolo più stringente, questo misura la massima **differenza** su ciascuno dei pixels.
   $$l_\infty=max(|x_1-x_1'|,|x_2-x_2'|,\cdots,|x_n-x_n'|)$$
##### 2.2.1.1.2 Vincolo di box
Nel quale viene definito una sorta di **range**, nel quale è possibile **effettuare** **modifiche**. 
Nel **dominio** dell'**immagine**, il vincolo box può essere infatti **utilizzato** per **vincolare** ogni **pixel** tra 0 e 255, **oppure** per garantire la **manipolazione** solo di una **regione** **specifica** dell'immagine.
Questa limitazione è interessante per **creare** **esempi** **reali**, in quanto **evita** la **manipolazione** di **pixel** di **sfondo** che non appartengono all'oggetto di interesse.

#### 2.2.1.2 Error-specific evasion attacks
In questo caso vi è una **difficoltà** maggiore, in particolar modo, il valore di $l$ **non** può **essere** una **classe generica**, bensì deve essere **identificata** puntualmente e **fissata**, detta *classe speciale*.
$$
max_{x'}\mathcal{A}(x',\Theta) = \Omega(x') = \bcancel{max_{l\neq k}}[f_l(x')-f_k(x')]
$$
In questo caso, non vi è il massimo in quanto la classe è fissata ed è unica.

La logica è quella di **massimizzare** la '**confidence**' della **classe  sbagliata** l, **minimizzando** allo **stesso** **tempo** la probabilità di **classificazione** **corretta**. 

#### 2.2.1.3 Error-specific vs Error generic attacks
Vediamo come per poter identificare se un attacco è di tipo error specif o error generic si necessità anche dell' eventuale classe target.
![[Pasted image 20220928171538.png|center|500]]
Da quest' immagine possiamo sicuramente affermare che il primo può essere un attacco **error-specific** , in realtà però entrambi possono essere error-generic.

### 2.2.2 Adversarial examples
Gli esempi adversarial, risultano infatti essere soltanto un **caso particolare**,degli **evasion attacks**, essendo che il problema viene definito in modo generale, imponendo il **vincolo** sulla **distanza**.

In particolare modo questi esempi si pongono un **obbiettivo maggiore**, ovvero si vuole **minimizzare** la **distanza** del **campione avversario** $x'$ **rispetto** al corrispondente **campione sorgente** x, sotto il vincolo che la classe prevista sia diversa. 
$$
min_{x'}d(x,x') \Longrightarrow\; f(x)\neq f(x')
$$
L'attaccante **prima** ha come **obbiettivo** quello di **effettuare** la **miss-classificazione** e solo **successivamente** c'è l'aspetto di **minimizzare** la **perturbazione**.

## 2.3 Common Adversarial Attacks
Esploriamo ora gli **attacchi** **adversarial** più **comuni** in letteratura, questi **saranno**:
- Noise
- Fast gradient sign method FGSM
- Basic iterative method BIM
- Projected gradient descent PGD
- DeepFool
- Carlini-Wagner CW

Dove i primi tre attacchi risultano essere del **primo ordine** ovvero si **basano** soltanto sull' utilizzo del **gradiente** della **loss function** rispetto all' **input**. 

### 2.3.1 Noise
Questo rappresenta l'attacco adversarial più semplice, venne scoperto in accordo con gli esperimenti sulla robustezza delle reti neurali.
In particolar modo ci si accorse che vi sono dei **problemi** nel **generalizzare** gli algoritmi di **apprendimento** nel caso di pattern di **rumore** ad **alta** **frequenza**.

Per **variazioni** del **rumore** ad **alta frequenza** si **intendono** grandi **variazioni** tra un **pixel** e il suo **vicino**. Questi **pattern** di **rumore** possono essere creati **randomicamente** da una **distribuzione** **gaussiana** **standard**.
![[Pasted image 20220928174637.png|center|500]]

### 2.3.2 Fast Gradient Sign Method (FGSM)

Ricordando che l'apprendimento delle reti neurali risulta essere basato sull' algoritmo di **gradient descent**, ovvero i parametri della rete, i **pesi**, vengono **cambiati** **iterativamente** fino al raggiungimento del **minimo** della **loss function**.

Il **cambiamento** viene **effettuato**, calcolando il **gradiente** della **loss function**, **rispetto** ai **pesi** e viene **calcolata** la **direzione** ed il **modulo** per cui modificare i parametri.
La minimizzazione viene effettuata **cambiando** i **pesi** nella **direzione** **opposta** a quella del **gradiente**.


FGSM risulta essere un attacco **white-box**, ovvero in cui l'attaccante ha la completa conoscenza della rete da attaccare. 
Ricordiamo che la rete **non** è **necessariamente** quella **target**, ma può anche essere una sua rete surrogata.

> [!NOTE] Obbiettivo di FGSM
> La logica è quella di **sfruttare** la **conoscenza** della  **loss** function, **creando** un **campione** che **aumenti** la loss per la classe vera.

L'algoritmo utilizzato per ogni sample di test $x'$ risulta essere il seguente:
1. Viene presa l'immagine originale x e la sua classe di appartenenza **vera** y.
2. Viene effettuata la predizione su x utilizzando il modello $h(x,w)$
3. Si **calcola** la **loss** della predizione basata sulla **classe vera** y, $\mathcal{L}(h(x,w),y)$ 
4. Si calcola il gradiente della loss rispetto all' **input** x ,$\nabla_x\; \mathcal{L}(h(x,w),y)$
5. Si utilizza il **gradiente** calcolato per costruire l'**immagine** **avversaria** in uscita con una **perturbazione** **proporzionale** alla grandezza del **rumore** $\epsilon$, scelto arbitrariamente.
$$
x'=x+\epsilon \cdot sign(\nabla_x\; \mathcal{L}(h(x,w),y))
$$
Vediamo come la loss viene calcolata in base all' input e ne viene utilizzato il segno.

Il **segno** viene **mantenuto** **eliminando** il valore del **modulo**, in quanto essendo che il gradiente **non** è **limitato**, si possono avere dei **valori** **troppo** **grandi** fuori dal dominio.
![[Pasted image 20220928183156.png|center|500]]
In generale può essere calcolata la **security evaluation curve**, sulla base del valore $\epsilon$.
Il rumore può assumere in tutto $2^3$ valori, in quanto per ogni canale, tre in tutto RGB, può assumere $\pm\epsilon$.

### 2.3.3 Basic iterative method (BIM)
Questa tecnica risulta essere una **variante** ad FGSM, viene **aggiunto** **ripetutamente** del **rumore** all'immagine x **mediante** molteplici **iterazioni**, in modo da **causare** una **miss-classification**.

Definendo con $t$ il **numero** massimo di **iterazioni** e con $\alpha$ l'ammontare di **rumore** **aggiunto** ad ogni **step**, ottengo:
$$
x'_t = x_{t-1}+\alpha\cdot sign(\nabla_x\mathcal{L}(h(x_{t-1},w),y))
$$
Da come è possibile vedere, BIM risulta essere molto simile ad FGSM, in questo caso però $\alpha$ risulta essere **molto minore** rispetto a $\epsilon$, essendo che il **rumore** **aggiunto** risulta essere **incrementale**.
Vediamo come gli **hyper-parameter** da dover **impostare** risultano essere rispettivamente **due** , il primo è il numero di **iterazioni**, mentre il secondo risulta essere la $\epsilon$.

BIM è più raffinato di FSGM in quanto **non** si è **limitati** ad un **solo** **tentativo** ma si hanno **più iterazioni**. Il processo, sebbene sia lo stesso, viene **iterato** con **valori** più **piccoli**, in modo da **avvicinarsi** meglio alla **curva** di **decisione**.
Il **trade-off**, risulta essere quello di avere una **generazione** più **lenta** dell' **adversarial** **sample**, ma ciò **non** risulta essere di particolare **interesse** per un ipotetico **attaccante**.
![[Pasted image 20220929154531.png|center|400]]

Dalla foto, si sono messi a **confronto** due **rumori**, quello generato mediante FGSM e quello mediante BIM.
Notiamo che a **destra** con FGSM vi è una **variazione** **univoca**, infatti il **rumore** risultante è **uniforme** **avendo effettuato una sola variazione mediante epsilon**.
Invece a sinistra con BIM, ci sono alcuni punti più **rumorosi** **rispetto** ad **altri**, in particolare vengono modificati**i punti dove ci sono maggiori variazioni**, ovvero i *bordi*, in questo caso.

### 2.3.4 Projected gradient descent (PGD)
Avendo con BIM la limitazione secondo cui ad ogni passo si necessita muoversi sempre verso la direzione calcolata dal gradiente e non in un intorno.
PGD introduce della **randomness**, ovvero viene **aggiunta** una piccola **variazione** così da poter **calcolare** un **punto** in cui muoversi nell' **intorno di epsilon**.
Vediamo infatti come la x viene **inizializzata** in modo **random** da un rumore randomico, **proveniente** da una **distribuzione uniforme**, con i valori in range $(-\epsilon,\epsilon)$. Inoltre viene anche utilizzata una **funzione** di **proiezione** $\Pi_\epsilon$ su l'intorno di $\epsilon$.
$$
x'_t = \Pi_\epsilon[x_{t-1}+\alpha\cdot sign(\nabla_x\mathcal{L}(h(x_{t-1},w),y))]
$$
PGD risulta essere l'**attacco** **più** **forte** del primo ordine.

### 2.3.5 Error-specific gradient-based attacks
Nel caso si volessero invece effettuare questa tipologia di attacchi, volendo un attacco **error-specific**, ovvero dove la **classe target** risulta essere **data**, si necessita, non sommare il valore del gradiente nella direzione opposta, ma **sottrarre** la **differenza** dell' **input** con la **classe** **target** C.

SI usa il **segno** **classico** del **gradiente** **ottimizzando** pero verso la **classe** **specifica** **C**.
- FGSM
  Error-generic :$x'=x+\epsilon \cdot sign(\nabla_x\; \mathcal{L}(h(x,w),y))$
  Error-specific : $x'=x-\epsilon \cdot sign(\nabla_x\; \mathcal{L}(h(x,w),C))$ 
- BIM
  Error-generic : $x'_t = x_{t-1}+\alpha\cdot sign(\nabla_x\mathcal{L}(h(x_{t-1},w),y))$
  Error-specific : $x'_t = x_{t-1}-\alpha\cdot sign(\nabla_x\mathcal{L}(h(x_{t-1},w),C))$
- PGD
  Error-generic: $x'_t = \Pi_\epsilon[x_{t-1}+\alpha\cdot sign(\nabla_x\mathcal{L}(h(x_{t-1},w),y))]$
  Error-specific: $x'_t = \Pi_\epsilon[x_{t-1}-\alpha\cdot sign(\nabla_x\mathcal{L}(h(x_{t-1},w),C))]$

### 2.3.6 DeepFool
I successivi due attacchi proposti, questo e CW, **non** sono **basati** sul **gradiente**, hanno l'**obbiettivo** di **effettuare** l'**attacco** più **efficace** possibile con la **minima** **perturbazione**.
Vediamo inoltre come risultano essere più lenti, rispetto ai precedenti.

Vengono preferiti e spesso utilizzati in quanto, si ottiene lo stesso **classification rate**, ovvero lo **stesso** **errore**, avendo però un **distortion rate** più piccolo.
Ovvero la **metrica di distanza**, una delle l-norme viste, risulta **essere** mediamente $3$ volte più **piccola**.

#### 2.3.6.1 Implementazione dell' attacco
Supponendo che il **classificatore**, sia **lineare** ed abbia soltanto **due features**. Abbiamo quindi **soltanto** un **hyperplane** che divide lo spazio delle due classi.
![[Pasted image 20220929162200.png|center|300]]

Dato quindi un **input** x, essendo un attacco white box e conoscendo quindi tutti gli aspetti del sistema, viene **preso** l' **hyperplane**, **deepfool** **proietta** x sull' **hyperplane**, tracciando la **retta** **perpendicolare** al piano e passante per lo stesso x, viene poi identificato il punto più vicino, per costruzione, che **giace** nell' **altra classe**.

Lo **svantaggio** di questo approccio **risiede** nel poterlo **applicare** **soltanto** quando il **problema** risulta essere **linearmente** **separabile**.

##### 2.3.6.1.1 Caso con multiclass-classifier
Per un classificatore multiclass, con classificazione lineare, DeepFool trova l'**hyperplane** più **vicino** all' input x, successivamente **proietta** il **punto** sul **suddetto** **piano** e trova il **punto** più **vicino** di miss-classificazione.

Nel caso in cui il classificatore **non** sia **lineare**, vengono effettuate **diverse** **iterazioni** dove si cerca di linearizzare il **classificatore** **intorno** all' input x per poi applicare lo stesso metodo. 
Ovviamente questa metodologia non risulta essere così efficiente.
![[Pasted image 20220929163702.png|center|540]]


### 2.3.7 Carlini Wagner
L'**attacco** è definito secondo una **processo** di **ottimizzazione**, il quale cerca la **minima** **distanza** che garantisce di far **classificare** al campione con la **classe** **errata**.
Nel caso di error-generic è la classe più vicina, mentre se error-specific è una classe data.

Questo attacco ha **minore trasferibilità** sulle altre reti. Questa affermazione è stata provata mediante un calcolo empirico.

In generale in accordo con quanto detto precedentemente, gli **attacchi** di tipo **gradient based** risultano essere più **trasferibili**, in quanto l'**algoritmo** che genera l'attacco **non** è **improntato** sul **modello** di **rete**, **bensì** sul **campione** da **attaccare**.

### 2.3.8 Evasion attacks on black-box models
Vediamo ora come risulta possibile poter **effettuare** degli evasion attacks, quando abbiamo un **modello** **non disponibile** e quindi ci troviamo in un caso **black-box**.
L'attaccante può utilizzare il concetto di **trasferibilità**, secondo  la seguente procedura:
- Viene allenato un modello **surrogato**, che cera di **copiare** l'andamento del modello **target**.
- Vengono **generati** più **campioni** **adversarial**, con differenti **white-box attacks**, utilizzando il modello surrogato.
- Vengono **applicati** i **samples** **adversarial**, al modello **target** e viene **verificata** la **trasferibilità** dell' **attacco**.

Sebbene, abbiamo detto che difendere il sistema rispetto ad attacchi white-box assicuri la miglior **robustezza**, vediamo come, anche dal punto di vista del **progettista**, risulta utile **applicare** questa **tecnica** in quanto risulta possibile controllare se il sistema soffre di attacchi **trasferibili**.

Questo in quanto spesso l'**attaccante**, partendo da una **condizione** **sfavorevole** ovvero black-box **potrebbe**, per trasferibilità **attaccare** la **rete** in modo efficiente.

## 2.4 Protect yourself
In generale si necessita **identificare**, **dopo** aver visto i vari **attacchi**, una **metodologia** che permetta di poter avere delle **misure** di **sicurezza**.
In generale si necessita difendersi da **due tipologie di attacchi**:
- **Attacchi passati**, si necessita effettuare una **difesa reattiva**. 
  Non basta però quest' ultima in quanto, non ci si protegge da eventuali attacchi futuri.
  Il metodo di difesa reattiva è il **re-training**, sui samples miss-classified, **non** si è però **sicuri** che la **difesa** sia **efficace**.
- **Attacchi futuri**,  gli approcci di **difesa** possibili risultano essere di **due** tipologie:
	- **Security by design**, contro i white-box attacks, permette di poter effettuare un **secure learning**, **andando** durante l'**apprendimento** a **tenere** in conto queste **problematiche**. 
	  L'attack detection risulta essere un' altra metodologia di difesa.
	- **Security by obscurity**, non risulta un metodo funzionante, in quanto esiste il concetto di **trasferibilità** che permette comunque attacchi.
	  In generale le tecniche sono quelle di **information hiding**, **randomization**.

Andiamo quindi ad analizzare di seguito diverse strategie di difesa:
- Adversarial training
- Image pre-processing before classification
- Detection of adversarial samples
- Gradient masking
- Robust optimization
  
Tenendo però in conto che esistono molte altre difese, [elenco aggiornato](https://www.robust-ml.org/defenses/).

### 2.4.1 Adversarial training
Durante l'**adversarial** **training** il **difensore** **riaddestra** il **modello** con esempi **avversari** nel training set, ma **etichettati** con **labels corrette**.
Questo fa **apprendere** al modello come **ignorare** il **rumore** ed apprendere soltanto da **features robuste**.

#### 2.4.1.1 Problematiche di questo approccio
Vediamo come questo approccio **non** risulta **scalabile** in quanto **nuovi** **samples** potrebbero essere **modificati** per **ingannare** il **sistema**, sulla base dei samples modificati precedentemente. 
Risulta addirittura **controproducente** in quanto, se si **reitera** questo **processo** si arriva ad un punto in cui, si **ri-allena** il modello con troppi dati **fake** e diventa **inutilizzabile** anche per i **samples corretti**.

Riassumendo viene **generalizzato** troppo il **modello**  e si **perde** **accuratezza** della rete sui **campioni** **veri**, inoltre risulta essere molto onerosa come soluzione.
Infatti, in generale **bisogna** sempre avere una **buona performance** sui **campioni** **puliti**. A prescindere da eventuali errori.

### 2.4.2 Gradient masking
Questa praticamente **non** è una **difesa**, l'**obbiettivo** è di **non permettere** all' **attaccante** di **utilizzare** il **gradiente**.
Vediamo come questa metodologia risulta essere inoltre **inefficiente** **rispetto** ad **attacchi** DeepFool e Carlini-Wagner. 

### 2.4.3 Image pre-processing before classification
In generale vediamo come un **avversario** **non** può **sapere**, che tipo di **pre-processing** è stato **effettuato** sull' **input**, come ad esempio la normalizzazione dei dati.
Un ' idea sarebbe applicare **de-noising** sull'**immagine**, questo **non** ha **effetto** sui **campioni** **puliti** ma **elimina** eventuale **rumore** dai campioni modificati per gli attacchi.

Sperimentalmente infatti compressioni come il JPEG, o altre trasformazioni, hanno dimostrato di essere efficaci  nel ridurre l'**errore** degli **adversarial** **samples**, ma **degradano** anche le **prestazioni** **generali**.

Una delle tecniche più famose di questa famiglia, risulta essere la **Defensive-GAN**, che **permette** a partire dal **sample** **malevolo** di **ripulirlo** del suo rumore.

### 2.4.4 Detection of adversarial samples
Viene utilizzata un' altra **rete** **neurale** a **monte** del **classificatore** già allenata, questa permette di poter **distinguere** samples con **rumore** da samples **puliti**.

In generale viene **addestrata**  sottoponendo a questa rete samples con rumore o senza, facendo riconoscere i medesimi campioni.

### 2.4.5 Robust optimization
Vediamo come risulta possibile poter utilizzare delle metodologie generiche per irrobustire la rete.
- **Regolarizzazione**, la **regolarizzazione** permette di avere **regioni** di **decisione** più **smooth** e **separate**, la **perturbazione** da dover applicare sul sample, in questo caso, risulta quindi essere **più** **intensa**, per ingannare la rete.
- **Certificazione della difesa**, in accordo con la **security evaluation curve**, per un dato set di dati e un dato modello, risulta utile trovare il **limite inferiore della perturbazione minima.**

# 3 Evaluation Recommendations
---
Elenchiamo ora alcune linee guida per poter progettare in modo metodologico una rete neurale che sia robusta rispetto agli attacchi visti fin ora.

## 3.1 Riportare model accuracy sui dati puliti
Vediamo come risulta molto importante riportare le **performance** del **modello** sui **dati puliti**, prima e dopo aver applicato una qualsiasi metodologia di difesa. 

Una **difesa** che **degrada** significativamente l'**accuratezza** del **modello** sul **task** **originale**, ovvero sui dati puliti, risulta **inutilizzabile** in molte situazioni.
Se la **probabilità** di un **attacco** effettivo è molto **bassa** e  un **errore** sugli **input avversari** **non** è **elevato**, allora può essere **inaccettabile** **incorrere** in una **diminuzione** dell'**accuratezza**.

## 3.2 Concentrarsi sugli attacchi più forti 
Risulta efficace prendere **maggiormente** in **considerazione** gli **attacchi** **più efficaci**, in particolar modo **non** si **parla** **solo** di **attacchi** con **conoscenza** del sistema di tipo **white-box**, elenchiamo infatti di seguito le buone pratiche da dover seguire.
- **Utilizzare attacchi** di cui si conosce l'**ottimizzazione**:
  Carlini Wagner con un **distorsione** $l_2$
  PGD con una **distorsione** $l_\infty$
- **Non testare** soltanto **attacchi** **utilizzati** durante il **training**
  Risulta importante valutare la robustezza del sistema, ovvero la sua **capacità di generalizzazione**, su **differenti attacchi.**
- **Non testare** soltanto **attacchi** **simili** tra loro
	Per esempio, l'applicazione dei soli **FGSM, BIM e PGD** potrebbe **non** essere **sufficiente**, poiché questi attacchi sono molto **simili** tra loro.
	
## 3.3 Effettuare un' analisi di trasferibilità
Risulta **utile** **verificare** questa **proprietà** in quanto l'**attaccante** molto probabilmente farà **attacchi black-box** piuttosto che **white-box**, valutando quindi la **trasferibilità** su modelli surrogati.
  
## 3.4 Verificare la convergenza dell'attacco
Su almeno un paio di input, **verificare** quale **numero** di **iterazioni** di **gradient descent** è **sufficiente** all' attacco , **tracciando il successo dell'attacco rispetto al numero di iterazioni.**

Questo diagramma dovrebbe infine **stabilizzarsi**, se non lo fa, l'**attacco** **non** è ancora  **convergente**, si necessita quindi aumentare le iterazioni.
Il numero di iterazioni necessarie è in genere inversamente proporzionale alla dimensione del passo e proporzionale alla distorsione consentita, bisogna tener però in conto anche la complessità del set di dati.

## 3.5 Studiare  gli hyperparameters di attacco
Per avere una robustezza sull' attacco si necessita investigare attentamente sull' effetto degli hyper-parameter all' interno del nostro modello.
Per poter far ciò bisogna effettuare delle **sperimentazioni** con un **set** di hyper-parameters più **eterogeneo** possibile, cercando di considerare nelle varie security evaluation curve questo aspetto.

## 3.6 Provare attacchi sia error specific che generic
 In teoria, un attacco **error-generic** è più **facile** di un attacco **error-specific**.  In realtà questi due attacchi **non** risultano essere **strettamente** **correlati** tra loro, in quanto uno **lavora** sul **ridurre** la percentuale di **predizione** corretta mentre l'**altro** sull' **aumentare** la **percentuale**  ma su una **classe** **errata**.
Per questo motivo risulta **utile** poter **provare** **entrambi** gli **attacchi**.

# 4 Analysis Recommendations
---
Ci concentriamo invece ora su quale deve essere l'analisi  per poter effettuare una comparazione ed uno studio rispetto ai parametri ed alle statistiche ottenute.

## 4.1 Comparare con lavori precedenti
Si necessita **confrontare** i **risultati** **ottenuti**, con eventuali altri **risultati** emersi nello studio effettuato da **altri** **ricercatori**.
Questo in quanto è altamente **improbabile** che un'**idea** sia **completamente** **nuova** e **non** **correlata** a nessuna **difesa** precedente.

Risulta però fare attenzione sulla **validità** dei risultati, ovvero bisogna confrontare questi risultati in relazione al momento in cui sono stati riportati e se questi siano ancora validi e **non obsoleti**.

## 4.2 Effettuare un sanity test
Si necessita verificare che il **test** e le **implementazioni** correlate, siano state **fatte** **correttamente**, questo può essere effettuando verificando diverse proprietà:
- Gli attacchi **iterativi** devono **avere** **prestazioni** **migliori** rispetto a quelli singoli.
- Il tasso di **successo** dell' attacco aumenta in accordo alla **perturbazione**.
- Con una alta **distorsione**, il modello dovrebbe fornire risposte randomiche.

## 4.3 Generare la security evaluation curves
Si necessita durante la diagnosi fare una **security evaluation curve**. 
In quanto  la **definizione** dell'**accuratezza** e dell'**errore** deve essere fatta sulle **condizioni** a **contorno**.

Infatti bisogna **analizzare** il **problema** sempre in accordo ai **limiti** imposti dal **sistema** da dover **attaccare**, se non ci fossero limiti si dovrebbe avere un **successo** nell' **errore** del 100%.

Vediamo come per gli attacchi **unbounded** infatti, come metrica conviene utilizzare la **distorsione** e non la percentuale di successo.

## 4.4 Verificare white box attacks migliori
Poiché negli attacchi white-box si hanno **più** **conoscenze** rispetto ai black-box, si dovrebbero avere **prestazioni** **migliori** o al massimo uguali, anche se raro.

In particolare ciò implica che gli **attacchi** basati sul **gradiente**, ovvero white box, dovrebbero, **superare** gli **attacchi** **senza** **gradiente**.Infatti se si ottengono **migliori** **prestazioni** su attacchi **non** basati su **gradiente**, significa che il **gradiente** sta venendo **nascosto**, come difesa.

## 4.5 Utilizzare altri domini
Valutando l'efficacia di una difesa rispetto ad un attacco **general purpose**, si può effettuare l'attacco anche su **domini** **diversi**, come ad esempio l'audio.

## 4.6 Report degli attacchi effettuati
Dopo aver eseguito una valutazione completa, le valutazioni della difesa deve **riportare** **tutti** gli **attacchi** e **includere** i relativi **hyperparameters**, in modo tale da consentirne la **riproducibilità**.

Risulta particolarmente importante riportare il numero di iterazioni per gli attacchi basati sull'ottimizzazione.

Inoltre **piuttosto** che riportare **attacchi** **simili**, risulta di particolare interesse riportare più **attacchi** **differenti**.