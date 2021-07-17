<template> 
  <div @click="passRange">
      <p>Knight</p>
      <!-- {{ pos }} -->
  </div>
</template>

<script>
import { inject, computed, reactive } from 'vue'

export default {
    name: 'Knight',
    emits: ["chessRange"],
    props: {
    'chessProp': {
      type: Object,
    },
  },
    setup(props, { emit }) {

      console.log('Knight prop', props.chessProp)
      // const pos = inject('pos')
      
      const range = (xy) => {
        console.log('xy: ', xy)
        
        let step = []

        let x = parseInt(xy.split('-')[0])
        let y = parseInt(xy.split('-')[1])

        step.push((x+1) + '-' + (y-2))
        step.push((x+2) + '-' + (y-1))
        step.push((x+2) + '-' + (y+1))
        step.push((x+1) + '-' + (y+2))

        step.push((x-1) + '-' + (y+2))
        step.push((x-2) + '-' + (y+1))
        step.push((x-2) + '-' + (y-1))
        step.push((x-1) + '-' + (y-2))
        
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