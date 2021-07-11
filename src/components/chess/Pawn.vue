<template> 
  <div>
      <p @click="passAttr">Pawn</p>
      {{ pos }}
  </div>
</template>

<script>
import { inject } from 'vue'

export default {
    name: 'Pawn',
    emits: ["chessAttr"],
    setup(props, { emit }) {

      const pos = inject('pos')

      const passAttr = () => {
        console.log('-/-/-/-passAttr')
        emit('chessAttr', 8787123)
      }
      return {
      passAttr,
      pos,
      }
    },
    
}
</script>