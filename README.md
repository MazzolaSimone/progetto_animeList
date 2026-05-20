# AnimeList

Applicazione web che permette di cercare e visualizzare anime, sviluppata come progetto scolastico.

## Descrizione

AnimeList è una Single Page Application (SPA) che permette di:
- Visualizzare i 24 anime più popolari al caricamento della pagina
- Cercare anime per nome tramite una barra di ricerca
- Visualizzare la scheda dettagliata di un anime cliccandoci sopra (titolo, valutazione, generi, studio, sinossi, episodi, stato)

## Tecnologie usate

- Vue.js — framework JavaScript per la costruzione dell'interfaccia
- CSS — per la grafica e il layout responsive
- Jikan API v4 — API pubblica e gratuita di MyAnimeList per i dati degli anime

## API

Viene usata la [Jikan API v4](https://jikan.moe/), che non richiede registrazione né chiave API.

Endpoint usati:
- `GET https://api.jikan.moe/v4/top/anime?limit=24` — carica i top anime all'avvio
- `GET https://api.jikan.moe/v4/anime?q=PAROLA_CHIAVE&limit=24` — ricerca per nome

## Struttura del progetto
animelist/
├── src/
│   ├── App.vue               # Componente principale
│   ├── components/
│   │   ├── RicercaAnime.vue  # Barra di ricerca e griglia risultati
│   │   └── SchedaAnime.vue   # Scheda dettaglio anime
│   └── main.js               # Punto di ingresso

## Come avviare il progetto

```bash
cd animelist
npm install
npm run serve
```
