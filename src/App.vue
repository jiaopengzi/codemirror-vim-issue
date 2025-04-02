<script setup lang="ts">
import { basicSetup, EditorView } from 'codemirror'
import { vim } from '@replit/codemirror-vim'

import { onMounted, useTemplateRef } from 'vue'

defineOptions({ name: 'EditorCodemirror' })

const codemirrorRef = useTemplateRef<HTMLElement | null>('codemirrorRef')

let cmView: EditorView = null!

const initializeCodeMirror = () => {
  cmView = new EditorView({
    doc: '',
    extensions: [
      vim({
        status: true,
      }),
      basicSetup,
    ],
    parent: codemirrorRef.value!,
  })
}

onMounted(() => {
  initializeCodeMirror()
})
</script>

<template>
  <div ref="codemirrorRef" id="my-codemirror"></div>
</template>

<style>
#my-codemirror {
  height: 200px;
  width: 800px;
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.cm-editor {
  height: 100%;
  width: 100%;
}

.cm-vim-panel {
  color: red;
  font-weight: 700;
  font-size: 20px;

  input {
    color: red;
  }
}
</style>
