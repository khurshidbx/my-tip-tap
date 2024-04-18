<template>
  <ul class="flex menu border-b border-solid border-gray-300">

    <header-item v-for="(item, index) in items" :key="index" :is-active="isActive(item.link)"
      @toggle-active="toggleActive(item.link)">
      {{ item.title }}
    </header-item>
  </ul>
</template>

<script>
import HeaderItem from './HeaderItem.vue'

export default {
  components: { HeaderItem },
  data() {
    return {
      items: [
        {
          title: 'Главная',
          link: 'main',
        },
        {
          title: 'Вставить',
          link: 'paste',
        },

        {
          title: 'Разметка страницы',
          link: 'pageLayout',
        },
        {
          title: 'Ссылки',
          link: 'links',
        },
        {
          title: 'Рассылки',
          link: 'newsLetters',
        },
        {
          title: 'Вид',
          link: 'view',
        },
        {
          title: 'Ижро',
          link: 'ijro',
        },
      ],
      activeIndex: null,
    }
  },

  mounted() {
    // Set the default activeIndex to the index of "Главная" when the component is mounted
    const defaultItem = 'Главная'
    this.activeIndex = this.items.indexOf(defaultItem)
  },
  methods: {
    isActive(index) {
      console.log(this.activeIndex)
      this.$emit('active-index', this.activeIndex)
      // Check if the current item is active based on its index
      return this.activeIndex === index

    },
    toggleActive(link) {
      // Toggle the active state of the clicked header-item
      if (this.activeIndex === link) {

        this.activeIndex = null // If the clicked item is already active, deactivate it
        this.$emit('active-link', null);
      } else {

        this.activeIndex = link // Activate the clicked item
        this.$emit('active-link', link);
      }
    },
  },
}
</script>

<style scoped>
.menu {
  border-bottom: 1px solid #ddd;
}
</style>
