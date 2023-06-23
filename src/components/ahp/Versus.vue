<template>
    <v-btn @click="open" :color="defineColor" :disabled="isDiagonal">
        {{ displayValue }}
    </v-btn>

    <v-dialog v-model="dialog" persistent>
        <v-card title="Versus" subtitle="Choose importance">

            <v-container>
                <v-row>
                    <v-col>
                        <v-card>
                            
                            <v-card-item>
                                <div>
                                    <div class="text-overline mb-1 rigth">
                                        {{ props.cell.criteriaRow }}
                                    </div>
                                </div>
                            </v-card-item>
                        </v-card>
                    </v-col>
                    <v-col>
                        <v-card>
                            
                            <v-card-item>
                                <div>
                                    <div class="text-overline mb-1">
                                        {{ props.cell.criteriaCol }}
                                    </div>
                                </div>
                            </v-card-item>
                        </v-card>
                    </v-col>
                </v-row>

                <v-row>
                    <v-slider color="red"
                    v-model="importanceIndex" :ticks="labels" :max="8" step="1" show-ticks="always" tick-size="4"></v-slider>
                </v-row>

            </v-container>

            <v-card-actions class="justify-end">
              <v-btn
                variant="text"
                @click="process"
              >Accept</v-btn>
            </v-card-actions>

        </v-card>
    </v-dialog>
</template>

<script setup>

import { matrix } from '../../state/matrix.js'
import { criteria } from '../../state/criteria.js'

import { reactive, computed, onMounted, ref } from 'vue'

const props = defineProps({
    cell: Object,
})

const dialog = ref(false)

const labels = ref({
    0: '9',
    1: '7',
    2: '5',
    3: '3',
    4: '1',
    5: '3',
    6: '5',
    7: '7',
    8: '9'
})

const importanceIndex = ref(4)

const defineColor = computed(() => {
    return props.cell.painted ? 'teal-lighten-4' :  'white'
})

const displayValue = computed(() => {

    let prefix = ""

    if(props.cell.inverse && props.cell.value != 0 && props.cell.value != 1) {
        prefix = "1 / "
    }

    if(props.cell.value == 0) return ""

    return prefix + props.cell.value
})

const isDiagonal = computed(() => {
    return (props.cell.ci == props.cell.ri)
})

function process() {

    matrix.matrixValues[props.cell.ri].array[props.cell.ci].value = labels.value[importanceIndex.value]
    matrix.matrixValues[props.cell.ci].array[props.cell.ri].value = labels.value[importanceIndex.value]

    matrix.matrixValues[props.cell.ri].array[props.cell.ci].painted = true
    matrix.matrixValues[props.cell.ci].array[props.cell.ri].painted = true

    if(importanceIndex.value < 4) {
        matrix.matrixValues[props.cell.ri].array[props.cell.ci].inverse = false
        matrix.matrixValues[props.cell.ci].array[props.cell.ri].inverse = true
    } else {
        matrix.matrixValues[props.cell.ri].array[props.cell.ci].inverse = true
        matrix.matrixValues[props.cell.ci].array[props.cell.ri].inverse = false
    }

    dialog.value = false
}

function open() {
    dialog.value = true

    if(props.cell.value != 0) {
        let i = 0
        if(props.cell.inverse) {
            i = 4
        }

        while(labels.value[i] != props.cell.value) i++

        importanceIndex.value = i
    }

}

</script>