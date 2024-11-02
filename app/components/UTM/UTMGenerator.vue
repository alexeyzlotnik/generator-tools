<template>
   <div class="utm-wrapper flex flex-col gap-10">
      <UCard class="password-settings w-full md:w-2/3 mx-auto">
         <template #header>
            <h2 class="text-2xl font-bold">The final URL</h2>
            <UButtonGroup size="xl" orientation="horizontal" class="w-full justify-between relative">
               <UInput size="xl" v-model="finalURL" label="Password" disabled="true" class="w-full" :ui="{base: 'h-46 min-h-46 !py-5 !ring-0 !shadow-none rounded !text-lg !pr-20 !cursor-grab'}" />
               <UTooltip text="Copy">
                  <UButton
                     variant="link"
                     icon="i-iconamoon:copy-light"
                     :padded="false"
                     class="cursor-pointer absolute top-1/2 right-3 transform -translate-y-1/2"
                     @click="copy"
                  />
               </UTooltip>
            </UButtonGroup>
         </template>
         <UForm class="w-full flex flex-col gap-5 justify-between items-start" v-model="settings">
            <div class="flex flex-col w-full gap-2" v-for="field in utm">
               <div class="flex w-full">
                  <label :for="field.id" class="bg-gray-100 p-2 rounded-l whitespace-nowrap border w-28 md:w-40 min-w-28 md:min-w-40 text-sm md:text-base flex items-center">{{ field.label }}</label>
                  <UInput
                     label="UTM Source"
                     size="xl"
                     class="w-full"
                     :ui="{base: 'rounded-r !rounded-l-none w-full'}"
                     v-model="field.value"
                     color="primary"
                     variant="outline"
                     id="utmSource"
                     :placeholder="field.placeholder"
                     :description="field.description"
                  />
               </div>
               <div class="help text-sm text-gray-500">
                  {{ field.description }}
               </div>
            </div>
         </UForm>
      </UCard>
   </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
const toast = useToast()

const finalURL = ref('')

const utm = ref([
   {
      id: 'url',
      label: 'URL',
      value: '',
      placeholder: 'https://www.example.com',
      description: 'The URL to generate UTM links for'
   },
   {
      id: 'utm_source',
      label: 'UTM Source',
      value: '',
      placeholder: 'utm_source',
      description: 'The source of the UTM link'
   },
   {
      id: 'utm_medium',
      label: 'UTM Medium',
      value: '',
      placeholder: 'utm_medium',
      description: 'The medium of the UTM link'
   },
   {
      id: 'utm_campaign',
      label: 'UTM Campaign',
      value: '',
      placeholder: 'utm_campaign',
      description: 'The campaign of the UTM link'
   },
   {
      id: 'utm_term',
      label: 'UTM Term',
      value: '',
      placeholder: 'utm_term',
      description: 'The term of the UTM link'
   }
])

function copy() {
   navigator.clipboard.writeText(finalURL.value)
   toast.add({title: 'Copied to clipboard', description: 'The UTM link has been copied to your clipboard'})
}

function generateUTM() {
   const baseUrl = utm.value[0].value !== '' ? utm.value[0].value : 'https://www.example.com'
   const params = utm.value
      .slice(1) // Skip the base URL field
      .filter(field => field.value) // Only include fields with values
      .map(field => `${field.id}=${field.value}`)
      .join('&')

   finalURL.value = params ? `${baseUrl}?${params}` : baseUrl
}

watch(utm, () => {
   generateUTM()
}, { deep: true })

onMounted(() => {
   generateUTM()
})
</script>
