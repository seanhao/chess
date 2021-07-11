<template>
  <div>
    select : {{ selectId }}
    <!-- parent: <input type="text" v-model.number="parent_prop"> -->
    <!-- from child: {{ child_emit }} -->
    <!-- <input type="text" v-model="parent_prop"> -->
  </div>
  <table>
  <!-- <button @click="reset()" >reset</button> -->
  <tr v-for="(row, rowIndex) in 3" :key="rowIndex">
    <td v-for="(col, colIndex) in 3" :key="colIndex" :id="rowIndex+'-'+colIndex" @click="selectToMove">
      <!-- {{ rowIndex }},{{ colIndex }} -
      ( {{ findChess(rowIndex+'-'+colIndex) }} ) -->
      
      <div v-if="findChess(rowIndex+'-'+colIndex)">
        <Chess class="chess" v-if="findChess(rowIndex+'-'+colIndex)" :chessProp="findChess(rowIndex+'-'+colIndex)"  />
      </div>
      <Step class="step" :stepProp="step" v-if="findStep(rowIndex+'-'+colIndex)"/>
    </td>
  </tr>
 
</table>
</template>

<script>
import Chess from './Chess.vue'
import Step from './Step.vue'
import { ref, reactive } from 'vue'

export default {
  name: 'Board',
  components: {
    Chess,
    Step,
  },
  setup() {
    const chess = reactive([{name: 'Pawn', type: 1, xy: '0-1'},
                            {name: 'King', type: 2, xy: '1-1'},])
    
    let step = reactive([])

    const findChess = (xy) => {
      // xy == Array == [x, y]
      // console.log('findChess XY: ', xy)
      // console.log('value : ', chess.value)
      let result = chess.find(c => c.xy == xy)
      // console.log('findChess: ', result)
      return result
    }

    const findChessIndex = (xy) => {
      let index = chess.findIndex(c => c.xy === xy)
      return index
    }

    const findStep = (xy) => {
      return step.find(s => s == xy)
    }
    
    // ????
    // const reset = () => {
    //   console.log('******** reset')
    //   chess = [{name: 'Pawn', type: 1, xy: '0-1'},
    //             {name: 'Queen', type: 0, xy: '1-1'},]
    //   console.log(chess)
    // }

    const move = () => {
      let chessIndex = chess.findIndex(c => JSON.stringify(c.xy) == JSON.stringify('1-1'))
      console.log('chessIndex: ', chessIndex)
      chess[chessIndex].xy = '1-0'

    }

    // const parent_prop = ref(123)
    // const child_emit = ref("")

    let selectId = ref("")

    // select
    // const select = (event) => {
    //   if (selectId.value === ""){
    //     selectId.value = event.currentTarget.id
    //     console.log("selectId.value: ", selectId.value)
    //   } else if (selectId.value === event.currentTarget.id) {
    //     selectId.value = ""
    //     console.log("cancel selectId.value: ", selectId.value)
    //   } else {
    //     selectId.value = event.currentTarget.id
    //     console.log("change selectId.value to:", selectId.value)
    //   }
    // }

    const selectToMove = (event) => {

      console.log('-*-start-*- selectToMove -- selectId.value :', selectId.value)

      if (event.currentTarget.querySelector('div.chess') === null && selectId.value === "") {
        
        // no chess 

        console.log('***** no chess!!!')

      } else {
        
        if (selectId.value === ""){
          
          // select target
          // console.log("event.currentTarget: ",event.currentTarget.querySelector('div'))
          selectId.value = event.currentTarget.id
          console.log("***** select selectId.value: ", selectId.value)
          
          let pos = selectId.value
          let x = parseInt(pos.split('-')[0])
          let y = parseInt(pos.split('-')[1])
          let possible1 = (x+1) + '-' + y
          let possible2 = (x-1) + '-' + y
          let possible3 = x + '-' + (y+1)
          let possible4 = x + '-' + (y-1)
          step = [possible1, possible2, possible3, possible4]
        
        } else if (selectId.value === event.currentTarget.id) {

          // cancel selected

          selectId.value = ""
          step = []
          console.log("***** cancel selectId.value: ", selectId.value)

        } else {

          // move
          
          if (event.currentTarget.querySelector('div.chess') != null) {
            
            //captured piece

            console.log("***** captured piece ")
            alert('danger!!')

          } else {
            
            // move

            let chessIndex = findChessIndex(selectId.value)
            let moveToId = event.currentTarget.id
            console.log("***** change selectId.value to:", selectId.value)
            console.log('***** chessIndex: ', chessIndex)
            console.log('***** step: ', step)
            if (findStep(moveToId)) {
              chess[chessIndex].xy = moveToId
              selectId.value = ""
              step = []
            } else {
              alert('can\'t move here')
            }
            

          }
          
        }

      }

      console.log('-*-end-*- selectToMove -- selectId.value :', selectId.value)
      
    }

    // const show = (value) => {
    //   console.log('show')
    //   child_emit.value = value
    // }


    return {
      // parent_prop,
      // child_emit,
      findChess,
      step,
      findStep,
      // show,
      // move,
      // reset,
      // select,
      selectId,
      selectToMove,
    }
  }
}
</script>
<style>
  table {width: 100%;}

  td {
    position: relative;
    width: 50px;
    height: 50px;
    border: 1px solid #ccc;
    /* padding-top: 5%; */
  }
  
  .step {
    background: #f00;
    width: 100%;
    height: 100%;
    opacity: 50%;
    position: absolute;
    top: 0;
    left: 0;
  }
</style>
