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
            if (chessMap.find(c => c.xy == possible)) {
                break
            }
        }
        for (let i = 1; i <= 7; i++) {
            let possible = (x-i) + '-' + (y-i)
            step.push(possible)
            if (chessMap.find(c => c.xy == possible)) {
                break
            }
        }
        for (let i = 1; i <= 7; i++) {
            let possible = (x-i) + '-' + (y+i)
            step.push(possible)
            if (chessMap.find(c => c.xy == possible)) {
                break
            }
        }
        for (let i = 1; i <= 7; i++) {
            let possible = (x+i) + '-' + (y-i)
            step.push(possible)
            if (chessMap.find(c => c.xy == possible)) {
                break
            }
        }

        // let possible5 = (x+i) + '-' + (y+i)
        // let possible6 = (x-i) + '-' + (y-i)
        // let possible7 = (x-i) + '-' + (y+i)
        // let possible8 = (x+i) + '-' + (y-i)


        // let step = [possiblei, possible2, possible3, possible4, 
        //             possible5, possible6, possible7, possible8, ]
        
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