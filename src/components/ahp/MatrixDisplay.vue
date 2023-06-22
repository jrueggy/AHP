<template>

    <v-btn @click="generateMatrix">
        Create Matrix
    </v-btn>

    <v-container>
        <v-row>
            <v-col>

            </v-col>
            <v-col v-for="criteria in criteria.criteriaList">
                <div>
                    {{  criteria }}
                </div>
            </v-col>
        </v-row>
        <v-row v-for="row in matrix.matrixValues">

            <v-col>
                <div>
                    {{ row.criteria }}
                </div>
            </v-col>

            <v-col v-for="col in row.array">
                <v-btn>
                    {{ col.value }}
                </v-btn>
            </v-col>
        </v-row>
    </v-container>    

    <Versus show=true></Versus>

</template>

<script setup>

import { matrix } from '../../state/matrix.js'
import { criteria } from '../../state/criteria.js'

import Versus from './Versus.vue'

function generateMatrix() {
    let criteriaNumber = criteria.criteriaList.length

    matrix.matrixValues = []

    for (let ri = 0; ri < criteriaNumber; ri++) {

        matrix.matrixValues.push({
            array: [],
            criteria: criteria.criteriaList[ri]
        })

        for (let ci = 0; ci < criteriaNumber; ci++) {
            let val = 0;
            if(ri == ci) val = 1
            
            matrix.matrixValues[ri].array.push({
                value: val,
                inverse: true,
                criteria: criteria.criteriaList[ci],
                ci: ci,
                ri: ri
            })
        }

    }

}

</script>