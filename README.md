# 🍽️ Menù online con ordine via QR

Un sito web **statico** (una sola pagina, senza server né database) che mostra il menù di un evento o di un locale e permette ai clienti di **preparare l'ordine dal proprio smartphone**. Alla fine l'ordine viene trasformato in un **codice QR** da mostrare in cassa, dove un lettore lo legge e lo registra.

È pensato per sagre, feste, eventi benefici, mense, chioschi o piccoli locali che vogliono ridurre code ed errori di trascrizione, ma si adatta a qualsiasi situazione in cui un cliente sceglie da un listino e poi paga in un punto fisico.

> ℹ️ Il QR contiene i dati dell'ordine, **non certifica il pagamento**: il pagamento avviene in cassa.

---

## 🎯 Come funziona

1. Il cliente apre il sito dal telefono (ad esempio inquadrando un QR code esposto sui tavoli o all'ingresso).
2. Indica il numero di **coperti** (campo obbligatorio).
3. Sfoglia il menù per categoria o con la ricerca e sceglie piatti e quantità.
4. Controlla il riepilogo e il totale, poi preme **"Conferma ordine e genera QR"**.
5. Mostra il QR in cassa (meglio uno screenshot): il lettore ne legge il contenuto e carica l'ordine.

Tutto il calcolo avviene nel browser del cliente: il sito non invia nulla a nessun server.

---

## ✨ Funzionalità

- **Menù diviso in categorie**, con ricerca e filtro.
- **Foto dei piatti** (facoltative) con prezzo e note.
- **Allergeni** per ogni piatto, con i 14 allergeni principali (più funghi e mais) e icone dedicate.
- **Varianti/gusti** per un piatto: ad esempio un dessert con più gusti, ognuno con la propria foto e i propri allergeni.
- **Gestione dei coperti** e calcolo automatico del totale.
- **Generazione del QR** direttamente nel browser.
- **Invio opzionale del riepilogo su WhatsApp**.
- **Chiusura degli ordini** con un solo interruttore.
- **Avviso prima di ricaricare la pagina**: se c'è un ordine in corso, il browser chiede conferma perché l'ordine andrebbe perso.
- **Design responsive**, pensato per lo smartphone.

---

## 📁 Struttura

```
.
├── index.html        # Pagina unica: HTML, CSS, JavaScript e libreria QR
└── img/              # Foto dei piatti (facoltative), divise per categoria
    ├── <Categoria 1>/
    ├── <Categoria 2>/
    └── ...
```

Non servono build, dipendenze o backend. L'unica risorsa esterna sono i font di Google Fonts, caricati da internet.

---

## 🚀 Pubblicazione

### In locale

Apri `index.html` con un browser.

### Con GitHub Pages (gratuito)

1. Crea una repository (o fai un *fork* di questa) e carica i file.
2. Vai su **Settings → Pages**.
3. In *Source* scegli il branch (es. `main`) e la cartella `/ (root)`.
4. Dopo qualche minuto il sito sarà online all'indirizzo mostrato da GitHub.

Va bene qualsiasi altro hosting per siti statici (Netlify, Cloudflare Pages, il tuo server web…): basta caricare i file.

---

## ⚙️ Come adattarlo al tuo evento

Tutta la personalizzazione si fa modificando `index.html` con un editor di testo qualsiasi.

### 1. Titolo, nomi e contatti

Sostituisci i testi nelle seguenti parti:

| Cosa | Dove |
|---|---|
| Titolo della scheda del browser | tag `<title>` nell'`<head>` |
| Descrizione per motori di ricerca e anteprime | `<meta name="description">` |
| Intestazione della pagina | blocco `<header>`: riga superiore (`kicker`), titolo (`<h1>`), medaglione con l'edizione (`.star`), sottotitolo (`.sub`) |
| Testi finali | tag `<footer>` |
| Link ai social | icone nella sezione `social`: sostituisci i `#` negli `href` con gli indirizzi delle tue pagine |

Colori e stile sono nel blocco `<style>`.

### 2. Impostazioni rapide

Nella sezione `CONFIGURAZIONE` del codice JavaScript:

```js
var ORDINI_APERTI = true;   // false = ordini chiusi (compare un avviso)
var WHATSAPP = "";          // es. "393331234567": prefisso senza "+". Vuoto = pulsante nascosto
```

### 3. Piatti e prezzi

L'elenco è l'array `MENU`. Ogni riga ha questo formato:

```js
[categoria, id, nome, prezzo, nota, allergeni]
```

| Campo | Significato |
|---|---|
| `categoria` | Nome della categoria. Le categorie compaiono nell'ordine in cui appaiono per la prima volta nell'elenco. |
| `id` | Codice breve e **unico** che finisce nel QR. Evita virgole, `\|` e la lettera `x` isolata (es. `Pasta.Rag`). |
| `nome` | Nome mostrato al cliente. |
| `prezzo` | Numero, con il punto per i decimali (es. `2.5`). |
| `nota` | Testo facoltativo sotto il nome; usa `""` se non serve. |
| `allergeni` | Nomi separati da virgola, es. `"Latte, Uova, Cereali"`. Facoltativo. |

Esempio:

```js
["Primi", "Pasta.Rag", "Pasta al ragù", 7, "", "Cereali, Uova, Sedano"],
["Bevande", "Acqua", "Acqua ½L", 1],
```

**Allergeni validi:** Cereali, Crostacei, Uova, Pesce, Arachidi, Soia, Latte, Frutta a guscio, Sedano, Senape, Semi di sesamo, Anidride solforosa e solfiti, Lupini, Molluschi, Funghi, Mais.
Se il campo è vuoto, il sito mostra "non ancora indicati: chiedi al personale".

#### Il coperto

La riga con id `Coperto` è **obbligatoria**: il sito usa il suo prezzo per calcolare i coperti, che il cliente deve indicare per procedere.

```js
["Coperto", "Coperto", "Coperto", 1.5]
```

Se non applichi un costo di coperto, metti `0` come prezzo: il campo resterà comunque il "numero di persone".

### 4. Varianti o gusti

Puoi attivare la scelta di varianti su **un massimo di due piatti** del menù (ad esempio due dessert con più gusti). Si configura nei blocchi `GUSTI_DI` / `GUSTI` e `GUSTI_TORTA_DI` / `GUSTI_TORTA`:

- `GUSTI_DI` contiene l'`id` del piatto del `MENU` che apre la scelta.
- Ogni variante ha il formato `[id, nome, allergeni, immagine]`.
- Il prezzo di ogni variante è quello del piatto di riferimento nel `MENU`.
- Se non ti servono varianti, svuota gli array.

```js
var GUSTI_DI = "Gelato";
var GUSTI = [
 ["Gel.Vaniglia", "Vaniglia", "Latte, Uova", ""],
 ["Gel.Limone",   "Limone",   "",            ""]
].map(/* lascia invariata la parte finale già presente nel codice */);
```

### 5. Immagini

Le foto sono facoltative. Per ogni piatto il sito cerca, in ordine:

1. `img/<cartella-categoria>/<nome-piatto>.jpg`
2. `img/<nome-piatto>.jpg`

Se non trova nulla, mostra la lettera iniziale del nome.

- Il nome del file è il nome del piatto **in minuscolo, senza accenti e con i trattini al posto di spazi e simboli**. Esempio: `Pasta al ragù` → `pasta-al-ragu.jpg`.
- La cartella di ogni categoria si definisce in `SUBDIR`, che associa il nome della categoria a una sottocartella di `img/`. Aggiungi una voce per ogni tua categoria:

```js
var SUBDIR = {"Primi": "Primi", "Secondi": "Secondi", "Bevande": "Bevande"};
```

- Per le varianti, il sito cerca `img/dolci/<nome-variante>.jpg` e poi `img/<nome-variante>.jpg`, oppure usa il percorso scritto nel quarto campo.
- Consigli: foto leggere (circa 50–100 KB l'una), in formato `.jpg`, tutte con lo stesso taglio.
- Alcune bevande hanno già un'icona integrata, legata a specifici `id`. Se non c'è né icona né foto, si usa la lettera iniziale.

---

## 🔲 Formato del QR

Il QR contiene una stringa compatta, solo ASCII:

```
SD26|<ID_ORDINE>|Copertox<N>,<id_piatto>x<qta>,...|<totale>
```

Esempio:

```
SD26|K3F9A2XYZ|Copertox2,Pasta.Ragx2,Acquax2|16.00
```

| Parte | Significato |
|---|---|
| `SD26` | Prefisso di riconoscimento: serve al lettore per capire che il QR è un ordine di questo sito. È scritto nel codice (cerca `"SD26\|"` in `index.html`) e può essere cambiato, purché il lettore sia aggiornato di conseguenza. |
| `<ID_ORDINE>` | Codice casuale che identifica l'ordine. |
| `Copertox<N>` | Numero di coperti. |
| `<id_piatto>x<qta>` | Ogni piatto con la quantità, usando gli `id` del `MENU`. |
| `<totale>` | Totale calcolato dal sito, con due decimali. |

### 🧾 Come leggerlo in cassa

Per usare il QR serve un **lettore** (un programma di cassa, un foglio di calcolo, uno script…) che:

1. legga il testo del QR (va bene anche un lettore a pistola che "scrive" da tastiera);
2. lo divida con il carattere `|` e poi con `,`;
3. cerchi ogni `id` nel **proprio listino** e ne usi il prezzo.

> ⚠️ **Importante:** non fidarti del totale scritto nel QR. Chiunque può copiare il sito, cambiare i prezzi e generare QR diversi. Il lettore deve **ricalcolare sempre il totale dal proprio listino**, usando dal QR solo gli `id` e le quantità. Il totale nel QR è utile al massimo come controllo di riferimento.

Tieni gli `id` del sito e quelli del listino di cassa **identici**, e aggiornali insieme ogni volta che modifichi il menù.

---

## ✅ Checklist prima di usarlo

- [ ] Titolo, nomi, contatti e link social sostituiti.
- [ ] Prezzi e allergeni controllati insieme a chi prepara i piatti.
- [ ] Riga `Coperto` presente e con il prezzo giusto.
- [ ] Foto presenti per tutti i piatti (o scelta consapevole di lasciare la lettera iniziale).
- [ ] Prova completa: ordine dal telefono → QR → lettura in cassa → totale corretto.
- [ ] Prova con il lettore QR che userai davvero: alcuni lettori da tastiera alterano i caratteri speciali.
- [ ] `WHATSAPP` impostato oppure lasciato vuoto.
- [ ] Sito raggiungibile dai telefoni dei clienti con la rete disponibile sul posto.

---

## 🛠️ Tecnologie

- HTML5, CSS3 e JavaScript puro, senza framework
- Libreria per QR code integrata nella pagina
- Google Fonts (*Alfa Slab One* e *Barlow Condensed*)

---

## 📌 Limiti da conoscere

- **Il carrello non viene salvato**: se si ricarica o si chiude la pagina, l'ordine in corso si perde. Il sito avvisa prima, ma sui telefoni l'avviso del browser non è garantito.
- **Non c'è un backend**: il sito non riceve gli ordini, li affida al QR. Per riceverli a distanza serve una parte server, qui non prevista.
- **Prezzi e menù sono nel codice**: per cambiarli bisogna modificare `index.html` e ripubblicare.
- **Dipende da internet**: i font vengono caricati da Google Fonts.

---

## 📄 Licenza

Aggiungi qui la licenza che preferisci (ad esempio MIT) se vuoi che altri possano riutilizzare il progetto.
