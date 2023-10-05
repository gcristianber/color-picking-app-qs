<template>
    <button type="button" class="border py-1.5 px-2 rounded-md bg-blue-500 hover:bg-blue-600 text-white focus:ring-2"
        @click="previewColor(colorPick)">Preview</button>
</template>

<script>
export default {
    // Receive the prop passed
    props: ['colorPick'],
    methods: {
        previewColor(colorId) {
            this.$emit('pickedColor', colorId)
        }
    }
}
</script>