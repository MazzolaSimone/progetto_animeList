<template>
  <div id="app">
    <header class="intestazione">
      <h1 class="titolo-sito">AnimeList</h1>
      <p class="sottotitolo">Cerca un anime</p>
    </header>

    <main class="contenuto-principale">
      <RicercaAnime @anime-selezionato="mostraScheda" />
    </main>

    <SchedaAnime
      v-if="animeSelezionato"
      :anime="animeSelezionato"
      @chiudi="animeSelezionato = null"
    />
  </div>
</template>

<script>
import RicercaAnime from './components/RicercaAnime.vue'
import SchedaAnime from './components/SchedaAnime.vue'

export default {
  name: 'App',
  components: {
    RicercaAnime,
    SchedaAnime
  },
  data() {
    return {
      animeSelezionato: null
    }
  },
  methods: {
    mostraScheda(anime) {
      this.animeSelezionato = anime
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #0d0d0d;
  color: #f0f0f0;
  font-family: 'Segoe UI', sans-serif;
}

#app {
  min-height: 100vh;
}

.intestazione {
  background: linear-gradient(135deg, #8b0000, #cc0000, #ff1a1a);
  padding: 40px 20px;
  text-align: center;
  box-shadow: 0 4px 20px rgba(204, 0, 0, 0.5);
}

.titolo-sito {
  font-size: 3rem;
  font-weight: 900;
  letter-spacing: 3px;
  text-shadow: 0 0 20px rgba(255, 100, 100, 0.8);
  color: #fff;
}

.sottotitolo {
  margin-top: 8px;
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.8);
}

.contenuto-principale {
  max-width: 1200px;
  margin: 0 auto;
  padding: 30px 20px;
}
</style>