<template>
  <div class="chosen-flavors">
    <p class="font-semibold">Saveurs choisies</p>
    <span
      v-for="flavor in flavors"
      :key="flavor.id"
      :class="{ 'text-purple': selectedFlavors.length === 0 }"
      >{{ flavor }}</span
    >
    <!-- <button class="btn btn-yellow">proceed further button !</button> -->
  </div>
</template>

<script>
export default {
  props: {
    selectedFlavors: {
      type: Array,
      default() {}
    }
  },
  computed: {
    flavors() {
      if (this.selectedFlavors.length === 0) {
        return ['Aucune pour le moment']
      } else {
        return this.selectedFlavors.join(', ')
      }
    }
  }
}
</script>

<style lang="sass" scoped>
.chosen-flavors
  text-align: center
</style>
