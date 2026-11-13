# 🌿 Menù online – Sagra del Sedano

Sito web del **menù online della 45ª Sagra del Sedano**, organizzata da **Alluvioni Cambiò** (*Insieme per Alluvioni APS*).

Permette ai clienti di consultare il menù dal proprio smartphone, scegliere piatti e quantità e generare un **codice QR dell'ordine** da mostrare in cassa. In questo modo si riducono le code e gli errori di trascrizione.

---

## 🎯 A cosa serve

Il sito risolve un problema pratico durante la sagra: **velocizzare la presa degli ordini**.

1. Il cliente apre il sito dal telefono (ad esempio inquadrando un QR code sul tavolo).
2. Indica quanti **coperti** servono (campo obbligatorio).
3. Sfoglia il menù per categoria o cerca un piatto, e aggiunge le quantità desiderate.
4. Controlla il riepilogo e il totale, poi preme **"Conferma ordine e genera QR"**.
5. Mostra il QR in cassa (consigliato uno screenshot): la cassa legge i dati dell'ordine e lo registra.

> ℹ️ Il QR contiene i dati dell'ordine ma **non certifica il pagamento**, che avviene in cassa.

---

## ✨ Funzionalità

- **Menù completo** diviso in categorie: Antipasti, Primi piatti, Secondi piatti, Contorni, Dolci, Bevande.
- **Ricerca** di un piatto e filtro per categoria.
- **Foto dei piatti** (indicative) con prezzo e note.
- **Allergeni**: icona "i" su ogni piatto con l'elenco dei 14 allergeni principali (più funghi e mais), con icone dedicate.
- **Scelta del gusto** per gelato e torta, con foto e allergeni di ogni gusto.
- **Gestione coperti** e calcolo automatico del **totale** dell'ordine.
- **Generazione del QR** direttamente nel browser, senza server e senza connessione per la parte di calcolo.
- **Invio opzionale su WhatsApp** del riepilogo ordine.
- **Chiusura ordini** con un semplice interruttore, quando non si vogliono più ricevere ordini online.
- **Design responsive**, pensato per l'uso da smartphone.

---

## 📁 Struttura del progetto

```
.
├── index.html        # Pagina unica: HTML, CSS e JavaScript (inclusa la libreria QR)
└── img/              # Foto dei piatti, divise per categoria
    ├── Antipasti/
    ├── Primi/
    ├── Secondi/
    ├── Contorni/
    └── Dolci/
```

Il sito è **statico**: non richiede backend, database o build. L'unica risorsa esterna sono i font di Google Fonts (*Alfa Slab One* e *Barlow Condensed*).

---

## 🚀 Come usarlo

### In locale

Basta aprire `index.html` con un browser.

### Pubblicazione con GitHub Pages

1. Carica i file nella repository.
2. Vai su **Settings → Pages**.
3. In *Source* scegli il branch (es. `main`) e la cartella `/ (root)`.
4. Dopo qualche minuto il sito sarà online all'indirizzo che GitHub ti mostrerà.

---

## ⚙️ Personalizzazione

Tutte le impostazioni si trovano all'interno di `index.html`, nella sezione `CONFIGURAZIONE`.

| Cosa | Dove | Come |
|---|---|---|
| Aprire/chiudere gli ordini | `ORDINI_APERTI` | `true` = aperti, `false` = chiusi (viene mostrato un avviso che rimanda alla cassa) |
| Numero WhatsApp | `WHATSAPP` | Numero con prefisso, senza `+` (es. `"393331234567"`). Vuoto = pulsante nascosto |
| Piatti e prezzi | array `MENU` | Formato `[categoria, id, nome, prezzo, nota, allergeni]` |
| Gusti del gelato | array `GUSTI` | Formato `[id, nome, allergeni, immagine]` |
| Gusti della torta | array `GUSTI_TORTA` | Stesso formato dei gusti del gelato |
| Link social | sezione `social` | Sostituire i `#` negli `href` con gli indirizzi delle pagine |

**Esempio di piatto:**

```js
["Antipasti", "Ins.Sedano", "Insalata di sedano", 4, "", "Sedano"],
```

**Immagini:** per ogni piatto il sito cerca `img/<Categoria>/<nome-piatto>.jpg` (il nome è in minuscolo, senza accenti e con i trattini al posto degli spazi). Se la foto non esiste, viene mostrata la lettera iniziale del piatto.

---

## 🔲 Formato del QR

Il QR contiene una stringa compatta, solo ASCII, pensata per essere letta dal sistema di cassa:

```
SD26|<ID_ORDINE>|Copertox<N>,<id_piatto>x<qta>,...|<totale>
```

Esempio:

```
SD26|K3F9A2XYZ|Copertox2,Bagnax2,Agn.Sugox2,Pol.Brasatox1|35.00
```

---

## 🛠️ Tecnologie

- HTML5, CSS3 e JavaScript puro (nessun framework)
- Libreria QR code integrata nella pagina
- Google Fonts

---

## 📬 Contatti

- **Insieme per Alluvioni APS** – ass.insiemeperalluvioni@gmail.com
- **Realizzato da** Forsinetti Diego – forsinetti.diego@gmail.com
