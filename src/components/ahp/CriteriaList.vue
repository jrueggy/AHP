<template>
    <v-chip-group column>
        <v-chip v-for="criteria in criteria.criteriaList">
            {{ criteria }}
            <v-icon key="criteria" @click="removeCriteria(criteria)"> mdi-close </v-icon>
        </v-chip>
    </v-chip-group>
</template>

<script setup>

import { criteria } from '../../state/criteria.js'

function removeCriteria(criteriaToDelete) {
    const index = criteria.criteriaList.indexOf(criteriaToDelete);
    if (index > -1) {
        criteria.criteriaList.splice(index, 1);
    }
} 

</script>