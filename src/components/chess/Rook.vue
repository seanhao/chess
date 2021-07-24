<template> 
  <div @click="passRange">
      <p>Rook</p>
  </div>
</template>

<script>
import { inject, computed, reactive } from 'vue'

export default {
    name: 'Rook',
    emits: ["chessRange"],
    props: {
    'chessProp': {
      type: Object,
    },
  },
    setup(props, { emit }) {

      const chessMap = inject('chessMap')
      
      const range = (xy) => {
        
        let x = parseInt(xy.split('-')[0])
        let y = parseInt(xy.split('-')[1])

        let step = []

        for (let i = 1; i <= 7; i++) {
            let possible = x + '-' + (y+i)
            step.push(possible)
            if (chessMap.find(c => c.xy == possible && c.isAlive == true)) {
                break
            }
        }
        for (let i = 1; i <= 7; i++) {
            let possible = x + '-' + (y-i)
            step.push(possible)
            if (chessMap.find(c => c.xy == possible && c.isAlive == true)) {
                break
            }
        }
        for (let i = 1; i <= 7; i++) {
            let possible = (x-i) + '-' + y
            step.push(possible)
            if (chessMap.find(c => c.xy == possible && c.isAlive == true)) {
                break
            }
        }
        for (let i = 1; i <= 7; i++) {
            let possible = (x+i) + '-' + y
            step.push(possible)
            if (chessMap.find(c => c.xy == possible && c.isAlive == true)) {
                break
            }
        }
        return step
      }

      const passRange = () => {

        emit('chessRange', {step: range(props.chessProp.xy)})
      }
      return {
      passRange,
      }
    },
    
}
</script>