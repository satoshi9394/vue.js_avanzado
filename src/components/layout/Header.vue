<template lang="pug">
    section.hero.is-success
        .hero-head
            header.nav
                .container
                    .nav-left
                        strong radio Platzi music
                    .nav-right.nav-menu
                        router-link.nav-item(to="/") Buscar
                        router-link.nav-item(to="about")  Nosotros

        .hero-body
            .container.has-text-centered
                h1.title Platzi music:
                    h3.subtitle Canciones que te gustan!
                pm-player


</template>


<script>
import PmPlayer from '@/components/Player.vue'


export default {
    components: { PmPlayer }
}
</script>


