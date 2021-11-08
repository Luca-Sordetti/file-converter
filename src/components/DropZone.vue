<script setup>
const props = defineProps({
    isDragActive: Boolean
});

</script>

<template>
    <div class="text-lg text-gray-800 text-center animate-pulse dotted p-20 md:p-40">
        <img src="/cloud-upload.svg" class="block mx-auto mb-5" width="125" alt="">
        <p v-if="!isDragActive">
            Drag 'n' drop some files here, or click to select files
        </p>

        <p v-else>
            Drop the files here ...
        </p>
    </div>
</template>

<style>

</style>