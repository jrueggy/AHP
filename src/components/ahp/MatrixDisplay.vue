<template>
    <v-container>
    <v-row>
            <v-col>
                    <v-btn @click="generateMatrix">
                        Create Matrix
                    </v-btn>
            </v-col>
        </v-row>
    </v-container>


    <v-container v-show="show">
        <v-row>
            <v-col>

            </v-col>
            <v-col v-for="criteria in criteria.criteriaList">
                <v-btn class="text-none" variant="outlined" color="pink">
                    
                    {{ criteria }}
                    
                </v-btn>
            </v-col>
        </v-row>
        <v-row v-for="row in matrix.matrixValues">

            <v-col>
                <v-btn class="text-none" variant="outlined" color="indigo">
                    {{ row.criteria }}
                </v-btn>
            </v-col>

            <v-col v-for="col in row.array">
                <Versus :cell=col>
                </Versus>
            </v-col>
        </v-row>
    </v-container>
</template>

<script setup>

import { matrix } from '../../state/matrix.js'
import { criteria } from '../../state/criteria.js'

import Versus from './Versus.vue'

import { ref, computed } from 'vue'

const show = ref(false)

const allPainted = computed(() => {
    let criteriaNumber = criteria.criteriaList.length

    for (let ri = 0; ri < criteriaNumber; ri++) {
        for (let ci = 0; ci < criteriaNumber; ci++) {
            if(!matrix.matrixValues[ri].array[ci].painted)
                return false
        }
    }
    return true
})

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
            let pnt = false;
            if(ri == ci) {
                val = 1
                pnt = true
            }
            
            matrix.matrixValues[ri].array.push({
                value: val,
                inverse: true,
                criteriaRow: criteria.criteriaList[ri],
                criteriaCol: criteria.criteriaList[ci],
                ci: ci,
                ri: ri,
                painted: pnt
            })
        }

    }

    show.value = true

}

</script>