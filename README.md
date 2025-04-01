# codemirror-vim-issue

### Reproduction Repository:
https://github.com/jiaopengzi/codemirror-vim-issue

### Problem
The issue occurs in lines 34-37 of the following code. Is this a bug or am I using it incorrectly? If it's a bug, please fix it. If not, please point out where I went wrong. Thank you very much.

```vue
<script setup lang="ts">
import { basicSetup, EditorView } from 'codemirror'
import { vim, getCM } from '@replit/codemirror-vim'
import { showPanel, type Panel } from '@codemirror/view'

import { onMounted, useTemplateRef } from 'vue'

defineOptions({ name: 'EditorCodemirror' })

const codemirrorRef = useTemplateRef<HTMLElement | null>('codemirrorRef')

function getVimMode(view: EditorView): string {
  const cm = getCM(view)
  if (cm && cm.state.vim && cm.state.vim.mode) {
    return `--${cm.state.vim.mode}--`
  }
  return ''
}

function bottomPanel(view: EditorView): Panel {
  const dom: HTMLElement | null = document.createElement('div')
  dom.textContent = getVimMode(view)

  const oldVimMode = getVimMode(view)

  console.log('old:', oldVimMode)

  return {
    top: false,
    dom,
    update(update) {
      const newVimMode = getVimMode(update.view)

      // When pressing ESC to exit insert mode into normal mode, it still prints "new: --insert--"
      // Logically it should print "new: --normal--" after ESC in insert mode
      // Is this a bug?
      // Note: Pressing ESC in non-insert modes correctly prints "new: --normal--"

      console.log('new:', newVimMode)
      dom.textContent = getVimMode(update.view)
    },
  }
}

let cmView: EditorView = null!

const initializeCodeMirror = () => {
  cmView = new EditorView({
    doc: '',
    extensions: [vim(), basicSetup, showPanel.of(bottomPanel)],
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
  height: 600px;
  width: 800px;
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.cm-editor {
  height: 100%;
  width: 100%;
}
</style>
```



### Project Setup

```sh
pnpm install
```

### Compile and Hot-Reload for Development

```sh
pnpm dev
```
