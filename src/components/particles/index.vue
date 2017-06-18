<template lang="pug">
  div#particles
</template>

<script>
  import options from './options.json'
  import particlesJS from './particles.js'
  import TweenLite from 'gsap'

  import transparent from './assets/transparent.png'

  export default {
    mounted () {
      this.loadPandoraProducts()
        .then(this.renderParticles)
    },

    methods: {
      loadPandoraProducts () {
        return new Promise((resolve, reject) => {
          this.$http.get('https://www.pandora.net/en-us/feeds/products/json/')
            .then(response => resolve(response.body.data.products.product))
            .catch(reject)
        })
      },

      renderParticles (products) {
        options.particles.shape.image.src = transparent

        particlesJS.particlesJS('particles', options)

        const pJS = particlesJS.pJSDom[0].pJS,
          particles = pJS.particles.array

        setTimeout(() => {
          particles.forEach(particle => this.customizeParticle(products, particle))
        }, 1000)
      },

      customizeParticle (products, particle) {
        const random = Math.floor(Math.random() * products.length),
          sku = products[random]['@id'],
          productImageUrl = `http://static.pandora.net/consumer/jewellery/01/135x135/${sku}.png`,
          image = new Image(),
          opacityObject = {opacity: 0}

        TweenLite.to(opacityObject, 2, {
          opacity: 1,
          ease: window.Power1.easeOut
        })

        image.addEventListener('load', () => {
          particle.customDraw = (canvasContext, x, y) => {
            canvasContext.globalAlpha = opacityObject.opacity
            canvasContext.drawImage(image, x, y, image.naturalWidth, image.naturalHeight)
          }
        })

        image.src = productImageUrl
      }
    }
  }
</script>

<style lang="stylus" scoped>
</style>
