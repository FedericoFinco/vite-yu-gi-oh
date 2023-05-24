<script >
import axios from 'axios'
import { store } from '../src/store';

export default {
  name: "App",
  components: {
    
  },
  data() {
    return {
      store
    }
  },
  mounted() {
    // console.log("App Mounted, store: ", this.store);

    //Al caricamento della pagina effettuo una chiamata ad una API
    axios.get(this.store.urlAPI).then(r => {
      //Se completata correttamente stampo il dato
      console.log("Ricevuto: ", r);
      this.store.ListaCarte = r.data.data
      console.log("Ricevuto: ", this.store.ListaCarte)
      //Copio i dati nello store per riutilizzarli in altri componenti
      // this.store.personaggi = r.data.results;
      //Imposto lo stato "loading" dell'applicazione su false
      this.store.loading = false;
    }).catch(errore => {
      //In caso di problemi, mostro l'errore in console
      console.error("Qualcosa è andato storto", errore);
      //Mi assicuro che il dato nello store sia un array vuoto
      // this.store.personaggi = [];
      //Il caricamento è comunque finito anche in questo caso
      // this.store.loading = false;
    });
  }
}


</script>

<template>
  <h1>prova</h1>
  <div v-if="store.loading == false" class="container wrapper">
    <div class="card" v-for="(carta,i) in this.store.ListaCarte">

      <div class="background">
        <img :src="store.ListaCarte[i].card_images[0].image_url" alt="">
        <p>
          {{ store.ListaCarte[i].name }}
        </p>
        <p>
          {{ store.ListaCarte[i].archetype }}
        </p>
      </div>

    </div>
  </div>
  
</template>

<style lang="scss" scoped>
@use './styles/general.scss' as *;
@use './styles/partials/mixins.scss' as *;
.wrapper{
    @include flex(row, space-between,flex-start ,wrap);
    .background{
      background-color: orangered;
      height: 400px;
    }
    .card{
      @include flex(column,center,flex-start);
      width: calc(100% / 5);
      margin-top: 2rem;
      padding: 1rem;
      p{
        margin-top: 1rem;
      }
      img{
        width: 100%;
      }

    }
}

</style>
