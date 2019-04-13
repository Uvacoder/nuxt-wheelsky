<template>
  <div class="details">
    <div>
      <p class="text-lg">Parfum actuel: {{ highlightedFlavor }}</p>
    </div>
    <button
      class="custom-button bg-transparent active:hover:bg-purple text-purple-dark font-semibold py-2 px-4 border border-purple rounded"
      @click="digFlavor"
    >
      {{ choisirSaveur }}
    </button>
  </div>
</template>

<script>
export default {
  props: {
    highlightedFlavor: {
      type: String,
      default: 'Erreur sur le parfum choisi !',
      required: true
    },
    depthTier: {
      type: Number,
      default: 1,
      required: true
    }
  },
  computed: {
    choisirSaveur() {
      if (this.depthTier < 3) {
        return 'Continuer'
      } else {
        return 'Choisir cette saveur'
      }
    }
  },
  methods: {
    digFlavor() {
      if (this.depthTier === 3) {
        this.$emit('reset-depth', this.highlightedFlavor)
      } else {
        this.$emit('dig-flavor', this.highlightedFlavor)
      }
    }
  }
}
</script>

<style lang="sass" scoped>
.details
  margin: 3em auto
  // border: solid 1px red
  // transform: translate3d(-25%, 0, 0)
.custom-button
  display: flex
  margin: 0 auto
  margin-top: 0.5em
</style>
