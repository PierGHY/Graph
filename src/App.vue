<script setup lang="ts">
import { ref } from 'vue';
import Graph from './components/Graph.vue';
import Select from './components/Select.vue';

const code = ref('01')

function getValue (value) {
  code.value = value
}
</script>

<template>
  <header>
    <Select @update="getValue"/>
    <Graph :key="code" :number-region="code"/>
  </header>
</template>

<style scoped>
</style>
