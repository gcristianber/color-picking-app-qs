<template>
  <div class="container mx-auto">
    <h1 class="text-xl/10 font-bold">Colors:</h1>
    <div class="flex gap-5">
      <ul class="flex-1 overflow-y-scroll max-h-screen">
        <li v-for="color in colors" :key="color.id"
          class="p-2 flex items-center justify-between first:rounded-t-lg last:rounded-b-lg border border-gray-300">
          <p>{{ color.name }}</p>
          <Button :colorPick="color.id" @picked-color="previewColor" />
        </li>
      </ul>
      <div class="flex-1">
        <ColorPreview :picked="picked" />
      </div>
    </div>
  </div>
</template>

<script>
// Importing axios library for fetching data from endpoint
import axios from 'axios'

// Importing components
import ColorPreview from './components/ColorPreview.vue';
import Button from './components/Button.vue';

export default {
  components: {
    // Register components
    ColorPreview,
    Button
  },
  data() {
    return {
      // Container for the data colors
      colors: [],
      // Container for selected color to preview
      picked: [],
    }
  },
  created() {
    //Use a created hook to render the colors
    this.fetchColors()
  },
  methods: {
    fetchColors() {
      axios.get('https://api.prolook.com/api/colors/prolook')
        .then((response) => {
          // Initialization of the responsed data from the endpoint
          let data = response.data

          // Guard clause, checking if the success property is true or false
          if (!data.success) return

          // Set the colors
          this.colors = response.data.colors
        })
    },
    previewColor(colorId) {
      // Searching the color from the colors[] array using find function
      this.picked = this.colors.find((element) => {
        return element.id == colorId
      })

    }
  }
}
</script>
