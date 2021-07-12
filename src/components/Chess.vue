<template> 
  <div v-bind:style="{ backgroundColor: color}">
      <!-- Chess<p>{{ chessProp.name }}</p> -->
      <component :is="chessType"  v-bind="$attrs" :chessProp="chessProp"></component>
      <!-- <King /> -->
  </div>
</template>

<script>
import { ref, toRef, computed, watch } from 'vue'
import King from './chess/King.vue'
import Pawn from './chess/Pawn.vue'

export default {
  name: 'Chess',
  components: {
    King,
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
    const color = ref('')
    
    const chessType = computed(() => {
      return props.chessProp.name
    })

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
      color,
      chessType,
      // getAttr,
    }
  }
}
</script>
