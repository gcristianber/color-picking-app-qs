<template>
    <div id="color-list" class="flex flex-wrap gap-2 overflow-y-auto max-h-screen">
        <div class="p-5 rounded-md h-48 w-48 flex-grow flex-shrink cursor-pointer " v-for="color in  colors "
            :key="color.id" :style="{ backgroundColor: '#' + color.hex_code, color: getTextColor(color.hex_code) }"
            @click="previewColor(color.id)">
            <p class="text-xl uppercase tracking-widest font-semibold">{{ color.hex_code }}</p>
            <p>{{ color.name }}</p>
        </div>
    </div>
</template>

<script>
export default {
    props: ['list'],
    data() {
        return {
            colors: []
        }
    },
    //Use the beforeUpdate hook to fetch the props list after the component mounted.
    beforeUpdate() {
        this.colors = this.list
        // console.log('beforeUpdate', this.list)
    },
    methods: {
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
        previewColor(colorId) {
            // console.log(colorId)
            this.$emit('pickedColor', colorId)
        }
    }
}
</script>