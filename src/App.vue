<script setup>
import { useDropzone } from "vue3-dropzone";
import { ref } from "vue";
import DropZone from "./components/DropZone.vue";
import Files from "./components/Files.vue";

const files = ref([]);
const uploading = ref(false);
const percentage = ref(0);

function onDrop(acceptFiles) {
    files.value = [...files.value, ...acceptFiles];
}

function removeFile(key)
{
    files.value.splice(key, 1);
}

function convert()
{
    uploading.value = true;

    setInterval(() => {
        percentage.value += .1;
    }, 10)
}

function download()
{
    if( percentage.value < 100 ) return;
}

const { getRootProps, getInputProps, isDragActive } = useDropzone({ onDrop });
</script>

<template>
    <div class="h-screen w-screen flex flex-col items-center justify-center bg-gray-200 p-5 md:p-0">
        <div class="bg-white shadow-md rounded-b-lg rounded-t-lg md:w-1/2 w-full overflow-hidden">
            <div class="bg-indigo-500 p-3 text-white text-xl font-semibold rounded-t-lg">
                Convertisseur
                <span class="text-gray-300 text-xs block">JPG to PNG</span>
            </div>  

            <div class="p-5">
                <div v-bind="getRootProps()" v-if="!uploading">
                    <input v-bind="getInputProps()" >
                    
                    <transition name="fade" :duration="300" mode="out-in">
                        <drop-zone v-if="files.length < 1" :isDragActive="isDragActive"></drop-zone>
            
                        <files v-else :files="files" @removeFile="removeFile" @convert="convert"></files>
                    </transition>
                </div>

                <transition name="fade">
                    <div v-if="uploading" class="text-center">
                        <h1 class="text-xl font-medium">Converting...</h1>

                        <div class="relative py-5">
                            <div class="overflow-hidden h-2 text-xs flex rounded bg-purple-200">
                                <div
                                :style="'width:' + percentage + '%'"
                                class="
                                    shadow-none
                                    flex flex-col
                                    text-center
                                    whitespace-nowrap
                                    text-white
                                    justify-center
                                    bg-indigo-500
                                "
                                ></div>
                            </div>
                        </div>         

                        <button 
                            class="bg-green-600 text-white py-2 text-md rounded-md hover:opacity-80 transition-opacity duration-300 w-full" 
                            :class="{'opacity-80 pointer-events-none': percentage < 100}"
                            @click.prevent="download"
                        >
                            Download my files    
                        </button>           
                    </div>
                </transition>
            </div>
        </div>
    </div>
</template>

<style lang="scss">
.dotted{
    background-image: url("data:image/svg+xml,%3csvg width='100%25' height='100%25' xmlns='http://www.w3.org/2000/svg'%3e%3crect width='100%25' height='100%25' fill='none' rx='10' ry='10' stroke='%233838CA7D' stroke-width='3' stroke-dasharray='9%2c 17%2c 12%2c 21' stroke-dashoffset='42' stroke-linecap='round'/%3e%3c/svg%3e");
    border-radius: 10px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
