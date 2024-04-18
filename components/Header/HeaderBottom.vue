<template>
  <ul class="bg-[] border-b border-b-solid py-1 px-2 flex items-center gap-3 flex-wrap">
    <li>
      <button title="Cохранить" class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
        <img class="w-[18px]" src="../../assets/svg/save.svg" alt="save" />
        <span class="">Сохранить</span>
      </button>
    </li>
    <li>
      <button title="Скачать" class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
        <img class="w-[18px]" src="../../assets/svg/download.svg" alt="save" />
        <span class="">Скачать</span>
      </button>
    </li>
    <li>
      <button title="Печать" @click="$emit('trig-func', 'print')"
        class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
        <img class="w-[18px]" src="../../assets/svg/print.svg" alt="save" />
        <span class="">Печать</span>
      </button>
    </li>
    <li class="flex gap-1">
      <button title="Отменить" :disabled="disableUndoButton" :class="{ 'disabled-button': disableUndoButton }"
        class="flex items-center gap-2 py-1 px-2 transition hover:bg-[#edf5ef] rounded"
        @click="$emit('trig-func', 'undo')">
        <img class="w-[18px] undo-img" src="../../assets/svg/rotate-left.svg" alt="save" />
      </button>
      <button title="Вернуть" :disabled="disableRedoButton" :class="{ 'disabled-button': disableRedoButton }"
        @click="$emit('trig-func', 'redo')"
        class="flex items-center gap-2 py-1 px-2 transition hover:bg-[#edf5ef] rounded">
        <img class="w-[18px]" src="../../assets/svg/rotate-right.svg" alt="save" />
      </button>
    </li>
    <li>
      <button @click="$emit('trig-func', 'cut')" title="Вырезать"
        class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
        <img class="w-[18px]" src="../../assets/svg/scissors.svg" alt="save" />
      </button>
    </li>
    <li>
      <button @click="$emit('trig-func', 'copy')" title="Копировать"
        class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
        <img class="w-[18px]" src="../../assets/svg/copy.svg" alt="save" />
      </button>
    </li>
    <li>
      <button @click="$emit('trig-func', 'paste')" title="Вставить"
        class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
        <img class="w-[18px]" src="../../assets/svg/paste-regular.svg" alt="save" />
      </button>
    </li>
    <li>
      <button class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded w-[150px]">
        <el-select v-model="selectedFont" placeholder="Calibri" @change="selectedFontHandler">
          <el-option v-for="item in fontStyles" :key="item" :label="item" :value="item">
          </el-option>
        </el-select>
      </button>
    </li>
    <li>
      <button class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
        <el-select class="w-[68px]" v-model="selectedFontSize" placeholder="14" @change="changeSizeHandler">
          <el-option v-for="item in fontSizeOptions" :key="item" :label="item" :value="item">
          </el-option>
        </el-select>
      </button>
    </li>
    <li>
      <button @click="$emit('trig-func', 'increaseSize')" title="Увеличить размер шрифта"
        class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
        A
      </button>
    </li>
    <li>
      <button title="Уменьшить размер шрифта"
        class="text-[12px] flex items-end gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
        A
      </button>
    </li>
    <li class="flex gap-2 items-center">
      <button title="цвет текста"
        class="transition hover:bg-[#edf5ef] rounded cursor-pointer py-1 px-3 flex align-center">
        <label for="colorPicker"></label>
        <input id="colorPicker" v-model="selectedColor" type="color" @input="handleColorChange" />
        <!-- <div v-if="selectedColor" style="margin-top: 10px">
          <span>Selected color: </span>
          <div
            :style="{
              width: '20px',
              height: '20px',
              backgroundColor: selectedColor,
              display: 'inline-block',
            }"
          ></div>
        </div> -->
      </button>
    </li>
    <li>
      <button title="Настройки шрифта" class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
        <span class="relative inline-block">A
          <img class="absolute bottom-0 right-[-4px] w-[12px]" src="../../assets/svg/paintbrush.svg"
            alt="save" /></span>
      </button>
    </li>
    <li class="border-l-2 border-left-[#ccc] pl-2">
      <button title="Полужирный"
        class="font-bold flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded"
        @click="$emit('trig-func', 'bold')">
        <img class="w-[18px]" src="../../assets/svg/bold.svg" alt="" />
      </button>
    </li>
    <li>
      <button title="Курсив" class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded"
        @click="$emit('trig-func', 'italic')">
        <img class="w-[18px]" src="../../assets/svg/italic.svg" alt="" />
      </button>
    </li>

    <li>
      <button title="Подчеркнутый"
        class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]"
        @click="$emit('trig-func', 'underline')">
        <img class="w-[18px]" src="../../assets/svg/underline.svg" alt="sd" />
        <!-- <i  class="ri-underline text-[#000]"></i> -->
      </button>
    </li>
    <li>
      <button title="Зачеркнутый" :class="{ 'is-active': checkIsActive }"
        class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]"
        @click="$emit('trig-func', 'strikethrough')">
        <img class="w-[18px]" src="../../assets/svg/strikethrough.svg" alt="sd" />
        <!-- <i  class="ri-underline text-[#000]"></i> -->
      </button>
    </li>
    <li>
      <button title="Подстрочный"
        class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]"
        @click="$emit('trig-func', 'subscript')">
        <img class="w-[18px]" src="../../assets/svg/subscript.svg" alt="sd" />
        <!-- <i  class="ri-underline text-[#000]"></i> -->
      </button>
    </li>
    <li>
      <button title="Надстрочный"
        class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]"
        @click="$emit('trig-func', 'superscript')">
        <img class="w-[18px]" src="../../assets/svg/superscript.svg" alt="sd" />
        <!-- <i  class="ri-underline text-[#000]"></i> -->
      </button>
    </li>
    <li>
      <button title="Очистить форматирование"
        class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded tex-[#000]"
        @click="$emit('trig-func', 'clearFormat')">

        <!-- <img class="w-[18px]" src="../../assets/svg/font-family.svg" alt="sd" /> -->
        <img class="w-[18px]" src="../../assets/svg/icon-clear.png" alt="">
        <!-- <i  class="ri-underline text-[#000]"></i> -->
      </button>
    </li>
    <li class="border-l-2 border-left-[#ccc] pl-2">
      <button class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]"
        @click="$emit('trig-func', 'unordered')">
        <img class="w-[18px]" src="../../assets/svg/list-unordered.svg" alt="sd" />
      </button>
    </li>
    <li>
      <button class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]"
        @click="$emit('trig-func', 'list-ordered')">
        <img class="w-[18px]" src="../../assets/svg/list-ordered.svg" alt="sd" />
      </button>
    </li>
    <li class="border-r-2 border-[#ccc] pr-2">
      <button class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]"
        @click="$emit('trig-func', 'align-left')">
        <img class="w-[18px]" src="../../assets/svg/align-left.svg" alt="sd" />
      </button>
    </li>
    <li class="border-r-2 border-[#ccc] pr-2">
      <button class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]"
        @click="$emit('trig-func', 'align-center')">
        <img class="w-[18px]" src="../../assets/svg/align-center.svg" alt="sd" />
      </button>
    </li>
    <li class="border-r-2 border-[#ccc] pr-2">
      <button class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]"
        @click="$emit('trig-func', 'align-right')">
        <img class="w-[18px]" src="../../assets/svg/align-right.svg" alt="sd" />
      </button>
    </li>
    <li class="border-r-2 border-[#ccc] pr-2">
      <button class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]"
        @click="$emit('trig-func', 'align-justify')">
        <img class="w-[18px]" src="../../assets/svg/align-justify.svg" alt="sd" />
      </button>
    </li>
    <li>
      <button class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]">
        <img class="w-[18px]" src="../../assets/svg/paragraph.svg" alt="sd" />
      </button>
    </li>
    <li>
      <button class="flex items-center gap-2 py-1 px-1 transition hover:bg-[#edf5ef] rounded text-[#000]"
        @click="$emit('trig-func', 'line-height')">

        <div class="dynamic-select cursor-pointer">
          <!-- <h1>hello</h1> -->
          <div class="select-header" @click="toggleOptions">
            <!-- <div class="selected-option">{{ selectedOption || 'Select an option' }}</div> -->
            <svg class="w-[24px]" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor">
              <path
                d="M11 4H21V6H11V4ZM6 7V11H4V7H1L5 3L9 7H6ZM6 17H9L5 21L1 17H4V13H6V17ZM11 18H21V20H11V18ZM9 11H21V13H9V11Z">
              </path>
            </svg>
          </div>
          <div v-if="isOpen" class="options">
            <div v-for="(option, index) in lineHeightOptions" :key="index" class="option bg-[#fff] px-2 py-1"
              @click="handleOptionClick(option)">
              {{ option }}
            </div>
          </div>
        </div>
      </button>
    </li>
    <li>
      <button class="border border-[#ccc] flex items-center gap-2 py-1 px-2 transition hover:bg-[#edf5ef] rounded"
        @click="$emit('trig-func', 'addImage')">

        Add Image
      </button>
    </li>

  </ul>
</template>

<script>
export default {
  props: {
    editor: {
      type: Object,
      required: true,
    },

    checkIsActive: {
      type: Function,
      default: null,
    },

    // fontSizeOptions: {
    //   type: Number,
    // },
    // fontStyles: {
    //   type: String,
    // },
  },
  data() {
    return {
      lineHeightOptions: ['1.0', '1.5', '2.0'],
      fontSizeOptions: [
        8, 10, 12, 14, 16, 18, 20, 24, 28, 32, 36, 40, 44, 48, 52, 56, 60, 64,
        68, 72,
      ],
      fontStyles: [
        'Angsana New',
        'Arial',
        'Arial Black',
        'Batang',
        'Book Antiqua',
        'Bookman Old Style',
        'Calibri',
        'Calibri Light',
        'Cambria',
        'Candara',
        'Century',
        'Century Gothic',
        'Century Schoolbook',
        'Comic Sans MS',
        'Consolas',
        'Constantia',
        'Corbel',
        'Cordia New',
        'Courier New',
        'DaunPenh',
        'Dotum',
        'FangSong',
        'Franklin Gothic Book',
        'Franklin Gothic Medium',
        'Garamond',
        'Gautami',
        'Georgia',
        'Gill Sans MT',
        'Gulim',
        'GungSuh',
        'Impact',
        'Iskoola Pota',
        'Iskoola Pota',
        'KaiTi',
        'Kalinga',
        'Kartika',
        'Latha',
        'Lucida Console',
        'Lucida Sans',
        'Lucida Sans Unicode',
        'MS Gothic',
        'MS Mincho',
        'MS PGothic',
        'MS PMincho',
        'Malgun Gothic',
        'Mangal',
        'Meiryo',
        'Microsoft JhengHei',
        'Microsoft YaHei',
        'MingLiU',
        'MingLiU_HKSCS',
        'Nyala',
        'PMingLiU',
        'PMingLiU-ExtB',
        'Palatino Linotype',
        'Raavi',
        'Rockwell',
        'Segoe UI',
        'Segoe UI Light',
        'Shruti',
        'SimHei',
        'SimSun',
        'SimSun-ExtB',
        'Sylfaen',
        'TW Cen MT',
        'Tahoma',
        'Times',
        'Times New Roman',
        'Trebuchet MS',
        'Tunga',
        'Verdana',
        'Vrinda',
      ],
      isOpen: false,
      selectedOption: null,
      selectedColor: '#000000',

      selectedFont: 'Calibri',
      selectedFontSize: '14',
    }
  },
  computed: {
    placeholderText() {
      // Compute the placeholder text dynamically based on the selected font size
      return this.selectedFontSize
    },
    disableUndoButton() {
      // Check if editor is not null and has the can() method
      return (
        !this.editor ||
        !this.editor.can ||
        !this.editor.can().chain().focus().undo().run()
      )
    },
    disableRedoButton() {
      return (
        !this.editor ||
        !this.editor.can ||
        !this.editor.can().chain().focus().redo().run()
      )
    },
  },
  watch: {
    selectedFont(newVal) {
      console.log('Selected font:', newVal)
      this.$emit('update-fond', newVal)
    },

    selectedFontSize(newVal) {
      console.log('Selected Font Size Child: ', newVal)
      this.$emit('input', newVal)
    },
  },
  methods: {
    toggleOptions() {
      this.isOpen = !this.isOpen;
    },
    handleOptionClick(option) {
      this.selectedOption = option;
      this.isOpen = false;
      console.log(option)
      this.$emit('line-height', option)
    },
    handleColorChange(event) {
      // Do something with the selected color
      this.$emit('trig-func', 'textColor')
      console.log('Selected color:', this.selectedColor)
      this.$emit('selected-color', this.selectedColor)
    },
    changeSizeHandler() {
      console.log('Selected Size in the Child ', this.selectedFontSize)
      this.$emit('trig-func', 'changeSize', this.selectedFontSize)
    },
    selectedFontHandler() {
      this.$emit('trig-func', 'fonts')
      this.$emit('font-selected', this.selectedFont)
    },
    fontFunctionHandler() {
      console.log(this.selectedFont)
    },
  },
}
</script>

<style lang="scss" scoped>
.disabled-button {
  opacity: 0.6;
  cursor: not-allowed;
}

.undo-img {
  fill: rgb(255, 238, 127);
}

.is-active {
  background: #d6cccc;
}

.option {
  box-shadow: rgba(60, 64, 67, 0.3) 0px 1px 2px 0px, rgba(60, 64, 67, 0.15) 0px 2px 6px 2px;
}

.dynamic-select {
  position: relative;
  display: inline-block;
}

.options {
  width: 60px;
  text-align: start;
  position: absolute;
  top: 110%;
  left: -5px;
  z-index: 999;
}
</style>