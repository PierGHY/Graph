<script setup lang="ts">
    import { Ref, onMounted, ref } from "vue";
    import vSelect from "vue-select";

    interface Departement {
        nom: string,
        code: string,
        codeRegion: String
    }

    const selectDepartement: Ref<Departement> = ref({
        "nom": "Ain",
        "code": "01",
        "codeRegion": "84"
    })

    const emit = defineEmits<{
        (e: 'update', value: string): void
    }>()

    const departements: Ref<Array<Departement>> = ref([]);

    onMounted(() => {
        fetch('https://geo.api.gouv.fr/departements').then(res => {
            res.json().then(jsonResponse => {
                departements.value = jsonResponse
            })
        })
    })

    function setSelected (value: Departement) {
        emit('update', value.code)
    }

</script>

<template>
    <v-select label="nom" :clearable="false" :options="departements" v-model="selectDepartement" @option:selected="setSelected">
    </v-select>
</template>

<style>
@import "vue-select/dist/vue-select.css";
</style>