<template> 
  <div @click="passRange">
      <p>Pawn</p>
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
      const chessMap = inject('chessMap')
      
      const range = (xy) => {
        console.log('xy: ', xy)
        let x = parseInt(xy.split('-')[0])
        let y = parseInt(xy.split('-')[1])
        let step = []
        
        if (props.chessProp.team === 1) {

          if (props.chessProp.isFirstMove === true) {
            console.log('chessMap:', chessMap)
            let possible = (x-2) + '-' + y
            if (chessMap.find(c => c.xy == possible && c.isAlive == true)) {
                
            } else {

                step.push(possible)
            }
            // props 在子層變過可自動返回父層耶!
            // props.chessProp.isFirstMove = false
          }

          step.push((x-1) + '-' + y)

        } else {

          if (props.chessProp.isFirstMove === true) {
              let possible = (x+2) + '-' + y
            if (chessMap.find(c => c.xy == possible && c.isAlive == true)) {
                
            } else {
                step.push(possible)
            }            
            // props.chessProp.isFirstMove = false
          }

          step.push((x+1) + '-' + y)

        }

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
      }
    },
    
}
</script>