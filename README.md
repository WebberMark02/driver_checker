# DriverChecker

Spiegazione delle impostazioni dell'applicazione:  
  
Model:  
- Threshold: massimo valore di Intersection-Over-Union per ogni box. Oltre questo valore, la box di un oggetto viene rimossa.
- Union-Over-Intersection Threshold:  
  
Window:  
- Window Threshold: percentuale minima di fotogrammi appartenenti a un certo gruppo (guidatore, passeggero) per poter considerare la finestra terminabile
- Window Size: grandezza della finestra
- Window Type: tipo di euristica tra Single Group (un fotogramma è valido se contiene solo oggetti appartenenti a un unico gruppo) oppure Multiple Group (un fotogramma è valido se contiene almeno un oggetto appartenente a un qualsiasi gruppo)
- Window Offset: numero minimo di fotogrammi per poter considerare la finestra terminabile
  
Per ogni parametro della window vengono scelti uno o più valori.  
Siano Th, S, Ty e O gli insiemi dei valori dei parametri, rispettivamente, per "Window Threshold", "Window Size", "Window Type" e "Window Offset".  
L'applicazione istanzia un totale di |W| = |Th| * |S| * |Ty| * |O| finestre che verranno riempite con i fotogrammi catturati dalla fotocamera frontale dello smartphone.  
Un fotogramma catturato dalla fotocamera viene inviato al modello. Il modello classifica il fotogramma. Il fotogramma classificato viene inviato a ogni finestra istanziata.  
Ogni finestra di tipo "SingleGroup" accetta il fotogramma se e solo se il fotogramma contiene solo oggetti appartenenti a un solo gruppo (guidatore, passeggero).  
ogni finestra di tipo "MultipleGroup" accetta il fotogramma se e solo se contiene almeno un oggetto appartenente a un qualsiasi gruppo (guidatore, passeggero).  
Ogni finestra contiene una condizione di completamento. Una volta che questa condizione è soddisfatta, la finestra valuta tutti i fotogrammi contenuti al suo interno e non accetta alcun altro fotogramma finché  
la sua condizione iniziale non verrà ripristinata.  



Quindi, l'applicazione utilizza una euristica diversa rispetto a quella utilizzata da Marco Spallone nel suo lavoro di tesi.  
Mentre Spallone conta gli oggetti di una certa classe in una finestra, questa applicazione conta i fotogrammi di un certo gruppo (guidatore, passeggero) in una finestra.  
  
Da quel che ho capito, un singolo fotogramma viene classificato tramite l'euristica "baseline", quella utilizzata anche da Spallone e descritta nella tesi di Jaramillo Saa.  
Da quel che ho capito, l'euristica "baseline" viene solo usata per classificare i fotogrammi appartenenti a finestre della tipologia "MultipleGroup". Infatti, un
fotogramma appartenente a una finestra di tipo "SingleGroup" è valido solo se tutti gli oggetti che contiene appartengono a un unico gruppo (Guidatore, Passeggero).  
  
Spiegazione dei parametri generali mostrati in resultFragment e logFragment:  
- Average Confidence: media delle "confidence" delle finestre, calcolata così:  
       protected val averageConfidence: Float  
            get() = sumOfConfidencePerWindowDone/totalWindows
- Most Frequent Group: gruppo più frequente tra tutte le finestre
- Model Threshold: Threshold impostato per il modello

Spiegazione dei parametri per ogni finestra mostrati in resultFragment e logFragment:  
- Gruppo rilevato: gruppo rilevato tra "Passenger" e "Driver"
- Window Size: grandezza della finestra
- Window Threshold: percentuale minima di fotogrammi appartenenti a un certo gruppo (guidatore, passeggero) per poter considerare la finestra terminabile
- Window Offset: numero minimo di fotogrammi per poter considerare la finestra terminabile
- Confidence: percentuale di fotogrammi appartenenti al gruppo rilevato
- Type: euristica utilizzata nella finestra
- Total Time: tempo impiegato per terminare la finestra
- Total Windows:  
- Images: numero di fotogrammi appartenenti a un certo gruppo (guidatore, passeggero)  
- Classes: numero di classi (passenger-window, passenger-belt, driver-window, driver-belt) contate nella finesta. Se in un fotogramma appaiono più istanze della stessa classe, la classe viene contata una sola volta.  
- Objects: numero di oggetti appartenenti a un certo gruppo (guidatore, passeggero)  
  
Spiegazione dei parametri visibili in cameraFragment durante la valutazione:
- Parametri a sinistra: Images\:Classes\:Objects per Guidatore  
- Parametri a destra: Images\:Classes\:Objects per Passeggero  
- I quadratini colorati che appaiono durante la valutazione, da quel che ho capito, rappresentano i fotogrammi catturati e classificati  
