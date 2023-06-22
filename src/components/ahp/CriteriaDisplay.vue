<template>

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

  <v-btn @click="addCriteria">
    Add Criteria
  </v-btn>

  <CriteriaList></CriteriaList>


</template>

<script setup>

import { ref } from 'vue'

import CriteriaList from './CriteriaList.vue'

import { criteria } from '../../state/criteria.js'

const newCriteria = ref("test")



function addCriteria(event) {
    if(newCriteria.value != "") {
        criteria.criteriaList.push(newCriteria.value)
    }
    newCriteria.value = ""
}

</script>