<template>


<v-container>
    <v-row>
        <v-col>
            <v-responsive
        class="mx-auto"
        max-width="344"
    >
    <v-text-field
        label="New criteria"
        hide-details="auto"
        v-model="newCriteria"
    ></v-text-field>
  </v-responsive>
        </v-col>

        <v-col>
              <v-btn @click="addCriteria">
                Add Criteria
              </v-btn>
        </v-col>
    </v-row>
</v-container>
    

  <CriteriaList></CriteriaList>


</template>

<script setup>

import { ref } from 'vue'

import CriteriaList from './CriteriaList.vue'

import { criteria } from '../../state/criteria.js'

const newCriteria = ref("")



function addCriteria(event) {
    if(newCriteria.value != "") {
        criteria.criteriaList.push(newCriteria.value)
    }
    newCriteria.value = ""
}

</script>