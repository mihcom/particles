<template lang="pug">
  div#particles
</template>

<script>
  import options from './options.json'
  import particlesJS from 'particles.js'

  import yellowHeart from './assets/heart-yellow.png'

  import pinkHeart from './assets/heart-pink.png'
  import greyHeart from './assets/heart-grey.png'

  const hearts = [yellowHeart, pinkHeart, greyHeart]

  export default {
    mounted () {
      this.renderParticles()
    },

    methods: {
      renderParticles () {
        options.particles.shape.image.src = yellowHeart

        particlesJS.particlesJS('particles', options)

        const pJS = particlesJS.pJSDom[0].pJS,
          particles = pJS.particles.array

        setTimeout(() => {
          particles.forEach(particle => this.customizeParticle(pJS, particle))
        }, 1000)
      },

      customizeParticle (pJS, particle) {
        const random = Math.floor(Math.random() * hearts.length),
          image = new Image()

        image.src = hearts[random]

        particle.draw = () => {
          pJS.tmp.img_obj = image
        }

        window.console.log('modified')
      }
    }
  }
</script>

<style lang="stylus" scoped>
</style>
