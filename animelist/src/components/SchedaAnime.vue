<template>
  <div class="overlay" @click.self="$emit('chiudi')">
    <div class="scheda">
      <!-- Bottone chiudi -->
      <button class="bottone-chiudi" @click="$emit('chiudi')">✕</button>

      <div class="scheda-contenuto">
        <!-- Immagine -->
        <div class="colonna-immagine">
          <img
            :src="anime.images.jpg.large_image_url || anime.images.jpg.image_url"
            :alt="anime.title"
            class="immagine-grande"
          />
          <div class="punteggio" v-if="anime.score">
             {{ anime.score }} / 10
          </div>
        </div>

        <!-- Dettagli -->
        <div class="colonna-dettagli">
          <h2 class="titolo-anime">{{ anime.title }}</h2>
          <p class="titolo-giapponese" v-if="anime.title_japanese">{{ anime.title_japanese }}</p>

          <div class="separatore"></div>

          <div class="riga-info">
            <span class="etichetta">Tipo</span>
            <span class="valore">{{ anime.type || 'N/D' }}</span>
          </div>

          <div class="riga-info">
            <span class="etichetta">Episodi</span>
            <span class="valore">{{ anime.episodes || 'In corso' }}</span>
          </div>

          <div class="riga-info">
            <span class="etichetta">Anno</span>
            <span class="valore">{{ anime.year || (anime.aired && anime.aired.prop && anime.aired.prop.from && anime.aired.prop.from.year) || 'N/D' }}</span>
          </div>

          <div class="riga-info">
            <span class="etichetta">Stato</span>
            <span class="valore">{{ traduttoreStato(anime.status) }}</span>
          </div>

          <div class="riga-info" v-if="anime.studios && anime.studios.length > 0">
            <span class="etichetta">Studio</span>
            <span class="valore">{{ anime.studios.map(s => s.name).join(', ') }}</span>
          </div>

          <div class="riga-info" v-if="anime.genres && anime.genres.length > 0">
            <span class="etichetta">Generi</span>
            <div class="lista-generi">
              <span
                v-for="genere in anime.genres"
                :key="genere.mal_id"
                class="etichetta-genere"
              >
                {{ genere.name }}
              </span>
            </div>
          </div>

          <div class="separatore"></div>

          <div class="sinossi" v-if="anime.synopsis">
            <p class="etichetta">Sinossi</p>
            <p class="testo-sinossi">{{ anime.synopsis }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SchedaAnime',
  props: {
    anime: {
      type: Object,
      required: true
    }
  },
  emits: ['chiudi'],
  methods: {
    traduttoreStato(stato) {
      const stati = {
        'Finished Airing': 'Completato',
        'Currently Airing': 'In corso',
        'Not yet aired': 'Non ancora iniziato'
      }
      return stati[stato] || stato || 'N/D'
    }
  }
}
</script>

<style scoped>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.85);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 20px;
  backdrop-filter: blur(4px);
}

.scheda {
  background: #1a1a1a;
  border-radius: 16px;
  max-width: 900px;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  position: relative;
  border: 2px solid #cc0000;
  box-shadow: 0 0 40px rgba(204, 0, 0, 0.4);
}

.bottone-chiudi {
  position: sticky;
  top: 12px;
  float: right;
  margin: 12px 12px 0 0;
  background: #cc0000;
  color: white;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  font-size: 1rem;
  cursor: pointer;
  z-index: 10;
  transition: background 0.2s, transform 0.2s;
}

.bottone-chiudi:hover {
  background: #ff1a1a;
  transform: scale(1.1);
}

.scheda-contenuto {
  display: flex;
  gap: 24px;
  padding: 24px;
  clear: both;
}

.colonna-immagine {
  flex-shrink: 0;
  width: 220px;
}

.immagine-grande {
  width: 100%;
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.5);
}

.punteggio {
  text-align: center;
  margin-top: 10px;
  font-size: 1.3rem;
  color: #ffd700;
  font-weight: bold;
}

.colonna-dettagli {
  flex: 1;
}

.titolo-anime {
  font-size: 1.7rem;
  font-weight: 900;
  color: #ff4444;
  line-height: 1.2;
  margin-bottom: 4px;
}

.titolo-giapponese {
  color: #888;
  font-size: 0.95rem;
  margin-bottom: 8px;
}

.separatore {
  height: 2px;
  background: linear-gradient(to right, #cc0000, transparent);
  margin: 16px 0;
  border-radius: 2px;
}

.riga-info {
  display: flex;
  align-items: flex-start;
  gap: 10px;
  margin-bottom: 10px;
}

.etichetta {
  font-size: 0.85rem;
  color: #cc0000;
  font-weight: 700;
  min-width: 110px;
}

.valore {
  font-size: 0.9rem;
  color: #e0e0e0;
}

.lista-generi {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.etichetta-genere {
  background: #330000;
  border: 1px solid #cc0000;
  color: #ff6666;
  padding: 3px 10px;
  border-radius: 20px;
  font-size: 0.8rem;
}

.sinossi {
  margin-top: 4px;
}

.testo-sinossi {
  font-size: 0.88rem;
  color: #cccccc;
  line-height: 1.6;
  margin-top: 8px;
  max-height: 180px;
  overflow-y: auto;
  padding-right: 4px;
}

/* Scrollbar stilizzata */
.testo-sinossi::-webkit-scrollbar,
.scheda::-webkit-scrollbar {
  width: 5px;
}

.testo-sinossi::-webkit-scrollbar-track,
.scheda::-webkit-scrollbar-track {
  background: #111;
}

.testo-sinossi::-webkit-scrollbar-thumb,
.scheda::-webkit-scrollbar-thumb {
  background: #cc0000;
  border-radius: 4px;
}

/* Responsive */
@media (max-width: 600px) {
  .scheda-contenuto {
    flex-direction: column;
  }
  .colonna-immagine {
    width: 100%;
  }
  .immagine-grande {
    max-height: 300px;
    object-fit: cover;
  }
}
</style>