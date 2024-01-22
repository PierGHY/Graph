<script setup lang="ts">
import { Ref, onMounted, ref } from 'vue';
import "echarts";
import VChart from "vue-echarts";

const option = ref({
    tooltip: {
        trigger: 'item'
    },
    legend: {
        type: 'scroll',
        orient: 'vertical',
        right: 10,
        top: 20,
        bottom: 20,
    },
    series: [
        {
            name: 'Population from',
            type: 'pie',
            radius: '70%',
            avoidLabelOverlap: false,
            emphasis: {
                itemStyle: {
                    shadowBlur: 10,
                    shadowOffsetX: 0,
                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
            },
            data: [
            ]
        }
    ]
});

interface Props {
    numberRegion: string
}

const props = withDefaults(defineProps<Props>(), {
    numberRegion: '73'
})

interface City {
    nom: string,
    code: string,
    codesPostaux: Array<String>,
    siren: string,
    codeEpci: string,
    codeDepartement: string,
    codeRegion: string,
    population: number
}

const cities: Ref<Array<City>> = ref([]);

onMounted(async () => {
    await fetch('https://geo.api.gouv.fr/departements/' + props.numberRegion + '/communes').then(res => {
        res.json().then(jsonResponse => {
            cities.value = jsonResponse
            cities.value = cities.value.sort((a, b) => b.population - a.population)
            cities.value.forEach(element => {
                option.value.series[0].data.push(
                    { name: element.nom, value: element.population }
                )
            });
        })
    })
})
</script>

<template>
    <div>
        <v-chart class="chart" :option="option"/>
    </div>
</template>

<style scoped>
.chart {
    height: 800px;
    width: 1400px;
}
</style>