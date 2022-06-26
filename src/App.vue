<script setup lang="ts">
import JsonEditor from './components/JsonEditor.vue'

let req: string = `{
    "input": {

    },
    "output": {

    }
}`
</script>

<template>
    <JsonEditor
        v-model="req"
    />
</template>

<style>
* {
    margin: 0;
}
</style>
