<template lang="pug">
    .content(v-if="track.album && track.preview_url")
        p
            img(:src="track.album.images[0].url")
        p
            strong  {{ track.name }}
            small   [{{ track.duration_ms | ms-to-mm }}]
        p 
            audio(controls, v-bind:src="track.preview_url" )




</template>


<script>

import { mapState } from 'vuex'

export default {
    
    computed: {
        ...mapState(['track'])
    },
}
</script>


<style lang="scss" scoped>

 img {
     width: 30vh;
     border-radius: 50%;
 }

</style>>

