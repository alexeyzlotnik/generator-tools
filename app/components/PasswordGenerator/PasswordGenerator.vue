<template>
   <div class="password-wrapper flex flex-col gap-10">
      <UCard class="password-settings w-full md:w-2/3 mx-auto">
         <template #header>
            <UButtonGroup size="xl" orientation="horizontal" class="w-full justify-between relative">
            <UInput size="xl" v-model="password" label="Password" class="w-full" :ui="{base: 'h-46 min-h-46 !py-5 !ring-0 !shadow-none rounded !text-3xl !pr-20'}" />
            <UTooltip text="Generate">
               <UButton
                  variant="link"
                  icon="i-heroicons:arrow-path-solid"
                  :padded="false"
                  class="cursor-pointer absolute top-1/2 right-3 transform -translate-y-1/2"
                  @click="generatePassword"
               />
            </UTooltip>
            <UTooltip text="Copy">
               <UButton
                  variant="link"
                  icon="i-iconamoon:copy-light"
                  :padded="false"
                  class="cursor-pointer absolute top-1/2 right-12 transform -translate-y-1/2"
                  @click="copyPassword"
               />
            </UTooltip>
            <UProgress :value="passwordComplexity" class="absolute bottom-0 left-0 w-full" />
         </UButtonGroup>
         </template>
         <UForm class="w-full flex flex-col sm:flex-row gap-3 justify-between items-start" v-model="settings">
            <!-- password length. Slider -->
            <div class="w-full sm:w-1/2 flex gap-3 items-center">
               <UInput
                  size="xl"
                  :ui="{
                     ring: 'ring-1 ring-gray-200 dark:ring-gray-800',
                  }"
                  v-model="settings.passwordLength"
                  type="number"
                  @input="(event) => { event.target.value > 50 ? event.target.value = 50 : event.target.value < 1 ? event.target.value = 1 : event.target.value }"
               />
               <div class="w-full flex flex-col gap-1">
                  <p>Password Length</p>
                  <URange size="md" v-model="settings.passwordLength" min="1" max="50" />
               </div>
            </div>

            <div class="w-full sm:w-1/2 flex gap-3 justify-between">
               <!-- password rules. Uppercase, lowercase, numbers, symbols -->
               <div class="password-rules">
                  <div class="password-rules-list flex flex-col sm:flex-row flex-wrap gap-3">
                     <UCheckbox
                        size="xl"
                        v-for="(rule, index) in settings.rules"
                        :label="rule.label"
                        v-model="rule.state"
                        :key="index"
                     />
                  </div>
               </div>
            </div>
         </UForm>
         <div class="flex justify-center mt-10">
            <UButton
               @click="copyPassword"
               size="xl"
            >
               Copy Password
            </UButton>
         </div>
      </UCard>
      <!-- 3 generated password examples -->
      <div class="generated-passwords flex flex-col gap-3 w-full md:w-2/3 mx-auto">
         <h2 class="text-2xl font-bold text-center">Password Examples</h2>
         <div class="generated-password flex gap-3 items-center justify-center bg-gray-100 dark:bg-gray-800 rounded-lg p-3" v-for="password in passwordExamples" :key="password">
            <p>{{ password }}</p>
            <!-- copy button -->
            <UTooltip text="Copy">
               <UButton
                  variant="link"
                  icon="i-iconamoon:copy-light"
                  :padded="false"
                  @click="copyPassword"
               />
            </UTooltip>
         </div>
      </div>
   </div>
</template>

<script setup>
import { ref } from 'vue'
const toast = useToast()

const password = ref('')
const passwordExamples = ref([])

const settings = ref({
   passwordLength: 16,
   selectedComplexity: 'medium',
   rules: [
      { label: 'Uppercase', state: true },
      { label: 'Lowercase', state: true },
      { label: 'Numbers', state: true },
      { label: 'Symbols', state: true },
   ]
})

// computed property to calculate password complexity (0 - 100)
const passwordComplexity = computed(() => {
   const enabledRules = settings.value.rules.filter(rule => rule.state)
   const totalRules = settings.value.rules.length
   const enabledRulesCount = enabledRules.length
   const minLength = 16

   // calculate the complexity based on the number of enabled rules and password length
   const complexity = Math.floor((enabledRulesCount / totalRules) * 100) * (settings.value.passwordLength / minLength)

   return complexity
})

function createPassword() {
   password.value = generatePassword()
}

function createPasswordExamples() {
   passwordExamples.value = []

   for (let i = 0; i < 3; i++) {
      let generatedPassword = generatePassword()
      passwordExamples.value.push(generatedPassword)
   }
}

function generatePassword() {
  const length = settings.value.passwordLength;
  const enabledRules = settings.value.rules.filter(rule => rule.state);

  // Character sets for each rule
  const charSets = {
    Uppercase: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
    Lowercase: 'abcdefghijklmnopqrstuvwxyz',
    Numbers: '0123456789',
    Symbols: '!@#$%^&*()_+~`|}{[]:;?><,./-=',
  };

  // Build the pool of characters based on active rules
  let pool = '';
  enabledRules.forEach(rule => {
    pool += charSets[rule.label] || '';
  });

  // Generate password from the pool of characters
  let generatedPassword = '';
  for (let i = 0; i < length; i++) {
    const randomIndex = Math.floor(Math.random() * pool.length);
    generatedPassword += pool[randomIndex];
  }

  // Set the generated password
  return generatedPassword;
}

function copyPassword() {
   navigator.clipboard.writeText(password.value);
   toast.add({ title: 'Password copied to clipboard', type: 'success', icon: 'i-heroicons:clipboard-document-check' })
}

watch(settings.passwordLength, () => {
   // set min: 1 and max: 50
   if (settings.value.passwordLength < 1) {
      settings.value.passwordLength = 1
   } else if (settings.value.passwordLength > 50) {
      settings.value.passwordLength = 50
   }
})

watch(settings, () => {
   createPassword()
   createPasswordExamples()
}, { deep: true })

onMounted(() => {
   createPassword()
   createPasswordExamples()
})
</script>