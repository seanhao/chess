<template> 
  <div @click="passRange">
      <p>Bishop</p>
      <!-- {{ chessMap }} -->
  </div>
</template>

<script>
import { inject, computed, reactive } from 'vue'

export default {
    name: 'Bishop',
    emits: ["chessRange"],
    props: {
    'chessProp': {
      type: Object,
    },
  },
    setup(props, { emit }) {

      console.log('Bishop prop', props.chessProp)

      const chessMap = inject('chessMap')
      
      const range = (xy) => {
        console.log('xy: ', xy)
        let x = parseInt(xy.split('-')[0])
        let y = parseInt(xy.split('-')[1])

        let step = []

        for (let i = 1; i <= 7; i++) {
            let possible = (x+i) + '-' + (y+i)
            step.push(possible)
            if (chessMap.find(c => c.xy == possible && c.isAlive == true)) {
                break
            }
        }
        for (let i = 1; i <= 7; i++) {
            let possible = (x-i) + '-' + (y-i)
            step.push(possible)
            if (chessMap.find(c => c.xy == possible && c.isAlive == true)) {
                break
            }
        }
        for (let i = 1; i <= 7; i++) {
            let possible = (x-i) + '-' + (y+i)
            step.push(possible)
            if (chessMap.find(c => c.xy == possible && c.isAlive == true)) {
                break
            }
        }
        for (let i = 1; i <= 7; i++) {
            let possible = (x+i) + '-' + (y-i)
            step.push(possible)
            if (chessMap.find(c => c.xy == possible && c.isAlive == true)) {
                break
            }
        }
        
        return step
      }

      const passRange = () => {
        console.log('-/-/-/-passRange')
        emit('chessRange', range(props.chessProp.xy))
      }
      return {
      passRange,
      chessMap
      }
    },
    
}
</script>