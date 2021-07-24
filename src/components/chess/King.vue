<template> 
  <div @click="passRange">
      <p>King</p>
      <!-- {{ pos }} -->
  </div>
</template>

<script>
import { inject, computed, reactive } from 'vue'

export default {
    name: 'King',
    emits: ["chessRange"],
    props: {
    'chessProp': {
      type: Object,
    },
  },
    setup(props, { emit }) {

      console.log('King prop', props.chessProp)
      // const pos = inject('pos')
      
      const range = (xy) => {
        console.log('xy: ', xy)
        
        let step = []

        let x = parseInt(xy.split('-')[0])
        let y = parseInt(xy.split('-')[1])

        step.push((x+1) + '-' + y)
        step.push((x-1) + '-' + y)
        step.push(x + '-' + (y+1))
        step.push(x + '-' + (y-1))
        step.push((x+1) + '-' + (y+1))
        step.push((x-1) + '-' + (y-1))
        step.push((x-1) + '-' + (y+1))
        step.push((x+1) + '-' + (y-1))
        
        return step
      }

      const passRange = () => {
        console.log('-/-/-/-passRange')
        // console.log('inject : ',pos.value)
        emit('chessRange', {step: range(props.chessProp.xy)})
      }
      return {
      passRange,
      // pos,
      }
    },
    
}
</script>