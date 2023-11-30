<template>
    <div class="flex justify-center items-center flex-col gap-4 border-[#454545] border-4 border-dashed h-full w-full rounded-xl"
        @drop="onDrop">
        <svg xmlns="http://www.w3.org/2000/svg" height="24" viewBox="0 -960 960 960" width="24"
            class="fill-[#7D7D7E] w-10 h-10">
            <path
                d="M440-200h80v-167l64 64 56-57-160-160-160 160 57 56 63-63v167ZM240-80q-33 0-56.5-23.5T160-160v-640q0-33 23.5-56.5T240-880h320l240 240v480q0 33-23.5 56.5T720-80H240Zm280-520v-200H240v640h480v-440H520ZM240-800v200-200 640-640Z" />
        </svg>
        <div>
            <span class="text-[#F0F0F0]">Drop your file here or </span><button @click="onClickHandler" class="text-[#2C8478]">browse</button>
        </div>
    </div>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'

const emit = defineEmits<{
    (e: 'file-uploaded', value: string): void
}>()

const events = ['dragenter', 'dragover', 'dragleave', 'drop']

function readFile(file: File): Promise<string | ArrayBuffer | null> {
    return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = function () {
            resolve(reader.result)
        };
        reader.onerror = function (error) {
            reject(error)
        };
    })
}


function preventDefaults(e: Event) {
    e.preventDefault()
}

async function onDrop(e: DragEvent) {
    const file = e.dataTransfer?.files[0]
    if (file) {
        const fileURL = await readFile(file)
        if (fileURL) {
            emit('file-uploaded', fileURL as string)
        }
    }
}

function onClickHandler(ev: Event) {
    let el: HTMLInputElement = document.createElement("input");
    el.type = "file";
    el.accept = "image/*";
    el.addEventListener('change', async function (ev2: Event) {
        if (el?.files?.length) {
            const fileURL = await readFile(el.files[0])
            emit('file-uploaded', fileURL as string)
        }
    });

    el.click();
}


onMounted(() => {
    events.forEach((eventName) => {
        document.body.addEventListener(eventName, preventDefaults)
    })
})

onUnmounted(() => {
    events.forEach((eventName) => {
        document.body.removeEventListener(eventName, preventDefaults)
    })
})

</script>
