<template> 
  <div>
      <p @click="passAttr">Pawn</p>
      {{ pos }}
  </div>
</template>

<script>
import { inject, computed, reactive } from 'vue'

export default {
    name: 'Pawn',
    emits: ["chessAttr"],
    setup(props, { emit }) {

      const pos = inject('pos')
      
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

      const passAttr = () => {
        console.log('-/-/-/-passAttr')
        console.log('inject : ',pos)
        console.log('inject : ',pos.value)
        emit('chessAttr', range(pos.value))
      }
      return {
      passAttr,
      pos,
      }
    },
    
}
</script>