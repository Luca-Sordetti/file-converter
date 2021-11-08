<script setup>
const emit = defineEmits(["convert", "removeFile"]);
const props = defineProps({
    files: Array
});

function preview_file(file)
{
    if( file.type.indexOf("image") !== -1 ){
        var src = URL.createObjectURL(file);
        return src
    }
}

function removeFile(key)
{
    emit("removeFile", key);
}

function convert()
{
    emit("convert");
}
</script>

<template>
    <div class="flex flex-col gap-5">
        <transition-group name="fade" mode="in-out">
            <div v-for="(file, key) in files" :key="key" class="bg-indigo-500 text-white flex items-center gap-3">
                <div class="w-24 h-16 bg-red-100 relative hidden md:block">
                    <img class="absolute h-full w-full object-cover" :src="preview_file(file)" alt="">
                </div>

                <div class="flex justify-between items-center w-full p-3 md:p-0">
                    <div class="flex justify-center flex-col">
                        <p class="text-lg">{{ file.name }}</p>
                        <p class="text-xs">Size : {{ Math.round((file.size / 1024 / 1000) * 1000) / 1000 }} MB</p>
                    </div>

                    <button class="mr-4 hover:opacity-40 transition-opacity duration-300 text-white" @click.stop="removeFile(key)">
                        <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
                    </button>
                </div>
            </div>
        </transition-group>

        <button class="bg-green-600 text-white py-3 text-lg rounded-md hover:opacity-80 transition-opacity duration-300" @click.prevent="convert" >
            Convert to JPG
        </button>
    </div>    
</template>