<template>
  <div>
    <div v-if="firstPhase" class="main-content">
      <wheelsky
        ref="wheel"
        :highlighted-flavor="highlightedFlavor"
        class="wheel-block"
        :available-flavors="activeFlavorArray"
        @update-flavor="updateHighlightedFlavor"
        @reset-wheel="resetWheel"
      ></wheelsky>
      <div class="right-aside-block">
        <flavor-details
          class="details-block"
          :highlighted-flavor="highlightedFlavor"
          :depth-tier="depthTier"
          @dig-flavor="flavorIsChosen"
          @reset-depth="selectFlavor"
        ></flavor-details>
        <chosen-flavors :selected-flavors="selectedFlavors"></chosen-flavors>
      </div>
    </div>
    <div v-if="!firstPhase">
      <user-form></user-form>
    </div>
  </div>
</template>

<script>
// import { Draggable } from 'gsap/Draggable'
import Wheelsky from '@/components/Wheelsky.vue'
import FlavorDetails from '@/components/FlavorDetails.vue'
import ChosenFlavors from '@/components/ChosenFlavors.vue'
import UserForm from '@/components/UserForm.vue'
import whiskyFlavors from '@/assets/data/whiskyFlavors.json'
let Draggable

export default {
  name: 'App',
  components: {
    Wheelsky,
    FlavorDetails,
    ChosenFlavors,
    UserForm
  },
  data() {
    return {
      allFlavors: [
        {
          name: 'banana',
          color: '#1ad1e5'
        },
        {
          name: 'strawberry',
          color: '#e2071c'
        },
        {
          name: 'cherry',
          color: '#fb1'
        },
        {
          name: 'pineapple',
          color: '#b000b5'
        },
        {
          name: 'watermelon',
          color: '#fa7a55'
        }
      ],
      all: whiskyFlavors,
      highlightedFlavor: undefined,
      selectedFlavors: [],
      previouslySelectedFlavor: undefined,
      depthTier: 1,
      firstPhase: true,
      activeFlavorArray: [
        {
          name: 'Doux'
        },
        {
          name: 'Floral'
        },
        {
          name: 'Fruité'
        },
        {
          name: 'Herbacé'
        },
        {
          name: 'Marin'
        },
        {
          name: 'Minéral'
        },
        {
          name: 'Huileux'
        },
        {
          name: 'Fumé'
        },
        {
          name: 'Soufré'
        },
        {
          name: 'Vineux'
        },
        {
          name: 'Boisé'
        },
        {
          name: 'Céréale'
        }
      ]
    }
  },
  mounted() {
    if (process.browser) {
      Draggable = require('gsap/Draggable')
      // window.onbeforeunload = function() {
      // return "Êtes-vous sûr de vouloir recharger la page ?";
      // }
    }
  },
  methods: {
    updateHighlightedFlavor(e) {
      this.highlightedFlavor = e.name
    },
    selectFlavor(e) {
      this.selectedFlavors.push(e)
      this.resetWheel()
    },
    flavorIsChosen(e) {
      this.activeFlavorArray = []
      let tempArray = []
      this.depthTier += 1
      if (this.depthTier === 2) {
        const elementsTier2 = this.all[0].children.find(el => el.name === e)
          .children
        for (let i = 0; i < elementsTier2.length; i++) {
          console.log(elementsTier2[i])
          tempArray.push(elementsTier2[i])
          //!todo need to filter the N+1 to avoid printing parents w/ no children...
        }
        console.log(elementsTier2)
        this.previouslySelectedFlavor = e
      } else if (this.depthTier === 3) {
        const elementsTier2 = this.all[0].children.find(
          el => el.name === this.previouslySelectedFlavor
        ).children
        const elementsTier3 = elementsTier2.find(el2 => el2.name === e).children
        for (let i = 0; i < elementsTier3.length; i++) {
          tempArray.push(elementsTier3[i])
        }
      }
      tempArray = tempArray.filter(
        x => this.selectedFlavors.indexOf(x.name) < 0
      )
      this.activeFlavorArray = tempArray
      if (this.activeFlavorArray.length === 1) {
        this.activeFlavorArray.push(this.activeFlavorArray[0])
      }
      if (this.activeFlavorArray.length === 0) {
        this.resetWheel()
        //!todo OMFG, what an MLG pro. kill me.
      }
      this.$refs.wheel.checkFlavor(Draggable.get('#wheel'))
    },
    resetWheel() {
      this.depthTier = 1
      this.activeFlavorArray = [
        {
          name: 'Doux'
        },
        {
          name: 'Floral'
        },
        {
          name: 'Fruité'
        },
        {
          name: 'Herbacé'
        },
        {
          name: 'Marin'
        },
        {
          name: 'Minéral'
        },
        {
          name: 'Huileux'
        },
        {
          name: 'Fumé'
        },
        {
          name: 'Soufré'
        },
        {
          name: 'Vineux'
        },
        {
          name: 'Boisé'
        },
        {
          name: 'Céréale'
        }
      ]
    }
  }
}
</script>
