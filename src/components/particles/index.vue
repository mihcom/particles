<template lang="pug">
  div#particles
</template>

<script>
  import options from './options.json'
  import particlesJS from './particles.js'
  import TweenLite from 'gsap'

  import transparent from './assets/transparent.png'
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
        options.particles.shape.image.src = transparent

        particlesJS.particlesJS('particles', options)

        const pJS = particlesJS.pJSDom[0].pJS,
          particles = pJS.particles.array

        setTimeout(() => {
          particles.forEach(particle => this.customizeParticle(particle))
        }, 1000)
      },

      customizeParticle (particle) {
        const random = Math.floor(Math.random() * hearts.length),
          image = new Image(),
          opacityObject = {opacity: 0}

        TweenLite.to(opacityObject, 2, {
          opacity: 1,
          ease: window.Power1.easeOut
        })

        image.addEventListener('load', () => {
          particle.customDraw = (canvasContext, x, y, width, height) => {
            canvasContext.globalAlpha = opacityObject.opacity
            canvasContext.drawImage(image, x, y, width, height)
          }
        })

        image.src = hearts[random]
      }
    }
  }
</script>

<style lang="stylus" scoped>
</style>
