<template>
    <div class="h-screen flex flex-col">
        <AppHeader />
        <div class=" mt-8 flex items-center justify-center">
            <div class="mb-36 flex flex-row gap-36 flex-wrap justify-center">
                <div class="bg-[#363636] md:h-96 md:w-96 h-80 w-80 rounded-xl">
                    <FileUpload v-if="!imageFileBase64" @file-uploaded="onFileUpload" />
                    <div v-else class="flex items-center justify-center rounded-xl relative overflow-clip h-full">
                        <img class="md:max-w-96 md:max-h-96 max-w-80 max-h-80" v-bind:src="imageFileBase64" />
                        <button class="absolute top-3 right-3" @click="removeImage">
                            <svg class="fill-[#7D7D7E] inline-block" xmlns="http://www.w3.org/2000/svg" height="24"
                                viewBox="0 -960 960 960" width="24">
                                <path
                                    d="m336-280 144-144 144 144 56-56-144-144 144-144-56-56-144 144-144-144-56 56 144 144-144 144 56 56ZM480-80q-83 0-156-31.5T197-197q-54-54-85.5-127T80-480q0-83 31.5-156T197-763q54-54 127-85.5T480-880q83 0 156 31.5T763-763q54 54 85.5 127T880-480q0 83-31.5 156T763-197q-54 54-127 85.5T480-80Zm0-80q134 0 227-93t93-227q0-134-93-227t-227-93q-134 0-227 93t-93 227q0 134 93 227t227 93Zm0-320Z" />
                            </svg>
                        </button>
                    </div>
                </div>

                <div class="bg-[#363636] md:h-96 md:w-96 h-80 w-80 rounded-xl text-[#F0F0F0]">
                    <div v-if="isImageGenerationInProgress" class="md:h-96 md:w-96 h-80 w-80 flex flex-col justify-center items-center">
                        <div>
                            <div class="space-x-2">
                                <svg v-if="!promptComplete" xmlns="http://www.w3.org/2000/svg"
                                    class="fill-[#7D7D7E] inline-block animate-spin" height="24" viewBox="0 -960 960 960"
                                    width="24">
                                    <path
                                        d="M480-80q-82 0-155-31.5t-127.5-86Q143-252 111.5-325T80-480q0-83 31.5-155.5t86-127Q252-817 325-848.5T480-880q17 0 28.5 11.5T520-840q0 17-11.5 28.5T480-800q-133 0-226.5 93.5T160-480q0 133 93.5 226.5T480-160q133 0 226.5-93.5T800-480q0-17 11.5-28.5T840-520q17 0 28.5 11.5T880-480q0 82-31.5 155t-86 127.5q-54.5 54.5-127 86T480-80Z" />
                                </svg>
                                <svg v-if="promptComplete" xmlns="http://www.w3.org/2000/svg"
                                    class="fill-[#7D7D7E] inline-block" height="24" viewBox="0 -960 960 960" width="24">
                                    <path
                                        d="m424-296 282-282-56-56-226 226-114-114-56 56 170 170Zm56 216q-83 0-156-31.5T197-197q-54-54-85.5-127T80-480q0-83 31.5-156T197-763q54-54 127-85.5T480-880q83 0 156 31.5T763-763q54 54 85.5 127T880-480q0 83-31.5 156T763-197q-54 54-127 85.5T480-80Zm0-80q134 0 227-93t93-227q0-134-93-227t-227-93q-134 0-227 93t-93 227q0 134 93 227t227 93Zm0-320Z" />
                                </svg>
                                <span>Generating prompt</span>
                            </div>
                            <div class="space-x-2">
                                <svg v-if="promptComplete" xmlns="http://www.w3.org/2000/svg"
                                    class="fill-[#7D7D7E] inline-block animate-spin" height="24" viewBox="0 -960 960 960"
                                    width="24">
                                    <path
                                        d="M480-80q-82 0-155-31.5t-127.5-86Q143-252 111.5-325T80-480q0-83 31.5-155.5t86-127Q252-817 325-848.5T480-880q17 0 28.5 11.5T520-840q0 17-11.5 28.5T480-800q-133 0-226.5 93.5T160-480q0 133 93.5 226.5T480-160q133 0 226.5-93.5T800-480q0-17 11.5-28.5T840-520q17 0 28.5 11.5T880-480q0 82-31.5 155t-86 127.5q-54.5 54.5-127 86T480-80Z" />
                                </svg>
                                <svg v-else xmlns="http://www.w3.org/2000/svg" class="fill-[#7D7D7E] inline-block"
                                    height="24" viewBox="0 -960 960 960" width="24">
                                    <path
                                        d="M480-80q-83 0-156-31.5T197-197q-54-54-85.5-127T80-480q0-83 31.5-156T197-763q54-54 127-85.5T480-880q83 0 156 31.5T763-763q54 54 85.5 127T880-480q0 83-31.5 156T763-197q-54 54-127 85.5T480-80Zm0-80q134 0 227-93t93-227q0-134-93-227t-227-93q-134 0-227 93t-93 227q0 134 93 227t227 93Zm0-320Z" />
                                </svg>
                                <span>Generating image</span>
                            </div>
                        </div>
                    </div>
                    <div v-if="!resultImage && !isImageGenerationInProgress"
                        class="flex justify-center items-center h-full">
                        Result will be displayed here
                    </div>
                    <img class="md:max-w-96 md:max-h-96 max-w-80 max-h-80 rounded-xl" v-if="!!resultImage" v-bind:src="resultImage" />
                </div>
            </div>
        </div>
    </div>
    <EnterKey class="fixed bottom-5 w-full" />
</template>


<script setup lang="ts">

const imageFileBase64 = ref<string | null>()
const resultImage = ref<string | null>()
const isImageGenerationInProgress = ref<boolean>(false);

async function onFileUpload(fileUrl: string) {
    if (!apiKeyRef.value) {
        alert('Please enter api key')
        return false
    }
    try {
        isImageGenerationInProgress.value = true
        imageFileBase64.value = fileUrl
        const resultImageUrl = await getConvertedImageURLFromBase64(fileUrl)
        if (imageFileBase64.value) {
            resultImage.value = resultImageUrl
        }
    } catch (e: any) {
        console.error(e)
        console.error(e?.cause)
        alert(e?.message)
    }
    finally {
        isImageGenerationInProgress.value = false
        promptComplete.value = false
    }
}

function removeImage() {
    imageFileBase64.value = null
    resultImage.value = null
    isImageGenerationInProgress.value = false
    promptComplete.value = false
}

</script>
