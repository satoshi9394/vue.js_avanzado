<template lang="pug">
  main

    transition(name="move")
      pm-notification(v-bind:notification="showNotification")
        p(slot="body") {{ searchMessage1 }}

    transition(name="move")
      pm-loader(v-show="isLoading")

    section.section(v-show="!isLoading")
      nav.nav
        .container
          input.input.is-large(
            type="text", 
            placeholder="Buscar canciones", 
            v-model="searchQuery",
            v-on:keyup.enter="search", alt="una estencion de la funcionalidad de vue"
            )
          a.button.is-info.is-large(@click="search") Buscar
          a.button.is-danger.is-large &times; 
      .container    
        p
          small {{ searchMessage2 }}
      .container.result
        .columns.is-multiline
         .column.is-one-quarter(v-for="t in tracks")
          pm-Track(
            v-blur="t.preview_url"
            v-bind:class= "{ 'is-active': t.id === selectedTrack  }",
            v-bind:track="t", 
            v-on:select="setSelectedTrack"
            )
</template>

<script>
import trackService from '@/services/track';

import PmTrack  from '@/components/Track.vue';

import PmLoader from '@/components/shared/Loader.vue';
import PmNotification from '@/components/shared/Notification.vue';


export default {
  name: 'App',

  components: { PmTrack, PmLoader, PmNotification} ,

  data () {
    return {
      searchQuery: '',
      tracks: [],


      isLoading: false,
      showNotification: false,

      selectedTrack : '',

    }
  },

  computed:{
    searchMessage1 () {
      if (this.tracks.length >= 1) {
        return `Se encontraron: ${ this.tracks.length }`
      }else {
        return `No se han encontrado resultados`
      }

    },
    searchMessage2 () {
      return `Encontrados: ${ this.tracks.length }`
    } 

  },

  watch: {
    showNotification () {
      if (this.showNotification) {
        setTimeout( () => {
          this.showNotification = false
        }, 3000 )
      }
    }
  },

  methods: {
    search () {
      if (this.searchQuery === '') { return }
      this.isLoading = true

      trackService.search(this.searchQuery)
        .then(res => {
            this.showNotification = res.tracks.total === 0
            this.tracks = res.tracks.items
            this.isLoading= false
        })
    },
    setSelectedTrack (id) {
      this.selectedTrack = id
    }
  },
}
</script>

<style lang="scss">

  .results {
    margin-top: 50px;
  }

  .is-active {
    border: 3px #23d160 solid;
  }


</style>
