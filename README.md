# driver_checker  

Spiegazione delle impostazioni dell'applicazione:  
  
Model:  
- Threshold:
- Union-Over-Intersection Threshold:  
  
Window:  
- Window Threshold: percentuale minima di fotogrammi appartenenti a un certo gruppo (guidatore, passeggero) per poter considerare la finestra terminabile
- Window Size: grandezza della finestra
- Window Type: tipo di euristica tra Single Group (un fotogramma è valido se contiene solo oggetti appartenenti a un unico gruppo) oppure Multiple Group (un fotogramma è valido se contiene almeno un oggetto appartenente a un qualsiasi gruppo)
- Window Offset: numero minimo di fotogrammi per poter considerare la finestra terminabile
  
Per ogni parametro della window vengono scelti uno o più valori.  
Siano Th, S, Ty e O gli insiemi dei valori dei parametri, rispettivamente, per "Window Threshold", "Window Size", "Window Type" e "Window Offset".  
L'applicazione istanzia un totale di |W| = |Th| * |S| * |Ty| * |O| finestre che verranno riempite con i fotogrammi catturati dalla fotocamera frontale dello smartphone.  

Quindi, l'applicazione utilizza una euristica diversa rispetto a quella utilizzata da Marco Spallone nel suo lavoro di tesi.  
Mentre Spallone conta gli oggetti di una certa classe in una finestra, questa applicazione conta i fotogrammi di un certo gruppo (guidatore, passeggero) in una finestra.
