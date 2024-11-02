<template>
  <section class="wordcloud shadow-box flex p-0 bg-white">
    <div
      class="w-1/3 p-5 border-r border-[#D9D9D9] flex justify-center flex-col gap-5"
      :class="{ 'items-center': !wordCloud.visible }"
    >
      <button @click="toggleWordCloud" v-if="!wordCloud.visible">
        {{ 'wordCloud.generateButtonText' }}
      </button>

      <div
        class="wordcloud-details flex flex-col h-full justify-between gap-2"
        v-if="wordCloud.visible"
      >

        <!-- <WordsEdit
          v-if="editMode"
          :words="wordCloud.words"
          :stopWords="localStopWords"
          @save="handleSaveEditMode"
        /> -->

        <!-- <Words
          v-else
          :words="
            wordCloud.words
              .filter((word) => !localStopWords.includes(word[0]))
              .map((word) => word[0])
          "
          :stopWords="localStopWords"
          @toggleEdit="toggleEditMode"
          @removeWord="removeWord"
        /> -->
      </div>
    </div>
    <div class="w-2/3">
      <div v-if="wordCloud.visible" class="h-full flex flex-col justify-between p-5">
        <div
          class="wordcloud-wrapper relative flex justify-center items-center h-full"
          id="wordcloud-wrapper"
          :style="`background-color: ${wordCloudBgColor}`"
        >
          <!-- <img
            src="@/assets/img/icons/three-dots.svg"
            alt="menu"
            class="options-menu absolute top-4 right-3 z-50 cursor-pointer"
            v-click-outside="closeMenu"
            @click.stop="toggleMenu"
          /> -->

          <!-- <TooltipMenu
            position="right"
            :open="menu.visible"
            :options="menu.options"
            @click="handleMenuClick"
            fitContent="true"
          /> -->

          <!-- <i
            v-if="closeEnabled"
            class="icon icon-close absolute -top-4 -right-4 z-50 cursor-pointer text-red-danger"
            @click="toggleWordCloud">
          </i> -->

          <VueWordCloud
            style="height: 480px; width: 95%"
            class="mx-auto"
            :words="wordCloud.words.filter((word) => !localStopWords.includes(word[0]))"
            :color="wordCloudColor"
            :font-family="wordCloud.font"
            :load-font="loadFont"
            @update:progress="progress"
            animation-delay="100"
            animation-duration="100"
          />
        </div>

        <!-- <pre>{{progress}}</pre> -->

        <!-- <div class="loading flex w-full h-full items-center justify-center" v-if="progress !== null">
               Loading...
            </div> -->

        <div class="wordcloud-toolbar mt-5">
          <div class="wordcloud-toolbar__style flex flex-wrap">
            <div class="words-color w-2/4 lg:w-1/4">
              <h4 class="mb-1">{{ 'wordCloud.wordsColor' }}</h4>
              <div class="flex gap-3">
                <div
                  class="flex h-8 w-8 p-1 border rounded cursor-pointer hover:border-[#CBCBCB]"
                  v-for="(preset, idx) in colorPresets.filter((preset) => !preset.custom)"
                  :key="idx"
                  :class="
                    wordCloud.colorPreset === preset.id ? 'border-[#CBCBCB]' : 'border-transparent'
                  "
                  @click="wordCloud.colorPreset = preset.id"
                >
                  <span
                    v-for="(item, id) in preset.colors"
                    :key="id"
                    style="flex: 1 1 0%"
                    :style="`background-color: ${item}`"
                  ></span>
                </div>
                <div
                  class="custom-color flex h-8 w-8 p-1 border rounded cursor-pointer"
                  v-title="`Custom color`"
                  :class="
                    wordCloud.colorPreset === colorPresets[colorPresets.length - 1].id &&
                    colorPresets[colorPresets.length - 1].colors.every((item) => item !== '')
                      ? 'border-[#CBCBCB]'
                      : 'border-transparent'
                  "
                >
                  <!-- <WordCloudColorPicker
                    @update:color="setCustomColor"
                    :colors="customColorOptions.map((preset) => preset.color)"
                  /> -->
                </div>
              </div>
            </div>

            <div class="words-bg w-2/4 lg:w-1/4">
              <h4 class="mb-1">{{ 'wordCloud.wordsBG' }}</h4>
              <div class="flex gap-3">
                <div
                  class="flex items-center justify-center h-8 w-8 p-1 border rounded cursor-pointer hover:border-[#CBCBCB]"
                  v-for="(preset, idx) in bgColorPresets.filter((preset) => !preset.custom)"
                  :key="idx"
                  :class="
                    wordCloud.bgColorPreset === preset.id
                      ? 'border-[#CBCBCB]'
                      : 'border-transparent'
                  "
                  @click="wordCloud.bgColorPreset = preset.id"
                >
                  <span
                    :style="`background-color: ${preset.color}`"
                    class="block h-full w-full rounded-sm hover:shadow-none"
                    :class="{ shadow: wordCloud.bgColorPreset !== preset.id }"
                  ></span>
                </div>
                <div
                  class="custom-color flex items-center justify-center h-8 w-8 p-1 border rounded cursor-pointer hover:border-[#CBCBCB]"
                  v-title="`custom color`"
                  :class="
                    wordCloud.bgColorPreset ===
                      bgColorPresets.find((preset) => preset.color == wordCloudBgColor).id &&
                    bgColorPresets.find((preset) => preset.color == wordCloudBgColor).custom
                      ? 'border-[#CBCBCB]'
                      : 'border-transparent'
                  "
                >
                  <!-- <WordCloudBgColorPicker
                    @update:color="setCustomBgColor"
                    :colors="customBgColorOptions.map((preset) => preset.color)"
                  /> -->
                </div>
              </div>
            </div>

            <div class="words-font w-full mt-3 lg:mt-0 lg:w-2/4">
              <h4 class="mb-1">{{ 'wordCloud.font' }}</h4>
              <!-- <FormKit
                type="dropdown"
                name="font"
                placeholder="Select"
                v-model="wordCloud.font"
                :options="fonts"
              /> -->
            </div>
          </div>

          <div class="wordcloud-toolbar__settings flex gap-5">
            <!-- <FormKit
              type="checkbox"
              :label="this.$t('wordCloud.remStopWords')"
              name="stopwords"
              v-model="removeStopWords"
            />
            <FormKit
              type="checkbox"
              :label="this.$t('wordCloud.splitByWords')"
              name="splitByWords"
              v-model="splitByWords"
            /> -->
          </div>
        </div>
      </div>

      <div v-else class="flex justify-center items-center p-5">
        <!-- <img src="@/assets/img/wordcloud-placeholder.svg" alt="wordcloud" /> -->
      </div>
    </div>
  </section>
</template>

<script setup>
import html2canvas from 'html2canvas'
import _ from 'lodash'
import VueWordCloud from 'vuewordcloud'
import { ref, onMounted } from 'vue'

const props = defineProps({
  data: {
    type: String,
    required: true
  },
  stopWords: {
    type: Array,
    default: () => []
  },
  closeEnabled: {
    type: Boolean,
    default: false
  }
})

const progress = ref(null)
const removeStopWords = ref(true)
const splitByWords = ref(true)
const editMode = ref(false)
const localStopWords = ref([])
const menu = ref({
  visible: false,
  options: []
})

const wordCloud = ref({
  visible: true,
  words: [],
  wordsEdited: [],
  font: 'Roboto',
  colorPreset: 1,
  bgColorPreset: 0,
})

const enStopWords = ref(["the", "and", "to", "of", "a", "in", "is", "it", "you", "that", "he", "she", "was", "for", "on", "are", "as", "with", "his", "her", "they", "I", "at", "be", "this", "have", "we", "but", "not", "can", "on", "all", "if", "your", "what", "about", "there", "out", "do", "up", "one", "go", "like", "no", "just", "so", "some", "more", "will", "would", "than", "been", "its", "who", "now", "my", "me", "could", "them", "did", "get", "into", "how", "made", "other", "two", "time", "may", "first", "than", "make", "most", "way", "each", "very", "even", "well", "where", "after", "back", "our", "over", "year", "day", "know", "any", "see", "come", "give", "take", "very", "through", "own", "too", "use", "also", "should", "could", "during"])

const fonts = ref(['Roboto','Montserrat','Kalnia','Open Sans','Poppins','Lato','Inter','Oswald','Kanit','Word Sans'])

const colorPresets = ref([
  { id: 1, colors: ['#56D08A', '#AAE7C4', '#D5F3E2'] },
  { id: 2, colors: ['#0C2461', '#079992', '#7ED6DF'] },
  { id: 3, colors: ['#B71540', '#E58E26', '#F9CA24'] },
  { id: 4, colors: [], custom: true }
])

const bgColorPresets = ref([
  { id: 1, color: '#ffffff' },
  { id: 2, color: '#F2F2F2' },
  { id: 3, color: '', custom: true }
])

const customBgColorOptions = ref([
  { id: 1, color: '#FCF7DD' },
  { id: 2, color: '#FCEAC4' },
  { id: 3, color: '#F7CBC4' },
  { id: 4, color: '#C9D2EB' },
  { id: 5, color: '#CFE3EB' },
  { id: 6, color: '#CAE9D1' },
  { id: 7, color: '#D8F3F5' },
  { id: 8, color: '#F6CCFE' },
  { id: 9, color: '#CBCCEE' },
  { id: 10, color: '#EEF3F7' },
  { id: 11, color: '#C1C2D3' },
  { id: 12, color: '#CDD1D5' }
])

const customColorOptions = ref([
  { id: 1, color: '#F6E58D' },
  { id: 2, color: '#F6B93B' },
  { id: 3, color: '#E55039' },
  { id: 4, color: '#4A69BD' },
  { id: 5, color: '#60A3BC' },
  { id: 6, color: '#78E08F' },
  { id: 7, color: '#F9CA24' },
  { id: 8, color: '#E58E26' },
  { id: 9, color: '#B71540' },
  { id: 10, color: '#0C2461' },
  { id: 11, color: '#0A3D62' },
  { id: 12, color: '#079992' },
  { id: 13, color: '#7ED6DF' },
  { id: 14, color: '#E056FD' },
  { id: 15, color: '#686DE0' },
  { id: 16, color: '#C8D6E5' },
  { id: 17, color: '#30336B' },
  { id: 18, color: '#576574' },
  { id: 19, color: '#22A6B3' },
  { id: 20, color: '#BE2EDD' },
  { id: 21, color: '#4834D4' },
  { id: 22, color: '#8395A7' },
  { id: 23, color: '#130F40' },
  { id: 24, color: '#222F3E' }
])


// COMPUTED
const wordCloudColor = computed(() => {
  const colorPreset = colorPresets.value.find(
    (preset) => preset.id === wordCloud.value.colorPreset
  )
  return ([, weight]) => {
    if (weight < 4) {
      return colorPreset.colors[2]
    } else if (weight >= 4 && weight <= 8) {
      return colorPreset.colors[1]
    } else {
      return colorPreset.colors[0]
    }
  }
})

const wordCloudBgColor = computed(() => {
  const bgColorPreset = bgColorPresets.value.find(
    (preset) => preset.id === wordCloud.value.bgColorPreset
  )
  return bgColorPreset?.color
})

const defaultStopWords = computed(() => {
  return enStopWords.value
})

// METHODS
function generateWordCloud() {
  if (wordCloud.value.wordsEdited.length > 0) {
    wordCloud.value.words = wordCloud.value.wordsEdited
    return
  }

  let split = splitByWords.value ? ' ' : ', '
  let replace = splitByWords.value ? /[.,\/#!$%^&*;:{}=`~()]/g : /[.\/#!$%^&*;:{}=`~()]/g

  let cleanedText = props.data
    .toLowerCase()
    .replace(replace, '')
    .split(split) // Clean Text
    .filter((word) => word.length > 0) // Remove Empty Words

  const wordCounts = _.countBy(cleanedText) // Count Words
  const stopWords = removeStopWords.value ? localStopWords.value : [] // Remove Stopwords
  const filteredWordCounts = _.omit(wordCounts, stopWords) // Filter Stopwords
  const sortedWordCounts = _.toPairs(filteredWordCounts).sort((a, b) => b[1] - a[1]) // Sort by Frequency

  wordCloud.value.words = sortedWordCounts
}

function toggleWordCloud() {
  wordCloud.value.visible = !wordCloud.value.visible

  if (wordCloud.value.visible) {
    emit('generate')
  }
}

function loadFont(fontFamily, fontStyle, fontWeight, text) {
  return document.fonts.load([fontStyle, fontWeight, '1px', fontFamily].join(' '), text)
}

function removeWord(event) {
  if (event || event === 0) {
    const word = event.word
    const type = event.type

    if (type === 'words') {
      wordCloud.value.words.splice(word, 1)
    } else if (type === 'stopWords') {
      localStopWords.value.splice(word, 1)
    }
  }
}

function toggleMenu() {
  menu.value.visible = !menu.value.visible
}

function closeMenu() {
  if (!menu.value.visible) return
  menu.value.visible = false
  // console.log('closeMenu')
}

function handleMenuClick(option) {
  // console.log('clicked')
  if (option && option.onClick) {
    option.onClick()
  }
}

function createMenuOptions() {
  menu.value.options = [
    {
      label: 'Download',
      visibility: true,
      onClick: () => {
        captureScreenshot()
      }
    }
  ]
}

function captureScreenshot() {
  const targetDiv = document.getElementById('wordcloud-wrapper') // Replace with the ID of your target div
  const optionsMenu = targetDiv.querySelector('.options-menu')
  optionsMenu.style.display = 'none' // hide options menu
  const tooltip = targetDiv.querySelector('.tooltip-menu')
  tooltip.style.display = 'none' // hide options menu

  html2canvas(targetDiv).then((canvas) => {
    // Convert the canvas to an image and open it in a new tab
    const dataUrl = canvas.toDataURL()
    // download image
    const a = document.createElement('a')
    a.href = dataUrl
    a.download = 'wordcloud.png'
    a.click()
  })
  // show options menu again
  optionsMenu.style.display = 'block'
  tooltip.style.display = 'block'
}

function toggleEditMode() {
  editMode.value = !editMode.value
}

function handleSaveEditMode(event) {
  wordCloud.value.wordsEdited = event.words
  localStopWords.value = event.stopWords
  toggleEditMode()
  generateWordCloud()
}

function setCustomBgColor(color) {
  const preset = bgColorPresets.value.length - 1
  bgColorPresets.value.find((preset) => preset.custom).color = color
  wordCloud.value.bgColorPreset = bgColorPresets.value[preset].id // set custom color id to words
}

function setCustomColor(color) {
  const preset = colorPresets.value.length - 1
  colorPresets.value[preset].colors = color.map((item) => item.color)
  wordCloud.value.colorPreset = colorPresets.value[preset].id // set custom color id to words
}

watch(() => props.data, () => {
  generateWordCloud()
})

watch(() => removeStopWords.value, () => {
  generateWordCloud()
})

watch(() => localStopWords.value, () => {
  generateWordCloud()
})

watch(() => props.stopWords, () => {
  localStopWords.value = props.stopWords ? props.stopWords : defaultStopWords.value
})

onMounted(() => {
  localStopWords.value = props.stopWords ? props.stopWords : defaultStopWords.value
  generateWordCloud()
  createMenuOptions()
})


</script>

<style>

</style>