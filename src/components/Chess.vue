<template> 
  <div class="chess" v-bind:style="{ backgroundColor: teamColor}">
  <!-- <div class="chess" v-bind:style="{ teamColor }"> -->
      <!-- Chess<p>{{ chessProp.name }}</p> -->
      <component :is="chessType"  v-bind="$attrs" :chessProp="chessProp"></component>
  </div>
</template>

<script>
import { ref, toRef, computed, watch } from 'vue'
import King from './chess/King.vue'
import Queen from './chess/Queen.vue'
import Bishop from './chess/Bishop.vue'
import Knight from './chess/Knight.vue'
import Rook from './chess/Rook.vue'
import Pawn from './chess/Pawn.vue'

export default {
  name: 'Chess',
  components: {
    King,
    Queen,
    Bishop,
    Knight,
    Rook,
    Pawn,
  },
  props: {
    'chessProp': {
      type: Object,
    },
  },
  // emits: ["from_child"],
  setup(props, { emit }) {
    
    console.log("prop:", props.chessProp.name)
    
    const chessType = computed(() => {
      return props.chessProp.name
    })
    
    const teamColor = computed(() => {

      if (props.chessProp.team == 1) {
        return '#FFBFFF'
      } else {
        return '#00EC00'
      }

    })

    // const data = toRef(props.chessProp)

    // watch(() => props.child_prop, (oldValue, newValue) => {
    //   console.log('---', oldValue, newValue)
    //   if (props.child_prop === 1) {
    //   color.value = 'black'
    //   console.log(color.value)
    // } else {
    //   color.value = 'white'
    //   console.log(color.value)
    // }
    // })

    return {
      teamColor,
      chessType,
    }
  }
}
</script>
<style>
  .chess {
    /* background: #f00; */
    width: 100%;
    height: 100%;
    opacity: 50%;
    position: absolute;
    top: 0;
    left: 0;
  }
  p {
    color: black;
  }
</style>