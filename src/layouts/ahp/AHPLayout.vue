<template>

    <CriteriaDisplay></CriteriaDisplay>

     <v-layout>
      <v-navigation-drawer
        permanent
        location="right"
      >
        
        AHP

        <v-divider></v-divider>

        PROCESS

      </v-navigation-drawer>
      <v-main style="height: 250px"></v-main>
    </v-layout>
</template>

<script setup>

import CriteriaDisplay from '../../components/ahp/CriteriaDisplay.vue'

</script>