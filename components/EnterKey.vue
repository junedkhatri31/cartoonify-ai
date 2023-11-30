<template>
    <div class="h-36 flex items-center justify-center">
        <div class="bg-[#363636] border-black border-2 flex px-4 py-2 w-80 rounded-xl">
            <div class="flex-1 flex items-center justify-center truncate w-80">
                <button class="text-[#F0F0F0] text-sm w-full" v-if="!isTextbox" @click="onApiKeyClick">
                    <span v-if="hiddenAPIKey" class="text-[#F0F0F0]">{{ hiddenAPIKey }}</span>
                    <span v-else>Your OpenAI API Key</span>
                </button>
                <input v-if="isTextbox" class="w-full h-full bg-transparent text-[#F0F0F0]" ref="apiKeyInput"
                    @blur="setAPIKey" />
            </div>
            <button @click="showHelp">
                <svg class="inline-block fill-[#7D7D7E]" xmlns="http://www.w3.org/2000/svg" height="24"
                    viewBox="0 -960 960 960" width="24">
                    <path
                        d="M478-240q21 0 35.5-14.5T528-290q0-21-14.5-35.5T478-340q-21 0-35.5 14.5T428-290q0 21 14.5 35.5T478-240Zm-36-154h74q0-33 7.5-52t42.5-52q26-26 41-49.5t15-56.5q0-56-41-86t-97-30q-57 0-92.5 30T342-618l66 26q5-18 22.5-39t53.5-21q32 0 48 17.5t16 38.5q0 20-12 37.5T506-526q-44 39-54 59t-10 73Zm38 314q-83 0-156-31.5T197-197q-54-54-85.5-127T80-480q0-83 31.5-156T197-763q54-54 127-85.5T480-880q83 0 156 31.5T763-763q54 54 85.5 127T880-480q0 83-31.5 156T763-197q-54 54-127 85.5T480-80Zm0-80q134 0 227-93t93-227q0-134-93-227t-227-93q-134 0-227 93t-93 227q0 134 93 227t227 93Zm0-320Z" />
                </svg>
            </button>
        </div>
    </div>
</template>

<script setup lang="ts">

const isTextbox = ref<boolean>(false)
const apiKeyInput = ref<HTMLInputElement | null>(null)
const hiddenAPIKey = ref<string | null>(null)

function hideApiKey(apiKey: string) {
    if (apiKey.length > 3) {
        const prefix = apiKey.substring(0, 3)
        const postfix = apiKey.substring(3)
        const postfixHidden = '*'.repeat(postfix.length)
        return prefix + postfixHidden
    }
    return apiKey
}

watchEffect(() => {
    hiddenAPIKey.value = apiKeyRef.value && hideApiKey(apiKeyRef.value)
})

function showHelp() {
    const help = "Sorry, this is weird. If you have an OpenAI developer key, you can put it in this input and we'll use it. We don't save / store / upload these.\n\nSee https://platform.openai.com/api-keys to get a key.\n\nThis app's source code: https://github.com/junedkhatri31/cartoonify-ai"
    alert(help)
}

function onApiKeyClick() {
    isTextbox.value = true
    setTimeout(() => {
        apiKeyInput?.value?.focus()
        if (apiKeyInput?.value && apiKeyRef.value) {
            apiKeyInput.value.value = apiKeyRef.value
        }
    })
}

function setAPIKey() {
    apiKeyRef.value = apiKeyInput?.value?.value || null
    isTextbox.value = false
}

</script>
