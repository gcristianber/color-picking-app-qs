<template>
    <div id="color-preview" class="relative text-white rounded-lg flex items-center justify-center h-96"
        :class="picked.length === 0 ? 'bg-gray-200 text-gray-500' : ''"
        :style="{ backgroundColor: '#' + picked.hex_code, color: getTextColor(picked.hex_code) }"
        @mouseover="toggleCopyContent(true, picked.id)" @mouseleave="toggleCopyContent(false)">
        <p v-if="picked.length === 0">Select a color to preview</p>

        <ul v-else class="text-center">
            <li class="uppercase font-semibold text-[48px]">{{ picked.hex_code }}</li>
            <li>{{ picked.name }} - {{ picked.color_code }}</li>
            <!-- <li>color code: {{ picked.color_code }}</li> -->
        </ul>

        <div class="absolute right-0 bottom-0 py-4 px-8 rounded-tl-md bg-opacity-20 bg-slate-400 cursor-pointer"
            v-if="copyContentFlag && picked != 0" @click="jsonContent">
            <div class="flex items-center gap-2">
                <span class="material-symbols-outlined">
                    content_copy
                </span>
                <p>Copy JSON content</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['picked'],
    data() {
        return {
            copyContentFlag: false,
        }
    },
    methods: {
        jsonContent() {
            let data = {
                name: this.picked.name,
                hex_code: this.picked.hex_code,
                color_code: this.picked.color_code
            }

            this.$emit('clipboard', data)
        },
        toggleCopyContent(flag) {
            this.copyContentFlag = flag
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
    }
}
</script>