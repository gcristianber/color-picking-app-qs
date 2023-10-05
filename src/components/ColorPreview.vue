<template>
    <div id="color-preview" class=" text-white rounded-lg flex items-center justify-center h-96"
        :class="picked.length === 0 ? 'bg-gray-200 text-gray-500' : ''"
        :style="{ backgroundColor: '#' + picked.hex_code, color: getTextColor(picked.hex_code) }">
        <p v-if="picked.length === 0">Select a color to preview</p>
        <ul v-else>
            <li>name: {{ picked.name }}</li>
            <li>hex: {{ picked.hex_code }}</li>
            <li>color code: {{ picked.color_code }}</li>
        </ul>
    </div>
</template>

<script>
export default {
    props: ['picked'],
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
    }
}
</script>