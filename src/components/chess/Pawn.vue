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
    emits: ["chessRange"],
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
        let step = []

        if (props.chessProp.isFirstMove === true) {
          step.push((x-2) + '-' + y)
          props.chessProp.isFirstMove = false
        }
        step.push((x-1) + '-' + y)

        // props 在子層變過可自動返回父層耶!

        
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