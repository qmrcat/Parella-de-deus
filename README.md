# Parelles de Deu

Joc de números per al navegador. L'objectiu és buidar el tauler unint parelles de números iguals, que sumin 10 o que sumin la **suma extra** de cada fase.

Tot el joc és un únic fitxer HTML amb JavaScript i CSS escrits a mà. No fa servir cap framework ni cap llibreria, i no cal compilar ni instal·lar res.

## Com es juga

- Tria dos números **iguals** (7 i 7) o que **sumin 10** (3 i 7).
- Cada fase té també una **suma extra** triada a l'atzar. Dos números que sumin aquest valor també fan parella, amb les mateixes regles que el 10.
- Han d'estar **connectats**:
  - al costat en horitzontal, en vertical o en diagonal;
  - o bé l'últim número d'una fila amb el primer de la fila següent.

  Les caselles ja ratllades no fan de barrera: una parella es pot unir per sobre d'elles.
- Quan una fila queda buida, desapareix i les de sota pugen.
- Si no trobes cap parella, el botó **Afegeix** copia al final del tauler tots els números que queden.
- Si buides tot el tauler, passes a la fase següent. La partida s'acaba quan no queden parelles ni usos d'Afegeix.

Parelles que sumen 10: `1+9`, `2+8`, `3+7`, `4+6`, `5+5`.

### La suma extra

- **Quin valor pot ser:** un número del 2 al 9 o de l'11 al 18, triat a l'atzar al començament de cada fase. Dues fases seguides no repeteixen mai la mateixa suma extra.
- **On es veu:** al marcador, a la frase de sota el títol i a l'apartat «Com es juga». Aquest apartat també llista les parelles que la sumen.
- **Com compta:** una parella que suma la suma extra dona els mateixos punts que una que suma 10.
- **Exemple:** si la suma extra és 13, també fan parella `4+9`, `5+8` i `6+7`.
- **Casos límit:** amb 2 (`1+1`) o amb 18 (`9+9`), l'única parella possible ja era vàlida per ser de números iguals.

### Puntuació

| Acció | Punts |
|---|---|
| Parella veïna | 1 |
| Parella separada per caselles ratllades | 4 |
| Fila buidada | 10 |
| Tauler net (fase superada) | 150 |

### Límits per fase

- **Afegeix:** 5 usos.
- **Pista:** 3 usos. Cada pista marca una parella vàlida.

### Mida del tauler

Cada fase comença amb un tauler de 9 columnes:
- fase 1: 27 números;
- fase 2: 36 números;
- fase 3 i següents: 45 números.

Els números es generen a l'atzar. El joc torna a generar el tauler fins que té entre 3 i 12 parelles disponibles d'entrada, tenint en compte la suma extra.

## Botons

| Botó | Què fa |
|---|---|
| **Demo** | Mostra una demo guiada de 12 passos, d'aproximadament un minut. Hi surt també la suma extra (en la demo, el 13). |
| **Afegeix** | Copia els números que queden al final del tauler. |
| **Pista** | Marca una parella possible. |
| **Nova** | Comença una partida nova. Si n'hi ha una en curs, cal tocar-lo dues vegades per confirmar. |

**Detalls de la demo:**
- Juga sola sobre un tauler preparat: un cercle blau marca on toca i un requadre explica cada jugada.
- No modifica la partida desada.
- Se'n surt amb «Surt de la demo», amb la tecla Esc o, al final, amb «Comença a jugar».

**Teclat:**
- Totes les caselles i els botons es poden fer servir amb el teclat (Tab, Enter o espai).
- Esc treu la selecció, o surt de la demo si està en marxa.

## Fitxers

| Fitxer | Per a què serveix |
|---|---|
| `parelles-de-deu-autonom.html` | Document HTML complet. És el que cal fer servir per obrir el joc o penjar-lo a qualsevol servidor. |
| `parelles-de-deu.html` | Codi de la pàgina publicada a Claude. No porta `<!doctype>`, `<html>`, `<head>` ni `<body>`, perquè la plataforma els afegeix en publicar-la. |
| `README.md` | Aquest document. |

## Com fer-lo servir

- **En local:** obre `parelles-de-deu-autonom.html` amb el navegador. No cal cap servidor.
- **A internet:** copia el fitxer a qualsevol allotjament estàtic (GitHub Pages, Netlify, el servidor d'una escola…). Si el reanomenes `index.html`, s'obrirà directament en entrar a la carpeta.

## Detalls tècnics

- **Sense dependències:** HTML, CSS i JavaScript sense frameworks ni llibreries.
- **Fonts:** les úniques peces externes són dues fonts de Google Fonts, *Bricolage Grotesque* per als números i els títols i *Figtree* per al text. Sense connexió, el joc funciona igual amb les fonts del sistema.
- **Partida desada:** la partida (inclosa la suma extra de la fase) i el rècord es guarden al `localStorage` del navegador, amb la clau `parelles-de-deu-v1`.
  - Si tanques la pàgina, en tornar-hi continues on eres.
  - Les dades només queden en aquell navegador i en aquell dispositiu.
  - En una finestra privada o amb l'emmagatzematge bloquejat, el joc funciona igual però no recorda res.
- **Tema clar i fosc:** el joc segueix automàticament la preferència del sistema.
- **Adaptable:** funciona en mòbil i en ordinador.
- **Accessibilitat:** respecta l'opció del sistema de reduir les animacions.

### Com es comprova si dues caselles estan connectades

El tauler és una llista lineal de caselles, que es mostra en files de 9. Per a cada casella, el joc busca cap endavant la primera casella no ratllada en quatre direccions:

| Direcció | Salt d'índex |
|---|---|
| Seguit (inclou el pas de final de fila a inici de la següent) | +1 |
| Vertical | +9 |
| Diagonal cap a la dreta | +10, sense sortir per la vora dreta |
| Diagonal cap a l'esquerra | +8, sense sortir per la vora esquerra |

Dues caselles estan connectades si una és la primera casella oberta que es troba des de l'altra en alguna d'aquestes direccions.

### Paràmetres que es poden canviar

Al principi del `<script>` hi ha aquestes constants:

```js
const COLS = 9;   // columnes del tauler
const ADDS = 5;   // usos d'Afegeix per fase
const HINTS = 3;  // pistes per fase
```

- **Sumes extra possibles:** la llista `TARGETS`.
- **Mida del tauler:** la calcula la funció `stageSize(stage)`.
- **Punts:** s'assignen a les funcions `tap()`, `afterMatch()` i `check()`.
- **Demo:** el tauler, la suma extra i els passos són a `DEMO_CELLS`, `DEMO_TARGET` i `DEMO_STEPS`.

## Crèdits

Joc inspirat en *Number Match* d'Easybrain (https://easybrain.com/numbermatch). És un projecte independent, sense cap relació amb Easybrain.

La suma extra, la puntuació, els límits d'Afegeix i de pistes, la mida dels taulers i la demo són decisions pròpies d'aquesta versió. No reprodueixen necessàriament el funcionament de l'aplicació original.
