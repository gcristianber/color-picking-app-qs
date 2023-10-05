<template>
  <div class="lg:container mx-auto">
    <header class="my-5 text-center">
      <h1 class="text-xl font-semibold text-slate-700">Color Picking App - Quickstrike</h1>
      <p class="text-gray-500">Made with <span class=" text-rose-500">&#9825;</span></p>
    </header>
    <div class="flex flex-col gap-5">
      <ColorPreview :picked="picked" @clipboard="getClipboard" />
      <ColorList :list="colors" @picked-color="previewColor" />
    </div>
  </div>
  <Backdrop :properties="showBackdrop" :picked="picked" />
</template>

<script>
// Importing axios library for fetching data from endpoint
import axios from 'axios'

// Importing components
import ColorList from './components/ColorList.vue';
import ColorPreview from './components/ColorPreview.vue';
import Backdrop from './components/Backdrop.vue';

export default {
  components: {
    // Register components
    ColorList,
    ColorPreview,
    Backdrop
  },
  data() {
    return {
      // Container for the data colors
      colors: [],
      // Container for selected color to preview
      picked: [],
      showBackdrop: {
        toggle: false,
        backdropColor: {
          text: null,
          color: null,
        }
      },
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

    },
    getTextColor(hex) {
      // Check if hex is defined
      if (hex) {
        // Convert the hex color code to RGB
        const r = parseInt(hex.slice(0, 2), 16);
        const g = parseInt(hex.slice(2, 4), 16);
        const b = parseInt(hex.slice(4, 6), 16);

        // Calculate the relative luminance to determine text color
        const luminance = (0.299 * r + 0.587 * g + 0.114 * b) / 255;

        // Return a suitable text color based on luminance
        return luminance > 0.5 ? '#000000' : '#FFFFFF'; // Black for light backgrounds, white for dark backgrounds
      }
      // If hex is not defined, return a default text color
      return '#000000';
    },
    getClipboard(data) {
      if (!data) return

      // Convert data as a string
      let json = JSON.stringify(data)
      this.showBackdrop.backdropColor.text = data.name
      this.showBackdrop.backdropColor.color = data.hex_code
      this.showBackdrop.toggle = true
      navigator.clipboard.writeText(json)

      // Show the backdrop for 1.25 seconds
      setTimeout(() => {
        this.showBackdrop.toggle = false; // Revert showBackdrop to false after the specified interval
      }, 1250);
    }
  }
}
</script>
