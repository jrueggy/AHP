<template>
    <v-btn @click="generateWeigths" :disabled="!allPainted">
        Get importance
    </v-btn>

    <v-table>
        <thead>
            <tr>
                <th class="text-left">
                    Criteria
                </th>
                <th class="text-left">
                    Importance (%)
                </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in weigths" :key="item.name">
                <td>{{ item.criteria }}</td>
                <td>{{ (item.importance * 100).toFixed(2)}}</td>
            </tr>
        </tbody>
    </v-table>
</template>


<script setup>

import { computed, ref } from 'vue'

import { matrix } from '../../state/matrix.js'
import { criteria } from '../../state/criteria.js'
import { importanceVector } from '../../state/importanceVector.js'

const allPainted = computed(() => {
    let criteriaNumber = criteria.criteriaList.length

    for (let ri = 0; ri < criteriaNumber; ri++) {
        for (let ci = 0; ci < criteriaNumber; ci++) {
            if (!matrix.matrixValues[ri].array[ci].painted)
                return false
        }
    }
    return true
})

const weigths = ref([
])

function generateWeigths() {
    let n = criteria.criteriaList.length

    let calcMatrix = []
    let vector = []

    for (let ri = 0; ri < n; ri++) {

        calcMatrix.push([])

        for (let ci = 0; ci < n; ci++) {

            let calcVal = Number(matrix.matrixValues[ri].array[ci].value)

            if (matrix.matrixValues[ri].array[ci].inverse) {
                calcVal = 1.0 / calcVal
            }

            calcMatrix[ri].push(calcVal)
        }

    }

    //normalize

    for (let j = 0; j < n; j++) {
        let colSum = 0

        for (let i = 0; i < n; i++) {
            colSum += calcMatrix[i][j]
        }

        for (let i = 0; i < n; i++) {
            calcMatrix[i][j] = calcMatrix[i][j] / colSum
        }
    }

    //geometric mean

    let vectorSum = 0

    for (let i = 0; i < n; i++) {
        let rowProduct = 1

        for (let j = 0; j < n; j++) {
            rowProduct *= calcMatrix[i][j]
        }

        vector.push(Math.pow(rowProduct, 1 / n))

        vectorSum += vector[i]

    }

    //normalize vector

    for (let i = 0; i < n; i++) {
        vector[i] /= vectorSum
    }

    //store vector to state

    importanceVector.values = []
    weigths.value = []

    for (let i = 0; i < n; i++) {
        importanceVector.values.push({
            criteria: criteria.criteriaList[i],
            importance: vector[i]
        })
        weigths.value.push({
            criteria: criteria.criteriaList[i],
            importance: vector[i]
        })
    }
}




</script>