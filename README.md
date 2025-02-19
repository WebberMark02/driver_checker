# DriverChecker

## Spiegazione delle impostazioni dell'app.
  
Model:  
- Threshold: massimo valore di Intersection-Over-Union per ogni box. Oltre questo valore, la box di un oggetto viene rimossa.
- Union-Over-Intersection Threshold:  
  
Window:  
- Window Threshold: percentuale minima di fotogrammi appartenenti a un certo gruppo (guidatore, passeggero) per poter considerare la finestra soddisfacibile
- Window Size: grandezza della finestra
- Window Type: tipo di euristica tra Single Group (un fotogramma è accettato dalla finestra se contiene solo oggetti appartenenti a un unico gruppo) oppure Multiple Group (un fotogramma è accettato dalla finestra se contiene almeno un oggetto appartenente a un qualsiasi gruppo)
- Window Offset: numero minimo di fotogrammi che la finestra deve valutare per poter essere considerata soddisfacibile

## Spiegazione del funzionamento generale dell'app  
  
Per ogni parametro della window vengono scelti uno o più valori.  
Siano Th, S, Ty e O gli insiemi dei valori dei parametri, rispettivamente, per "Window Threshold", "Window Size", "Window Type" e "Window Offset".  
L'applicazione istanzia un totale di |W| = |Th| * |S| * |Ty| * |O| finestre che verranno riempite con i fotogrammi catturati dalla fotocamera frontale dello smartphone.  
Una volta che viene premuto il tasto "START LIVE", inizia la valutazione in tempo reale.  
Un fotogramma catturato dalla fotocamera viene inviato al modello. Il modello classifica il fotogramma. Il fotogramma classificato viene inviato a ogni finestra istanziata.  
Ogni finestra di tipo "SingleGroup" accetta il fotogramma se e solo se il fotogramma contiene solo oggetti appartenenti a un solo gruppo (guidatore, passeggero).  
Ogni finestra di tipo "MultipleGroup" accetta il fotogramma se e solo se contiene almeno un oggetto appartenente a un qualsiasi gruppo (guidatore, passeggero).  
Ogni finestra contiene una condizione di soddisfazione. Una volta che questa condizione è soddisfatta, la finestra valuta tutti i fotogrammi contenuti al suo interno e non accetta alcun altro fotogramma finché  
la sua condizione iniziale non verrà ripristinata.  
Una finestra è soddisfatta se è stata riempita completamente e se il suo valore di confidence è maggiore o uguale al suo valore di threshold.
Una volta che il gestore delle finestre riconosce che tutte le finestre sono state soddisfatte, la valutazione termina e vengono mostrati i risultati, che possono essere salvati nel database locale dell'applicazione.  
Non necessariamente tutte le finestre devono essere soddisfatte.  
Ecco le euristiche specifiche:  
- una valutazione, prima di considerare il risultato valido, deve superare un certo
limite ThMIN di confidence che deve essere almeno del 50%
- una valutazione deve avere un limite massimo di tempo TimeMAX per completarsi.
Al raggiungimento del limite si utilizza il valore calcolato anche se non tutte le
Window son state soddisfatte
- l’unico caso in cui una valutazione può superare TimeMAX è se Conf < ThMIN
- porre un limite massimo ThMAX di confidence per poter decretare il risultato con certezza
- è necessario dare più importanza ad un falso positivo rispetto ad un falso negativo
per poter mantenere la funzionalità di prevenzione.
  
Il modello usato dall'applicazione è YOLOv5.  
Da quel che ho capito, Spallone usò YOLOv8 nel suo lavoro di tesi.  
Invece, Jaramillo ha usato YOLOv8.  
  
Quindi, l'applicazione utilizza una euristica diversa rispetto a quella utilizzata da Marco Spallone nel suo lavoro di tesi.  
Mentre Spallone conta gli oggetti di una certa classe in una finestra, questa applicazione conta i fotogrammi di un certo gruppo (guidatore, passeggero) in una finestra.  
    
Da quel che ho capito, un singolo fotogramma viene classificato tramite l'euristica "baseline", quella utilizzata anche da Spallone e descritta nella tesi di Jaramillo Saa.  
Da quel che ho capito, l'euristica "baseline" viene solo usata per classificare i fotogrammi appartenenti a finestre della tipologia "MultipleGroup". Infatti, un
fotogramma appartenente a una finestra di tipo "SingleGroup" è valido solo se tutti gli oggetti che contiene appartengono a un unico gruppo (Guidatore, Passeggero).  
  
## Spiegazione dei parametri mostrati nei risultati di una valutazione
  
Spiegazione dei parametri generali mostrati in resultFragment e logFragment:  
- Average Confidence: media delle "confidence" delle finestre, calcolata così:  
       protected val averageConfidence: Float  
            get() = sumOfConfidencePerWindowDone/totalWindows
- Most Frequent Group: gruppo più frequente tra tutte le finestre
- Model Threshold: Threshold impostato per il modello

Spiegazione dei parametri per ogni finestra mostrati in resultFragment e logFragment:  
- Gruppo rilevato: gruppo rilevato tra "Passenger" e "Driver"
- Window Size: grandezza della finestra
- Window Threshold: percentuale minima di fotogrammi appartenenti a un certo gruppo (guidatore, passeggero) per poter considerare la finestra soddisfacibile
- Window Offset: numero minimo di fotogrammi per poter considerare la finestra soddisfacibile
- Confidence: percentuale di fotogrammi appartenenti al gruppo rilevato
- Type: euristica utilizzata nella finestra
- Total Time: tempo impiegato per soddisfare la finestra
- Total Windows: numero di volte in cui la finestra ha dovuto fare uscire un fotogramma per accoglierne un altro
- Images: numero di fotogrammi appartenenti a un certo gruppo (guidatore, passeggero)  
- Classes: numero di classi (passenger-window, passenger-belt, driver-window, driver-belt) contate nella finesta. Se in un fotogramma appaiono più istanze della stessa classe, la classe viene contata una sola volta.  
- Objects: numero di oggetti appartenenti a un certo gruppo (guidatore, passeggero)
  
Per ogni fotogramma, viene mostrato il tempo trascorso dalla sua acquisizione fino al termine della sua analisi.
  
## Spiegazione dei parametri visibili durante una valutazione
  
Spiegazione dei parametri visibili in cameraFragment durante la valutazione:
- Parametri a sinistra: Images\:Classes\:Objects per Guidatore  
- Parametri a destra: Images\:Classes\:Objects per Passeggero  
- I quadratini colorati che appaiono durante la valutazione, da quel che ho capito, rappresentano i fotogrammi catturati e classificati  
