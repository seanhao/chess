<template> 
    <div @click="passRange">
        <p>Pawn</p> 
    </div>
</template>

<script>
import { inject, computed, reactive } from 'vue'

export default {
    name: 'Pawn',
    emits: ['chessRange'],
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

            // 士兵方向判斷
            let direct = (props.chessProp.team === 1) ? -1 : 1
            
            let oneSquare = (x+direct) + '-' + y

            step.push(oneSquare)

            if (props.chessProp.isFirstMove === true) {

                console.log('chessMap:', chessMap)
                
                // 第一次移動的第二格
                let twoSquare = (x+direct*2) + '-' + y
                console.log('twoSquare ', twoSquare)

                // 若前方無人阻擋
                if (!(chessMap.find(c => c.xy == oneSquare && c.isAlive == true))) {
                    step.push(twoSquare)
                } 
                // props 在子層變過可自動返回父層耶!
                // props.chessProp.isFirstMove = false
            }
        
            return step
        }

        // 士兵只能吃斜前方的
        const capture = (xy) => {
            let x = parseInt(xy.split('-')[0])
            let y = parseInt(xy.split('-')[1])
            let pos = []
            let direct = (props.chessProp.team === 1) ? -1 : 1

            pos.push((x+direct) + '-' + (y-1))
            pos.push((x+direct) + '-' + (y+1))

            return pos

        }

        const passRange = () => {
            // console.log('-/-/-/-passRange')
            // console.log('inject : ',pos.value)
            emit('chessRange', {step: range(props.chessProp.xy),
                                capture: capture(props.chessProp.xy),
            })
        }
        return {
            passRange,
        }
    },
    
}
</script>