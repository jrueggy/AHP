<template>


<v-card>
    <v-toolbar
      color="primary"
    >
      <v-toolbar-title>Analytical Hierarchy Process (AHP)</v-toolbar-title>
    </v-toolbar>
    <div>
      <v-tabs
        v-model="tab"
        color="primary"
      >
        <v-tab value="criteria">
          <v-icon start>
            mdi-format-list-bulleted
          </v-icon>
          Criteria
        </v-tab>
        <v-tab value="matrix">
          <v-icon start>
            mdi-grid
          </v-icon>
          Preference Matrix
        </v-tab>
        <v-tab value="importance">
          <v-icon start>
            mdi-trophy-variant-outline
          </v-icon>
          Criteria Importance
        </v-tab>
      </v-tabs>
      <v-window v-model="tab">
        <v-window-item value="criteria">
            
            <CriteriaDisplay></CriteriaDisplay>
          
        </v-window-item>
        <v-window-item value="matrix">
          
            <MatrixDisplay></MatrixDisplay>
          
        </v-window-item>
        <v-window-item value="importance">
          
            <CriteriaWeights></CriteriaWeights>

          
        </v-window-item>
      </v-window>
    </div>
  </v-card>
  
</template>

<script setup>

import CriteriaDisplay from '../../components/ahp/CriteriaDisplay.vue'
import MatrixDisplay from '../../components/ahp/MatrixDisplay.vue'
import CriteriaWeights from '../../components/ahp/CriteriaWeights.vue'

import { ref, computed } from 'vue'

const tab = ref("criteria")

</script>