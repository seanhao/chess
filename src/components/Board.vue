<template>
  <div>
    select : {{ selectId }}
    <!-- chessMap : {{ chessMap }}
    chess : {{ chess }} -->
    <!-- parent: <input type="text" v-model.number="parent_prop"> -->
    <!-- from child: {{ child_emit }} -->
    <!-- <input type="text" v-model="parent_prop"> -->
  </div>
  <table>
  <!-- <button @click="reset()" >reset</button> -->
  <tr v-for="(row, rowIndex) in 8" :key="rowIndex">
    <td v-for="(col, colIndex) in 8" :key="colIndex" :id="rowIndex+'-'+colIndex" @click="selectToMove">
      <!-- {{ rowIndex }},{{ colIndex }} -
      ( {{ findChess(rowIndex+'-'+colIndex) }} ) -->
      <Chess v-if="findChess(rowIndex+'-'+colIndex)" :chessProp="findChess(rowIndex+'-'+colIndex)" @chessRange="getRange" />
      <Step class="step" v-if="findStep(rowIndex+'-'+colIndex)"/>
      <Capture class="capture" v-if="findCapture(rowIndex+'-'+colIndex)"/>
    </td>
  </tr>
 
</table>
</template>

<script>
import Chess from './Chess.vue'
import Step from './Step.vue'
import Capture from './Capture.vue'
import { computed, ref, reactive, provide, onMounted, watchEffect, isReactive, toRefs } from 'vue'

export default {
  name: 'Board',
  components: {
    Chess,
    Step,
    Capture,
  },
  setup() {
    // const chess = reactive([{id: 1, name: 'King', xy: '1-1', team: 1, isAlive: true},
    //                         {id: 2, name: 'Pawn', xy: '0-1', team: 2, isAlive: true},
    //                         {id: 3, name: 'Pawn', xy: '3-1', team: 1, isAlive: true},
    //                         {id: 4, name: 'Bishop', xy: '2-1', team: 2, isAlive: true},
    //                         {id: 5, name: 'Rook', xy: '5-5', team: 2, isAlive: true},
    //                         {id: 5, name: 'Knight', xy: '5-7', team: 1, isAlive: true}])

    const chess = reactive([{id: 1, name: 'Rook', xy: '0-0', team: 2, isAlive: true},
                            {id: 1, name: 'Knight', xy: '0-1', team: 2, isAlive: true},
                            {id: 1, name: 'Bishop', xy: '0-2', team: 2, isAlive: true},
                            {id: 1, name: 'Queen', xy: '0-3', team: 2, isAlive: true},
                            {id: 1, name: 'King', xy: '0-4', team: 2, isAlive: true},
                            {id: 1, name: 'Bishop', xy: '0-5', team: 2, isAlive: true},
                            {id: 1, name: 'Knight', xy: '0-6', team: 2, isAlive: true},
                            {id: 1, name: 'Rook', xy: '0-7', team: 2, isAlive: true},

                            {id: 1, name: 'Pawn', xy: '1-0', team: 2, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '1-1', team: 2, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '1-2', team: 2, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '1-3', team: 2, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '1-4', team: 2, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '1-5', team: 2, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '1-6', team: 2, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '1-7', team: 2, isAlive: true, isFirstMove: true, },

                            {id: 1, name: 'Rook', xy: '7-0', team: 1, isAlive: true},
                            {id: 1, name: 'Knight', xy: '7-1', team: 1, isAlive: true},
                            {id: 1, name: 'Bishop', xy: '7-2', team: 1, isAlive: true},
                            {id: 1, name: 'Queen', xy: '7-3', team: 1, isAlive: true},
                            {id: 1, name: 'King', xy: '7-4', team: 1, isAlive: true},
                            {id: 1, name: 'Bishop', xy: '7-5', team: 1, isAlive: true},
                            {id: 1, name: 'Knight', xy: '7-6', team: 1, isAlive: true},
                            {id: 1, name: 'Rook', xy: '7-7', team: 1, isAlive: true},
                            
                            {id: 1, name: 'Pawn', xy: '6-0', team: 1, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '6-1', team: 1, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '6-2', team: 1, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '6-3', team: 1, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '6-4', team: 1, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '6-5', team: 1, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '6-6', team: 1, isAlive: true, isFirstMove: true, },
                            {id: 1, name: 'Pawn', xy: '6-7', team: 1, isAlive: true, isFirstMove: true, },])
    
    // const chessMap = computed(() => {

    //   let result = []

    //   chess.forEach((element) => {
        
    //     result.push(element.xy)
        
    //   })
    //   return result

    // })
    
    
    let step = reactive([])
    let capture = reactive([])

    const getRange = (val) => {
      console.log('-/-/-/- getRange: ', val)

      // 此層判斷為避免capture在step range外的chess造成step轉換為captured的chess之step range
      // 冒泡順序為chessType的div -> chess -> board 故最一開始board的selectId會為空
      // 首先沒有chess被選擇時才可以賦予range
        if (selectId.value == '') {
            step = val.step

            if (val.capture) {
                capture = val.capture
            } else {
                capture = val.step
            }
        }
      
    }

    const findChess = (xy) => {
      // xy == Array == [x, y]
      // console.log('findChess XY: ', xy)
      // console.log('value : ', chess.value)
      let result = chess.find(c => c.xy == xy && c.isAlive == true)
      // console.log('findChess: ', result)
      return result
    }

    const findChessIndex = (xy) => {
      let index = chess.findIndex(c => c.xy == xy && c.isAlive == true)
      return index
    }

    const findStep = (xy) => {
      return step.find(s => s == xy)
    }

    const findCapture = (xy) => {
      return capture.find(s => s == xy)
    }
    

    let selectId = ref("")



    const selectToMove = (event) => {

      console.log('-*-start-*- selectToMove -- selectId.value :', selectId.value)

      if (event.currentTarget.querySelector('div.chess') === null && selectId.value === "") {
        
        // 選擇區域沒有棋子
        console.log('***** 選擇區域沒有棋子!!!')

      } else {
        // 有棋子存在

        if (selectId.value === ""){
          // 選取目標棋子
          // select target
          // console.log("event.currentTarget: ",event.currentTarget.querySelector('div'))
          selectId.value = event.currentTarget.id

          console.log("***** select selectId.value: ", selectId.value)
        
        } else if (selectId.value === event.currentTarget.id) {

          // 點選到已選取棋子，取消選取

          selectId.value = ''
          step = []
          capture = []
          console.log('***** 點選到已選取棋子，取消選取 selectId.value: ', selectId.value)

        } else {
          
          // 移動或吃掉
          
          let moveToId = event.currentTarget.id
          
          // 可移動或可以吃(為了士兵)， 這部分要再確認一下邏輯
          if (findStep(moveToId) || findCapture(moveToId)) {

            // 可移動範圍
            
              if (event.currentTarget.querySelector('div.chess') != null) {
                // 有棋子存在，吃棋判斷
                // captured piece

                let capturedId = event.currentTarget.id
                let chessIndex = findChessIndex(selectId.value)
                let capturedChessIndex = findChessIndex(capturedId)

                if (findCapture(moveToId)) {

                    if (chess[chessIndex].team == chess[capturedChessIndex].team) {
                    // 同隊不能互吃
                    alert('同隊不能互吃！！')
                    } else {
                    // 不同隊執行吃棋
                    // alert('captured piece!')

                    // 被吃的會死掉
                    chess[capturedChessIndex].isAlive = false
                    
                    // 吃的移動到捕獲位置
                    chess[chessIndex].xy = capturedId

                    // 士兵第一次移動判斷
                    chess[chessIndex].isFirstMove = false

                    selectId.value = ""
                    step = []
                    capture = []
                    }
                } else {

                    // 主要是士兵不可吃
                    // 避免士兵吃移動位置
                    alert('非吃棋範圍')

                }


              } else {
                // 沒有旗子存在，移動
                // move

                let chessIndex = findChessIndex(selectId.value)
                
                console.log("***** change selectId.value to:", selectId.value)
                console.log('***** chessIndex: ', chessIndex)
                console.log('***** step: ', step)
                
                if (findStep(moveToId)) {
                    chess[chessIndex].xy = moveToId

                    // 士兵第一次移動
                    chess[chessIndex].isFirstMove = false
                    
                    selectId.value = ""
                    step = []
                    capture = []

                } else {
                    // 不可移動範圍
                    // 也是為了士兵避免移動到捕獲位置，這邊要調整邏輯
                    alert('不可移動範圍')
                }
                
                
              }
              

            } else {
              // 不可移動範圍
              alert('不可移動範圍')

            }

          
          
        }

      }

      console.log('-*-end-*- selectToMove -- selectId.value :', selectId.value)
      
    }

    // const show = (value) => {
    //   console.log('show')
    //   child_emit.value = value
    // }

    provide('chessMap', chess)

    return {
      // parent_prop,
      // child_emit,
      findChess,
      step,
      findStep,
      findCapture,
      // show,
      // move,
      // reset,
      // select,
      selectId,
      selectToMove,
      getRange,
    //   chessMap,
      chess,
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

  .capture {
    background: rgb(0, 0, 0);
    width: 100%;
    height: 100%;
    opacity: 50%;
    position: absolute;
    top: 0;
    left: 0;
  }
</style>
