<template>
  <div class="utm-wrapper grid grid-cols-1 md:grid-cols-2 gap-10">
     <UCard class="password-settings w-full">
        <UForm class="col-span-2 md:col-span-1 flex flex-col justify-between items-start space-y-6" v-model="settings">
           <div class="flex flex-col w-full gap-2" v-for="step in calculations" :key="step.step">
              <div class="flex flex-col w-full space-y-2">
                <h3 class="text-xl step-title">{{ step.name }}</h3>
                <div v-for="input in step.inputs" :key="input.id" class="field flex items-center gap-1">
                    <label :for="input.id" class="w-fit text-nowrap">{{ input.label }}</label>
                    <UInput
                      v-if="input.type === 'number'"
                      type="number"
                      class="w-20"
                      :ui="{
                          base: 'rounded'
                      }"
                      color="primary"
                      variant="outline"
                      :id="input.id"
                      v-model="formValues[input.id]"
                    />
                    <USelectMenu
                      v-if="input.type === 'select'"
                      :options="input.options"
                      v-model="formValues[input.id]"
                      class="w-full"
                    />
                    <span v-if="input.afterText">{{ input.afterText }}</span>
                </div>
              </div>
              <div v-if="step.description" class="help text-sm text-gray-500">
                 {{ step.description }}
              </div>
           </div>
        </UForm>
     </UCard>
     <UCard >
       <div class="col-span-2 md:col-span-1 space-y-8">
          <div class="space-y-4">
            <h3 class="text-xl border-b pb-2 mb-2">Коэффициент запаса прочности днища</h3>
            <div class="space-y-1">
              <p>Толщина стенки:</p>
              <p>k = <span class="highlighted-value">2.11</span></p>
            </div>
          </div>

          <div class="space-y-4">
            <h3 class="text-xl border-b pb-2 mb-2">Результаты расчета днища</h3>
            <div class="space-y-1">
              <p>Суммарная прибавка к толщине стенки днища:</p>
              <p>c = c1 + c2 + c3 = {{ formValues['с1'] }} + {{ formValues['с2'] }} + {{ formValues['с3'] }} = <span class="highlighted-value">{{ c }} мм.</span></p>
            </div>
            <div class="space-y-1">
              <p>Радиус кривизны в вершине днища:</p>
              <p>R = D2 / ( 4 * H ) = {{ formValues['d'] }}^2 / ( 4 * {{ formValues['h'] }} ) = <span class="highlighted-value">{{ R }} мм.</span></p>
            </div>
            <div class="space-y-1">
              <p>Расчетная толщина стенки обечайки:</p>
              <p>sр = p * R / (2 * φ * [σ] - 0.5 * p) = {{ formValues['p'] }} * {{ R }} / (2 * {{ formValues['ф'] }} * {{ formValues['σ'] }} - 0.5 * {{ formValues['p'] }}) = <br><span class="highlighted-value">{{ sp }} мм.</span>
              </p>
            </div>
            <div class="space-y-1">
              <p>Расчетная толщина обечайки с учетом прибавок:</p>
              <p>sр + c = {{ sp }} + {{ c }} = <span class="highlighted-value">{{ spPlusC }} мм</span>, прочность обеспечена.</p>
            </div>
            <div class="space-y-1">
              <p>Допускаемое внутреннее избыточное давление:</p>
              <p>[p] = 2*(s - c) * φ * [σ] / (R + 0.5*(s - c) ) = 2*( {{ formValues['s'] }} - {{ c }} ) * {{ formValues['ф'] }} * {{ formValues['σ'] }} / ({{ R }} + 0.5*( {{ formValues['s'] }} - {{ c }} ) ) = <span class="highlighted-value">{{ allowableP }} МПа</span>, прочность обеспечена.
              </p>
            </div>
          </div>
        </div>
     </UCard>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const calculations = ref([
  {
    step: 1,
    name: 'Шаг 1: для начала расчета задайте давление',
    inputs: [
      {
        id: 'p',
        label: 'Расчетное давление р =',
        afterText: 'МПа',
        type: 'number',
        default_value: 1,
      },
      {
        id: 't',
        label: 'Расчетная температура Т =',
        afterText: 'ºС',
        type: 'number',
        default_value: 20,
      }
    ]
  },
  {
    step: 2,
    name: 'Шаг 2: задайте диаметр и толщину днища',
    inputs: [
      {
        id: 'd',
        label: 'Внутренний диаметр днища D =',
        afterText: 'мм',
        type: 'number',
        default_value: 1000,
      },
      {
        id: 's',
        label: 'Толщина стенки днища s =',
        afterText: 'мм',
        type: 'number',
        default_value: 10,
      }
    ]
  },
  {
    step: 3,
    name: 'Шаг 3: задайте высоту днища',
    inputs: [
      {
        id: 'headType',
        label: 'Тип днища',
        type: 'select',
        default_value: 'Эллиптическое днище',
        options: [
          { id: 'elliptical', label: 'Эллиптическое днище' },
          { id: 'hemispherical', label: 'Полусферическое днище' }
        ]
      },
      {
        id: 'h',
        label: 'Высота днища H =',
        afterText: 'мм',
        type: 'number',
        default_value: 250,
      }
    ]
  },
  {
    step: 4,
    name: 'Шаг 4: выберите материал днища',
    inputs: [
      {
        id: 'steelGrade',
        label: 'Марка стали днища',
        type: 'select',
        options: [
          { label: 'Option 1' },
          { label: 'Option 2' },
          { label: 'Option 3' },
          { label: 'Option 4' }
        ]
      },
      {
        id: 'σ',
        label: 'Допускаемое напряжение [σ] =',
        afterText: 'МПа',
        type: 'number',
        default_value: 154,
      }
    ]
  },
  {
    step: 5,
    name: 'Шаг 5: уточните прибавки к толщине стенки',
    description: 'Как правило, технологическая прибавка c3 для эллиптических днищ 15% от номинальной толщины стенки днища, для полусферических - 10 %.',
    inputs: [
      {
        id: 'с1',
        label: 'Прибавка на коррозию c1 =',
        afterText: 'мм',
        type: 'number',
        default_value: 0,
      },
      {
        id: 'с2',
        label: 'Компенсация минусового допуска c2 =',
        afterText: 'мм',
        type: 'number',
        default_value: 0,
      },
      {
        id: 'с3',
        label: 'Технологическая прибавка c3 =',
        afterText: 'мм',
        type: 'number',
        default_value: 1.5,
      }
    ]
  },
  {
    step: 6,
    name: 'Шаг 6: уточните коэффициент сварного соединения',
    inputs: [
      {
        id: 'ф',
        label: 'Коэффициент прочности сварного шва ф =',
        type: 'number',
        default_value: 1,
      },
    ]
  },
])

// 1. Initialize formValues with default values
const formValues = ref({})

onMounted(() => {
  calculations.value.forEach(step => {
    step.inputs.forEach(input => {
      formValues.value[input.id] = input.default_value ?? ''
    })
  })
})

// 2. Computed properties for formulas

// c = c1 + c2 + c3
const c = computed(() => {
  const c1 = Number(formValues.value['с1'] || 0)
  const c2 = Number(formValues.value['с2'] || 0)
  const c3 = Number(formValues.value['с3'] || 0)
  return +(c1 + c2 + c3).toFixed(2)
})

// R = D^2 / (4 * H)
const R = computed(() => {
  const D = Number(formValues.value['d'] || 0)
  const H = Number(formValues.value['h'] || 1)
  return +(Math.pow(D, 2) / (4 * H)).toFixed(2)
})

// sp = p * R / (2 * φ * [σ] - 0.5 * p)
const sp = computed(() => {
  const p = Number(formValues.value['p'] || 0)
  const Rv = R.value
  const phi = Number(formValues.value['ф'] || 1)
  const sigma = Number(formValues.value['σ'] || 1)
  return +(p * Rv / (2 * phi * sigma - 0.5 * p)).toFixed(2)
})

// sp + c
const spPlusC = computed(() => {
  return +(sp.value + c.value).toFixed(2)
})

// [p] = 2*(s - c) * φ * [σ] / (R + 0.5*(s - c))
const allowableP = computed(() => {
  const s = Number(formValues.value['s'] || 0)
  const phi = Number(formValues.value['ф'] || 1)
  const sigma = Number(formValues.value['σ'] || 1)
  const Rv = R.value
  const cVal = c.value
  return +(2 * (s - cVal) * phi * sigma / (Rv + 0.5 * (s - cVal))).toFixed(2)
})

// Watch for changes in headType and update defaults accordingly
watch(
  () => formValues.value['headType'],
  (newVal) => {
    if (!newVal) return
    // If using object as value, compare by id
    const id = typeof newVal === 'object' ? newVal.id : newVal
    if (id === 'elliptical') {
      formValues.value['h'] = 250
      formValues.value['с3'] = 1.5
    } else if (id === 'hemispherical') {
      formValues.value['h'] = 450
      formValues.value['с3'] = 1
    }
  },
  { immediate: true }
)

function generateUTM() {

}


</script>

<style>
.highlighted-value{
  background: rgb(54, 131, 255);
  color: #fff;
  padding: 2px 6px;
  border-radius: 8px;
}

.step-title{
  color: rgb(54, 131, 255);
}

.text-nowrap {
  text-wrap: nowrap;
}
</style>