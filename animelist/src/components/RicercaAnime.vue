<template>
  <div>
    <!-- Barra di ricerca -->
    <div class="barra-ricerca">
      <input
        v-model="parolaChiave"
        @keyup.enter="cercaAnime"
        type="text"
        placeholder="Cerca un anime... (es. Naruto, One Piece)"
        class="input-ricerca"
      />
      <button @click="cercaAnime" class="bottone-cerca">Cerca</button>
    </div>

    <!-- Messaggio di caricamento -->
    <div v-if="caricamento" class="messaggio-stato">
       Caricamento in corso...
    </div>

    <!-- Messaggio errore -->
    <div v-if="errore" class="messaggio-errore">
      X {{ errore }}
    </div>

    <!-- Griglia risultati -->
    <div v-if="listaAnime.length > 0" class="griglia-anime">
      <div
        v-for="anime in listaAnime"
        :key="anime.mal_id"
        class="carta-anime"
        @click="selezionaAnime(anime)"
      >
        <img
          :src="anime.images.jpg.image_url"
          :alt="anime.title"
          class="copertina"
        />
        <div class="info-carta">
          <h3 class="nome-anime">{{ anime.title }}</h3>
          <span class="valutazione" v-if="anime.score">{{ anime.score }}</span>
          <span class="valutazione" v-else>N/D</span>
        </div>
      </div>
    </div>

    <!-- Nessun risultato -->
    <div v-if="ricercaEffettuata && listaAnime.length === 0 && !caricamento" class="messaggio-stato">
       Nessun anime trovato per "{{ parolaChiave }}"
    </div>
  </div>
</template>

<script>
export default {
  name: 'RicercaAnime',
  data() {
    return {
      parolaChiave: '',
      listaAnime: [],
      caricamento: false,
      errore: null,
      ricercaEffettuata: false
    }
  },
  async mounted() {
    await this.caricaAnimeTop()
  },
  methods: {
    async caricaAnimeTop() {
      this.caricamento = true
      this.errore = null
      try {
        const risposta = await fetch('https://api.jikan.moe/v4/top/anime?limit=24')
        const dati = await risposta.json()
        this.listaAnime = dati.data
      } catch (err) {
        this.errore = 'Impossibile caricare gli anime. Riprova più tardi.'
      } finally {
        this.caricamento = false
      }
    },
    async cercaAnime() {
      if (!this.parolaChiave.trim()) {
        await this.caricaAnimeTop()
        this.ricercaEffettuata = false
        return
      }
      this.caricamento = true
      this.errore = null
      this.ricercaEffettuata = true
      try {
        const risposta = await fetch(
          `https://api.jikan.moe/v4/anime?q=${encodeURIComponent(this.parolaChiave)}&limit=24`
        )
        const dati = await risposta.json()
        this.listaAnime = dati.data
      } catch (err) {
        this.errore = 'Errore durante la ricerca. Riprova.'
        this.listaAnime = []
      } finally {
        this.caricamento = false
      }
    },
    selezionaAnime(anime) {
      this.$emit('anime-selezionato', anime)
    }
  }
}
</script>

<style scoped>
.barra-ricerca {
  display: flex;
  gap: 12px;
  margin-bottom: 30px;
  justify-content: center;
}

.input-ricerca {
  flex: 1;
  max-width: 600px;
  padding: 14px 20px;
  font-size: 1rem;
  border: 2px solid #cc0000;
  border-radius: 30px;
  background-color: #1a1a1a;
  color: #f0f0f0;
  outline: none;
  transition: border-color 0.3s, box-shadow 0.3s;
}

.input-ricerca:focus {
  border-color: #ff4444;
  box-shadow: 0 0 12px rgba(255, 68, 68, 0.4);
}

.input-ricerca::placeholder {
  color: #888;
}

.bottone-cerca {
  padding: 14px 28px;
  background: linear-gradient(135deg, #cc0000, #ff1a1a);
  color: white;
  border: none;
  border-radius: 30px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
}

.bottone-cerca:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(204, 0, 0, 0.5);
}

.bottone-cerca:active {
  transform: translateY(0);
}

.griglia-anime {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 20px;
}

.carta-anime {
  background-color: #1a1a1a;
  border-radius: 12px;
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.25s, box-shadow 0.25s;
  border: 1px solid #2a2a2a;
}

.carta-anime:hover {
  transform: translateY(-6px) scale(1.02);
  box-shadow: 0 10px 30px rgba(204, 0, 0, 0.4);
  border-color: #cc0000;
}

.copertina {
  width: 100%;
  height: 240px;
  object-fit: cover;
  display: block;
}

.info-carta {
  padding: 12px;
}

.nome-anime {
  font-size: 0.9rem;
  font-weight: 700;
  color: #f0f0f0;
  margin-bottom: 6px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.valutazione {
  font-size: 0.85rem;
  color: #ffd700;
}

.messaggio-stato {
  text-align: center;
  padding: 40px;
  font-size: 1.1rem;
  color: #888;
}

.messaggio-errore {
  text-align: center;
  padding: 20px;
  font-size: 1rem;
  color: #ff4444;
  background-color: #1a0000;
  border-radius: 8px;
  margin-bottom: 20px;
}
</style>