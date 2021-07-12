<template> 
  <div @click="passRange">
      <p>Pawn</p>
      <!-- {{ pos }} -->
  </div>
</template>

<script>
import { inject, computed, reactive } from 'vue'

export default {
    name: 'Pawn',
    emits: ["chessAttr"],
    props: {
    'chessProp': {
      type: Object,
    },
  },
    setup(props, { emit }) {

      console.log('Pawn prop', props.chessProp)
      // const pos = inject('pos')
      
      const range = (xy) => {
        console.log('xy: ', xy)
        let x = parseInt(xy.split('-')[0])
        let y = parseInt(xy.split('-')[1])
        let possible1 = (x+1) + '-' + y
        let possible2 = (x-1) + '-' + y
        let possible3 = x + '-' + (y+1)
        let possible4 = x + '-' + (y-1)
        let step = [possible1, possible2, possible3, possible4]
        
        return step
      }

      const passRange = () => {
        console.log('-/-/-/-passRange')
        // console.log('inject : ',pos.value)
        emit('chessRange', range(props.chessProp.xy))
      }
      return {
      passRange,
      // pos,
      }
    },
    
}
</script>