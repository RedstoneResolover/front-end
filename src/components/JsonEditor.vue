<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'

import * as monaco from 'monaco-editor/esm/vs/editor/editor.api'
import jsonWorker from 'monaco-editor/esm/vs/language/json/json.worker?worker'
import 'monaco-editor/esm/vs/language/json/monaco.contribution'
import type { Window as MonacoWindow, editor as MonacoEditor } from 'monaco-editor'

(window as MonacoWindow).MonacoEnvironment ??= {
  getWorker() {
    return new jsonWorker()
  }
}

const monacoRoot = ref<HTMLDivElement | void>()
let editor: MonacoEditor.IStandaloneCodeEditor

const props = defineProps<{
    modelValue: string
}>()

const emits = defineEmits<{
    (e: 'update:modelValue', modelValue: string): void
}>()

onMounted(() => {
    editor = monaco.editor.create(monacoRoot.value!, {
        language: 'json',
        value: props.modelValue,
        automaticLayout: true
    })

    editor.onDidChangeModelContent(() => {
        emits('update:modelValue', editor.getValue())
    })
})

onUnmounted(() => {
    editor.dispose()
})
</script>

<template>
    <div ref="monacoRoot" class="monaco-root"></div>
</template>

<style scoped>
.monaco-root {
    height: 100vh;
    width: 50%;
}
</style>