# SnapQuote — la pagina che vende

Due pagine statiche, niente da compilare e niente da installare: si aprono con
un doppio clic e si pubblicano copiandole. Sono servite da GitHub Pages su
`https://riccardogheobeo07-max.github.io/snapquote/`.

| file | cos'è |
| --- | --- |
| `index.html` | tutto: cosa fa lo strumento, come funziona, i piani, il modulo per l'audit gratuito. Il foglio di stile e il programma stanno dentro la pagina |
| `grazie.html` | dove arriva chi ha appena pagato. Marcata `noindex` |
| `immagini/` | schermate vere dell'estensione, **generate**: vedi sotto |
| `llms.txt` `robots.txt` `sitemap.xml` | quello che leggono i motori e gli assistenti IA |

## Le tre cose da sapere prima di toccarla

**1. Le immagini non si modificano a mano.** Nascono da
`parita/vetrina.py` nel repository del tool, che apre l'estensione vera con
dei dati di prova e la fotografa. Per rifarle:

```bash
python parita/vetrina.py --scatta
```

Le riscatta, le mette in `parita/immagini/` per il Chrome Web Store e ne copia
in `immagini/` le cinque che servono qui. Una copia rifatta a mano si
dimentica: è già successo, e nella schermata più grande della pagina è rimasto
per un po' il nome precedente del prodotto — cioè proprio dove il cambio di
nome doveva vedersi per primo.

**2. Le favicon nemmeno.** Le disegna `parita/icone.py` e le riscrive dentro le
due pagine `parita/rinfresca_marchio.py`. Icona dell'estensione, marchio della
vetrina e favicon del sito sono lo stesso disegno: sono già finiti a
raccontare tre cose diverse, e quello script è la rete che lo impedisce.

**3. Prima di pubblicare, si verifica.** Dal repository del tool:

```bash
python parita/verifica_landing.py
```

Controlla quello che si rompe in silenzio: una classe scritta nel markup e mai
definita, una regola che non usa nessuno, un `href="#qualcosa"` che non porta
da nessuna parte, un'immagine con `width`/`height` diversi dalla sua misura
vera (il browser tiene il posto sbagliato e la pagina salta sotto le dita di
chi legge), un'immagine senza `alt`, una schermata rimasta indietro rispetto
alla vetrina.

E le decisioni che senza un controllo tornano indietro da sole al primo
copia-e-incolla: niente maiuscoletto, niente tema scuro, il nome vecchio
sparito, la firma dell'autore in fondo, i prezzi dei cartellini uguali a
quelli dei dati strutturati, il titolo che promette «nove cose» e sotto ne ha
nove.

Il controllo stesso è messo alla prova da `parita/prova_verifica_landing.py`,
che fa una copia usa e getta del sito, la rompe in trentatré modi diversi e
pretende che il guardiano se ne accorga ogni volta. Tutt'e due girano dentro
`python verifica_tutto.py`.

## Il proprio audit

La pagina vende un audit SEO, quindi deve passare il proprio. Si serve il sito
in locale — da questa cartella:

```bash
python -m http.server 8861
```

e lo si analizza con lo strumento vero, dalla cartella del tool:

```bash
python audit.py http://127.0.0.1:8861/index.html
```

Tre rilievi restano e sono del server di prova, non della pagina: HTTPS,
compressione e il tempo di risposta. Su GitHub Pages non esistono.

## Il tema è chiaro e basta

Come il popup. C'era un tema scuro, ed è stato tolto: chi ha il computer al
buio vedeva un sito scuro, installava l'estensione e gli si apriva un popup
bianco. Il sito prometteva un prodotto diverso da quello consegnato.

## Il pagamento

I bottoni dei piani funzionano in tre modi che scalano uno sull'altro, e il
codice in fondo a `index.html` lo spiega per esteso: il server delle licenze
se c'è, altrimenti dei link di pagamento incollati a mano, altrimenti il
modulo di contatto. In nessuno dei tre casi i dati della carta passano da
questo sito.

## L'indirizzo

Il sito sta su `https://riccardogheobeo07-max.github.io/snapquote/`, e quello
stesso indirizzo è scritto in sette posti: il canonical, `og:url`, `og:image`,
`twitter:image`, i dati strutturati, la `sitemap.xml` e il `robots.txt`. Se due
di quei posti dicessero indirizzi diversi, Google seguirebbe quello sbagliato e
la pagina sparirebbe dai risultati senza nessun errore visibile da nessuna
parte: per questo `verifica_landing.py` li confronta tutti a ogni verifica.

Il repository si chiamava `audit-shopify` fino al cambio di nome del prodotto.
Attenzione a cosa sopravvive e cosa no: GitHub rimanda dal nome vecchio del
**repository**, ma **non** dalle Pages. Misurato subito dopo la rinomina:

    https://riccardogheobeo07-max.github.io/audit-shopify/  ->  HTTP 404

Qui non e' costato niente, perche' il vecchio indirizzo non era mai stato dato
a nessuno. Ma se un giorno il sito avra' un dominio suo e dei visitatori, un
cambio di nome va accompagnato da un rimando fatto a mano.

---

Di Riccardo Gheoni.
