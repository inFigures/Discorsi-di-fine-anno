# Discorsi-di-fine-anno
Questa è una analisi dei discorsi di fine anno dei Presidenti della Repubblica Italiana. Tutti i discorsi sono quelli ufficiali che sono disponibili sul sito del **Quirinale** alla [pagina](https://presidenti.quirinale.it/).

Gli URL esatti di ciascun singolo discorso dal 1948 ad oggi sono disponibili nel file `elenco_discorsi.csv`

Tutti i discorsi in formato `.txt` sono contenuti nella cartella `\discorsi`

## Note importanti
* Le trascrizioni disponibili sul sito del Quirinale non sono sempre delle migliori. Ci sono parecchi errori di battitura, grammaticali e di ortografia. Per questo motivo in molti casi si è provveduto ad una correzione manuale di diversi discorsi. Un lavoro manuale che ha richiesto del tempo. 
* Il primo discorso del Presidente della Repubblica fu tenuto da Luigi Einaudi il 31/12/1949
* Sul sito del Quirinale i discorsi del 1952 e 1953 sono identici per un errore di chi gestisce il titolo. Pertanto il discorso del 1952 (file 1952.txt) è stato sostituito con quello effettivamente pronunciato. Il testo è stato trovato a questo indirizzo [discorsi Einaudi](https://archivio.quirinale.it/discorsi-bookreader//discorsi/Einaudi.html#page/424/mode/2up)

## Automazione dello scarico dei discorsi
In prima battuta si è cercato di automatizzare lo scarico dei testi tramite tecniche di web scraping. Purtroppo il sito del Quirinale blocca tutte le richieste provenienti da bot o script automatizzati. La libreria `requests` di python pertanto non può essere utilizzata. L'alternativa è utilizzare codice javascript direttamente nella console di Chrome. Ecco un esempio di codice che salva in un file `trascrizione.txt` il discorso di una pagina, ad esempio https://presidenti.quirinale.it/elementi/237076 

```javascript
// XPath del div che contiene il discorso (può cambiare a seconda delle pagine)
const xpath = '//*[@id="content-col"]/div[2]/div[1]/div';

const result = document.evaluate(xpath, document, null, XPathResult.FIRST_ORDERED_NODE_TYPE, null).singleNodeValue;

if (result) {
    // Considero solo il testo indipendentemente da eventuali tag <p> presenti nel <div>
    const text = result.innerText;

    const blob = new Blob([text], { type: 'text/plain' });
    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = 'trascrizione.txt';
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    console.log('Download completato!');
} else {
    console.log('Elemento non trovato con l\'XPath specificato.');
}

```
Per utilizzare questo codice su Chrome, visitare la pagina https://presidenti.quirinale.it/elementi/237076 premere il tasto `F12` per aprire la Console di Chrome, incollare il codice e premere invio. Verrà scaricato un file denominato `trascrizione.txt` con il discorso del Presidente Leone tenuto il 31/12/1975.

Eventualmente questo codice javascript può essere utilizzato in uno script python che utilizza la **libreria Selenium**. Il punto è che il codice non può essere ottimizzato per tutte le estrazioni per via del fatto che la struttura delle pagine che ospitano i discorsi sono spesso diverse tra loro. 
