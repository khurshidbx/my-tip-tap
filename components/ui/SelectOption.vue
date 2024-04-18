<template>
  <div class="dynamic-select cursor-pointer">
    <h1>hello</h1>
    <div class="select-header" @click="toggleOptions">
      <div class="selected-option">{{ selectedOption || 'Select an option' }}</div>
      <svg class="w-[24px]" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"><path d="M11 4H21V6H11V4ZM6 7V11H4V7H1L5 3L9 7H6ZM6 17H9L5 21L1 17H4V13H6V17ZM11 18H21V20H11V18ZM9 11H21V13H9V11Z"></path></svg>
    </div>
    <div v-if="isOpen" class="options">
      <div v-for="(option, index) in options" :key="index" class="option bg-[#fff] px-2 py-1" @click="handleOptionClick(option)">
        {{ option }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    options: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      isOpen: false,
      selectedOption: null
    };
  },
  methods: {
    toggleOptions() {
      this.isOpen = !this.isOpen;
    },
    handleOptionClick(option) {
      this.selectedOption = option;
      this.isOpen = false;
      console.log(option)
    }
  }
};
</script>

<style scoped>
/* Add your custom styling here */
.option {
  box-shadow: rgba(60, 64, 67, 0.3) 0px 1px 2px 0px, rgba(60, 64, 67, 0.15) 0px 2px 6px 2px;
}
.dynamic-select {
  position: relative;
  display: inline-block;
}
.options {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 999;
}
</style>