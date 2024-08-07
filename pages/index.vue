<template>
  <div>
    <HeaderTop @active-link="handleActiveLink" />
    <HeaderBottom v-show="selectedLink === 'main'" :checkIsActive="checkIsActive" v-model="selectedFont"
      :editor="editor" @trig-func="allFunctions" @font-selected="selectFontHandler"
      @selected-color="selectedColorHandler" @line-height="lineHeightHandler" />
    <HeaderBottom2 v-show="selectedLink === 'paste'" @trig-table="trigTableHandler"></HeaderBottom2>
    <HeaderBottom3 v-show="selectedLink === 'pageLayout'"></HeaderBottom3>
    <div class="flex flex-col items-center justify-center py-12 bg-gray-100">
      <div @contextmenu.prevent="handleRightClick" class="w-[100%] h-[100%] flex items-center justify-center">


        <horizontal-ruler class=""></horizontal-ruler>
        <TipTapEditor class="w-[816px] h-[1056px]" :editor="editor" />
      </div>
    </div>
    <div class="h-[100px]"></div>
  </div>
</template>

<script>
import { Editor, Extension } from '@tiptap/vue-2'
import StarterKit from '@tiptap/starter-kit'
import { Color } from '@tiptap/extension-color'
import TextAlign from '@tiptap/extension-text-align'
import TextStyle from '@tiptap/extension-text-style'
import BulletList from '@tiptap/extension-bullet-list'

// import { Extension } from "@tiptap/core";
import FontFamily from '@tiptap/extension-font-family'
import Underline from '@tiptap/extension-underline'
import Image from '@tiptap/extension-image'
import Strike from '@tiptap/extension-strike'
import Superscript from '@tiptap/extension-superscript'
import ListItem from '@tiptap/extension-list-item'

import Subscript from '@tiptap/extension-subscript'


import Table from '@tiptap/extension-table'
import TableCell from '@tiptap/extension-table-cell'
import TableHeader from '@tiptap/extension-table-header'
import TableRow from '@tiptap/extension-table-row'

import HeaderBottom from '../components/Header/HeaderBottom.vue'
import HeaderTop from '../components/Header/HeaderTop.vue'
import TipTapEditor from '../components/TipTapEditor.vue'
import HeaderBottom2 from '../components/Header/HeaderBottom2.vue'
import HeaderBottom3 from '../components/Header/HeaderBottom3.vue'

import HorizontalRuler from '../components/HorizontalRuler.vue'
import '@tiptap/extension-text-style'

const FontSize = Extension.create({
  name: 'fontSize',
  addOptions() {
    return {
      types: ['textStyle'],
    }
  },
  addGlobalAttributes() {
    return [
      {
        types: this.options.types,
        attributes: {
          fontSize: {
            default: null,
            parseHTML: (element) =>
              element.style.fontSize.replace(/['"]+/g, ''),
            renderHTML: (attributes) => {
              if (!attributes.fontSize) {
                return {}
              }
              return {
                style: `font-size: ${attributes.fontSize}`,
              }
            },
          },
        },
      },
    ]
  },
  addCommands() {
    return {
      setFontSize:
        (fontSize) =>
          ({ chain }) => {
            return chain()
              .setMark('textStyle', { fontSize: fontSize + 'px' })
              .run()
          },
      unsetFontSize:
        () =>
          ({ chain }) => {
            return chain()
              .setMark('textStyle', { fontSize: null })
              .removeEmptyTextStyle()
              .run()
          },

      increaseByTwo:
        () =>
          ({ chain, state }) => {
            console.log(state)
            // const textStyleMark = state.marks.textStyle
            // const currentFontSize = textStyleMark
            //   ? textStyleMark.fontSize
            //   : '12px'
            // const increasedFontSize = parseInt(currentFontSize) + 2
            // return chain()
            //   .setMark('textStyle', { fontSize: increasedFontSize + 'px' })
            //   .run()
          },
    }
  },
})

const LineHeight = Extension.create({
  name: 'lineHeight',

  addOptions() {
    return {
      types: ['heading', 'paragraph'],
      heights: ['100%', '115%', '150%', '200%', '250%', '300%'],
      defaultHeight: '100%',
    }
  },

  addGlobalAttributes() {
    return [
      {
        types: this.options.types,
        attributes: {
          lineHeight: {
            default: this.options.defaultHeight,
            parseHTML: (element) =>
              element.style.lineHeight || this.options.defaultHeight,
            renderHTML: (attributes) => {
              if (attributes.lineHeight === this.options.defaultHeight) {
                return {}
              }
              return { style: `line-height: ${attributes.lineHeight}` }
            },
          },
        },
      },
    ]
  },

  addCommands() {
    return {
      setLineHeight:
        (height) =>
          ({ commands }) => {
            if (!this.options.heights.includes(height)) {
              return false
            }
            return this.options.types.every((type) =>
              commands.updateAttributes(type, { lineHeight: height })
            )
          },

      unsetLineHeight:
        () =>
          ({ commands }) => {
            return this.options.types.every((type) =>
              commands.resetAttributes(type, 'lineHeight')
            )
          },
    }
  },
})

// const CustomTableCell = TableCell.extend({
//   addAttributes() {
//     return {
//       // extend the existing attributes …
//       ...this.parent?.(),

//       // and add a new one …
//       backgroundColor: {
//         default: null,
//         parseHTML: element => element.getAttribute('data-background-color'),
//         renderHTML: attributes => {
//           return {
//             'data-background-color': attributes.backgroundColor,
//             style: `background-color: ${attributes.backgroundColor}`,
//           }
//         },
//       },
//     }
//   },
// })
export default {
  name: 'IndexPage',

  components: {
    HeaderTop,
    HeaderBottom,
    TipTapEditor,
    HorizontalRuler,
    HeaderBottom2,
    HeaderBottom3
  },

  data() {
    return {
      editor: null,
      options: ['Option 1', 'Option 2', 'Option 3'],
      fontSizeOptions: [
        8, 10, 12, 14, 16, 18, 20, 24, 28, 32, 36, 40, 44, 48, 52, 56, 60, 64,
        68, 72,
      ],
      secondMenu: false,
      selectedFont: null,
      selectedFontSize: null,
      selectedFontSize1: null,
      selectedFontSizeFromChild: null,
      selectedColor: null,
      selectedLineHeight: null,
      activeIndex: null,
      selectedLink: 'main',
    }
  },
  mounted() {
    this.editor = new Editor({
      content: ``,
      extensions: [
        StarterKit,
        TextStyle,
        FontSize,
        FontFamily,
        Underline,
        Strike,
        Superscript,
        Subscript,
        ListItem,
        BulletList,
        Image,
        LineHeight,
        TextAlign.configure({
          types: ['heading', 'paragraph'],
        }),
        Color,
        Table.configure({
          resizable: true,
        }),
        TableRow,
        TableHeader,
        TableCell,
      ],
    })
  },
  beforeDestroy() {
    this.editor.destroy()
  },
  methods: {

    handleRightClick(event) {
      // Your custom logic goes here
      alert('Right-click detected!');
      console.log('Right-click event:', event);
    },

    handleActiveLink(link) {
      this.selectedLink = link
    },

    lineHeightHandler(comingValue) {
      this.selectedLineHeight = comingValue
    },
    selectedColorHandler(selectedColor) {
      this.selectedColor = selectedColor
    },
    selectFontHandler(selectedFont) {
      this.selectedFont = selectedFont
    },
    updateFontText(newVal) {
      // this.selectedFont = newVal
    },

    increaseFontSize() {
      // const currentFontSize = this.editor.state
      // console.log('Current fontsize: ', this.editor.state)
      this.editor.chain().focus().increaseByTwo().run()
    },

    changeFontFamily() {
      // this.editor.chain().focus().setFontFamily('Comic Sans MS, Comic Sans').run()
      this.editor.commands.setFontFamily('Bookman Old Style')
    },

    changeFontSize(fontSize) {
      console.log('Selected font size:', this.selectedFontSize)
      this.editor.commands.setFontSize(this.selectedFontSize)
    },

    checkIsActive(className) {
      return this.editor.isActive(className)
    },

    allFunctions(val, selectedFontSize) {
      this.selectedFontSize = selectedFontSize
      console.log('Last Value', this.selectedFontSizeFromChild)
      if (val === 'undo') {
        console.log('Undo is working')
        this.editor.chain().focus().undo().run()
      }
      if (val === 'redo') {
        console.log('Redo is working')
        this.editor.chain().focus().redo().run()
      }

      if (val === 'fonts') {
        this.editor.commands.setFontFamily(`${this.selectedFont}`)
      }

      if (val === 'changeSize') {
        this.editor.commands.setFontSize(this.selectedFontSize)
      }

      if (val === 'cut') {
        const text = this.editor.getHTML() // Get the HTML content of the editor

        // Remove <p> tags from the text content
        const textContent = text.replace(/<\/?p>/g, '')

        // Copy the text content to the clipboard
        navigator.clipboard
          .writeText(textContent)
          .then(() => {
            console.log('Content cut and copied to clipboard')
            // Remove the content from the editor
            this.editor.commands.deleteSelection()
          })
          .catch((err) => {
            console.error('Error cutting content:', err)
          })
      } else if (val === 'copy') {
        const text = this.editor.getHTML() // Get the HTML content of the editor

        const textContent = text.replace(/<\/?p>/g, '')
        console.log(textContent)

        navigator.clipboard
          .writeText(text)
          .then(() => {
            console.log('Content copied to clipboard')
          })
          .catch((err) => {
            console.error('Error copying content:', err)
          })
      } else if (val === 'paste') {
        navigator.clipboard
          .readText()
          .then((copiedText) => {
            const textContent = copiedText.replace(/<\/?p>/g, '')
            console.log('Content from clipboard:', textContent) // Log the copied text
            // Insert the copied text into the editor at the current selection point
            this.editor.commands.insertContent(textContent)
            console.log('Content pasted from clipboard')
          })
          .catch((err) => {
            console.error('Error pasting content:', err)
          })
      } else if (val === 'print') {
        const printWindow = window.open('', '_blank')
        // Copy editor content to the new window
        const content = this.editor.getHTML()
        printWindow.document.open()
        printWindow.document.write(content)
        printWindow.document.close()

        // Print the new window
        printWindow.print()
      }

      if (val === 'bold') {
        this.editor.chain().focus().toggleBold().run()
      }

      if (val === 'italic') {
        this.editor.chain().focus().toggleItalic().run()
      }

      if (val === 'underline') {
        this.editor.chain().focus().toggleUnderline().run()
      }

      if (val === 'strikethrough') {
        this.editor.chain().focus().toggleStrike().run()
        this.editor.isActive('strike')
        this.checkIsActive('strike')
      }

      if (val === 'subscript') {
        this.editor.chain().focus().toggleSubscript().run()
      }

      if (val === 'superscript') {
        this.editor.chain().focus().toggleSuperscript().run()
      }

      if (val === 'unordered') {
        console.log('bullet List')
        this.editor.chain().focus().toggleBulletList().run()
      }

      if (val === 'list-ordered') {
        console.log('List Ordered is working')
        // this.editor.commands.toggleOrderedList()
        this.editor.chain().focus().toggleOrderedList().run()
      }

      if (val === 'align-left') {
        console.log('align left')
        this.editor.chain().focus().setTextAlign('left').run()
      }
      if (val === 'align-center') {
        console.log('align center')
        this.editor.chain().focus().setTextAlign('center').run()
      }
      if (val === 'align-right') {
        console.log('align right')
        this.editor.chain().focus().setTextAlign('right').run()
      }
      if (val === 'align-justify') {
        console.log('align justify')
        this.editor.chain().focus().setTextAlign('justify').run()
      }

      if (val === 'textColor') {
        this.editor.chain().focus().setColor(this.selectedColor).run()
      }

      if (val === 'clearFormat') {
        this.editor.commands.unsetAllMarks()
      }

      if (val === 'line-height') {
        console.log('Ishladi', this.selectedLineHeight)
        this.editor.chain().focus().setLineHeight('150%').run()
      }

      if (val === 'addImage') {
        console.log('Image is working ...')
        const url = window.prompt('URL')

        if (url) {
          this.editor.chain().focus().setImage({ src: url }).run()
        }
      }

      // if (val === 'cut') {
      //   console.log('Cut is working')
      //   const text = this.editor.getHTML() // Get the HTML content of the editor

      //   // Remove <p> tags from the text content
      //   const textContent = text.replace(/<\/?p>/g, '')

      //   // Copy the text content to the clipboard
      //   navigator.clipboard
      //     .writeText(textContent)
      //     .then(() => {
      //       console.log('Content cut and copied to clipboard')
      //       // Remove the content from the editor
      //       this.editor.commands.deleteSelection()
      //     })
      //     .catch((err) => {
      //       console.error('Error cutting content:', err)
      //     })
      // }
      // if (val === 'copy') {
      //   console.log('Copy is working')
      //   const text = this.editor.getHTML() // Get the HTML content of the editor

      //   const textContent = text.replace(/<\/?p>/g, '')
      //   console.log(textContent)

      //   navigator.clipboard
      //     .writeText(text)
      //     .then(() => {
      //       console.log('Content copied to clipboard')
      //     })
      //     .catch((err) => {
      //       console.error('Error copying content:', err)
      //     })
      // }
      // if (val === 'paste') {
      //   console.log('Paste is working')
      //   navigator.clipboard
      //     .readText()
      //     .then((copiedText) => {
      //       const textContent = copiedText.replace(/<\/?p>/g, '')
      //       console.log('Content from clipboard:', textContent) // Log the copied text
      //       // Insert the copied text into the editor at the current selection point
      //       this.editor.commands.insertContent(textContent)
      //       console.log('Content pasted from clipboard')
      //     })
      //     .catch((err) => {
      //       console.error('Error pasting content:', err)
      //     })
      // }
      if (val === 'increaseSize') {
        // this.editor.chain().focus().increaseFontSize().run();
      }
    },

    updateFont({ font, size }) {
      this.selectedFont = font
      this.selectedFontSize = size
    },

    trigTableHandler(funcName) {
      if (funcName === 'insertTable') {
        console.log(' ishlavoti  ')
        this.editor.chain().focus().insertTable({ rows: 3, cols: 3, withHeaderRow: true }).run()
      }

      if (funcName === 'deleteTable') {
        this.editor.chain().focus().deleteTable().run()
      }

      if (funcName === 'addColumnBefore') {
        this.editor.chain().focus().addColumnBefore().run()
      }

      if (funcName === 'addColumnAfter') {
        this.editor.chain().focus().addColumnAfter().run()
      }

      if (funcName === 'deleteColumn') {
        this.editor.chain().focus().deleteColumn().run()
      }

      if (funcName === 'addRowBefore') {
        this.editor.chain().focus().addRowBefore().run()
      }

      // if(funcName === 'addRowBefore') {
      //   this.editor.chain().focus().addRowBefore().run()
      // }

      if (funcName === 'addRowAfter') {
        this.editor.chain().focus().addRowAfter().run()
      }

      if (funcName === 'deleteRow') {
        this.editor.chain().focus().deleteRow().run()
      }
    }
  },
}

// Define FontSize extension
</script>

<style lang="css">
@import url('../assets/css/mian.css');

.khzuck {
  font-family: MyCustomFont, sans-serif;
}

/* ".tiptap {
  margin: 1rem 0;
}" */

.tiptap>*+* {
  margin-top: 0.75em;
}

.tiptap ul,
.tiptap ol {
  padding: 0 1rem;
  color: #000;
}

.tiptap h1,
.tiptap h2,
.tiptap h3,
.tiptap h4,
.tiptap h5,
.tiptap h6 {
  line-height: 1.1;
}

.tiptap code {
  background-color: rgba(97, 97, 97, 0.1);
  color: #616161;
}

.tiptap pre {
  background: #0d0d0d;
  color: #fff;
  font-family: 'JetBrainsMono', monospace;
  padding: 0.75rem 1rem;
  border-radius: 0.5rem;
}

.tiptap pre code {
  color: inherit;
  padding: 0;
  background: none;
  font-size: 0.8rem;
}

.tiptap img {
  max-width: 100%;
  height: auto;
}

.tiptap blockquote {
  padding-left: 1rem;
  border-left: 2px solid rgba(13, 13, 13, 0.1);
}

.tiptap hr {
  border: none;
  border-top: 2px solid rgba(13, 13, 13, 0.1);
  margin: 2rem 0;
}

/* Table-specific styling */
.tiptap table {
  border-collapse: collapse;
  table-layout: fixed;
  width: 100%;
  margin: 0;
  overflow: hidden;
}

.tiptap table td,
.tiptap table th {
  min-width: 1em;
  border: 2px solid #ced4da;
  padding: 3px 5px;
  vertical-align: top;
  box-sizing: border-box;
  position: relative;
}

.tiptap table td>*,
.tiptap table th>* {
  margin-bottom: 0;
}

.tiptap table th {
  font-weight: bold;
  text-align: left;
  background-color: #f1f3f5;
}

.tiptap table .selectedCell:after {
  z-index: 2;
  position: absolute;
  content: "";
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background: rgba(200, 200, 255, 0.4);
  pointer-events: none;
}

.tiptap table .column-resize-handle {
  position: absolute;
  right: -2px;
  top: 0;
  bottom: -2px;
  width: 4px;
  background-color: #adf;
  pointer-events: none;
}

.tiptap table p {
  margin: 0;
}

.tableWrapper {
  overflow-x: auto;
}

.resize-cursor {
  cursor: ew-resize;
  cursor: col-resize;
}
</style>
