<template>
    <div class="absolute top-0 bg-emerald-500 min-h-screen w-full flex items-center justify-center"
        v-show="properties.toggle" :style="{ backgroundColor: '#' + properties.backdropColor.color }">
        <div class="w-full text-center">
            <div class=" mb-5">
                <p class=" text-[96px] tracking-wider uppercase font-semibold"
                    :style="{ backgroundColor: '#' + picked.hex_code, color: getTextColor(picked.hex_code) }">Success!</p>
            </div>
            <p class="uppercase text-[36px] tracking-wide font-semibold "
                :style="{ backgroundColor: '#' + picked.hex_code, color: getTextColor(picked.hex_code) }">{{
                    properties.backdropColor.text }}
            </p>
        </div>
        <p></p>
    </div>
</template>

<script>
export default {
    props: ['properties', 'picked'],
    mounted() {
        console.log(this.properties)
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
    }

}
</script>