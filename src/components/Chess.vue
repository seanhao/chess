<template> 
  <div class="chess" v-bind:style="{ backgroundColor: teamColor}">
      <!-- Chess<p>{{ chessProp.name }}</p> -->
      <component :is="chessType"  v-bind="$attrs" :chessProp="chessProp"></component>
      <!-- <King /> -->
  </div>
</template>

<script>
import { ref, toRef, computed, watch } from 'vue'
import King from './chess/King.vue'
import Pawn from './chess/Pawn.vue'
import Bishop from './chess/Bishop.vue'
import Rook from './chess/Rook.vue'

export default {
  name: 'Chess',
  components: {
    King,
    Pawn,
    Bishop,
    Rook,
  },
  props: {
    'chessProp': {
      type: Object,
    },
  },
  // emits: ["from_child"],
  setup(props, { emit }) {
    
    console.log("prop:", props.chessProp.name)
    const color = ref('')
    
    const chessType = computed(() => {
      return props.chessProp.name
    })

    let teamColor = ''

    if (props.chessProp.team == 1) {
      teamColor = '#FFBFFF'
    } else {
      teamColor = '#00EC00'
    }

    const data = toRef(props.chessProp)

    // const getAttr = (val) => {
    //   console.log('-/-/-/- get Attr')
    //   console.log('-/-/-/- Attr: ', val)
    // }

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
      // getAttr,
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