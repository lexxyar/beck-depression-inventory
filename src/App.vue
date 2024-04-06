<script setup>
import {computed, ref} from "vue";
import domtoimage from 'dom-to-image';

const val1 = ref(9)
const val2 = ref(25)
const val3 = ref(12)
const maxVal1 = 63
const maxVal2 = 39
const maxVal3 = 24
const chart1MaxWidth = 797
const chart2MaxWidth = 419
const chart3MaxWidth = 419

const value1 = computed({
  get: () => val1.value,
  set: value => {
    if (value < 0) val1.value = 0
    else if (value > maxVal1) val1.value = maxVal1
    else val1.value = value
  }
})
const value2 = computed({
  get: () => val2.value,
  set: value => {
    if (value < 0) val2.value = 0
    else if (value > maxVal2) val2.value = maxVal2
    else val2.value = value
  }
})
const value3 = computed({
  get: () => val3.value,
  set: value => {
    if (value < 0) val3.value = 0
    else if (value > maxVal3) val3.value = maxVal3
    else val3.value = value
  }
})
const chart1Width = computed(() => Math.round(chart1MaxWidth * value1.value / maxVal1))
const chart2Width = computed(() => Math.round(chart2MaxWidth * value2.value / maxVal2))
const chart3Width = computed(() => Math.round(chart3MaxWidth * value3.value / maxVal3))

const resultText = computed(() => {
  if (value1.value <= 9) return `отсутствие депрессивных симптомов`
  if (value1.value > 9 && value1.value <= 18) return `субдепрессия, умеренная депрессия`
  if (value1.value > 18 && value1.value <= 29) return `выраженная депрессия средней тяжести`
  return `тяжелая депрессия`
})
const resultColor = computed(() => {
  if (value1.value <= 9) return `green`
  if (value1.value > 9 && value1.value <= 18) return `olive`
  if (value1.value > 18 && value1.value <= 29) return `darkred`
  return `crimson`
})

const download = (dataUrl, ext) => {
  const link = document.createElement('a');
  link.download = `bek-chart.${ext}`;
  link.href = dataUrl;
  link.click();
}
const onPngClick = () => {
  const node = document.getElementById('sclrx')
  domtoimage.toPng(node, {})
      .then(blob => download(blob, 'png'))
}

const onJpegClick = () => {
  const node = document.getElementById('sclrx')
  domtoimage.toJpeg(node, {})
      .then(blob => download(blob, 'jpeg'))
}

const onSvgClick = () => {
  const node = document.getElementById('sclrx')
  domtoimage.toSvg(node, {})
      .then(blob => download(blob, 'svg'))
}

</script>

<template>
  <div class="w-screen min-h-screen flex flex-col">
    <div class="flex flex-wrap gap-4 items-center px-2 pt-2 pb-4 shadow-lg mb-16">
      <div class="flex items-center gap-2">
        <div class="">Шкала депрессии</div>
        <input class="border rounded border-gray-300 px-2 py-1.5 w-14" type="number" min="0" max="63" v-model="value1">
      </div>
      <div class="flex items-center gap-2  lg:border-l lg:border-l-gray-200 lg:pl-2">
        <div class="">Когнитивно-аффективные проявления</div>
        <input class="border rounded border-gray-300 px-2 py-1.5 w-14" type="number" min="0" max="39" v-model="value2">
      </div>
      <div class="flex items-center gap-2  lg:border-l lg:border-l-gray-200 lg:pl-2">
        <div class="">Соматические проявления</div>
        <input class="border rounded border-gray-300 px-2 py-1.5 w-14" type="number" min="0" max="24" v-model="value3">
      </div>
      <div>
        <button type="button"
                class="text-white bg-[#4285F4] hover:bg-[#4285F4]/90 focus:ring-4 focus:outline-none focus:ring-[#4285F4]/50 font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center dark:focus:ring-[#4285F4]/55 me-2 mb-2"
                @click="onPngClick"
        >
          <svg class="w-4 h-4 me-2" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
               stroke-width="1.5"
               stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round"
                  d="M3 16.5v2.25A2.25 2.25 0 0 0 5.25 21h13.5A2.25 2.25 0 0 0 21 18.75V16.5M16.5 12 12 16.5m0 0L7.5 12m4.5 4.5V3"/>
          </svg>
          PNG
        </button>
        <button type="button"
                class="text-white bg-[#4285F4] hover:bg-[#4285F4]/90 focus:ring-4 focus:outline-none focus:ring-[#4285F4]/50 font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center dark:focus:ring-[#4285F4]/55 me-2 mb-2"
                @click="onJpegClick"
        >
          <svg class="w-4 h-4 me-2" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
               stroke-width="1.5"
               stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round"
                  d="M3 16.5v2.25A2.25 2.25 0 0 0 5.25 21h13.5A2.25 2.25 0 0 0 21 18.75V16.5M16.5 12 12 16.5m0 0L7.5 12m4.5 4.5V3"/>
          </svg>
          JPEG
        </button>
        <button type="button"
                class="text-white bg-[#4285F4] hover:bg-[#4285F4]/90 focus:ring-4 focus:outline-none focus:ring-[#4285F4]/50 font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center dark:focus:ring-[#4285F4]/55 me-2 mb-2"
                @click="onSvgClick"
        >
          <svg class="w-4 h-4 me-2" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
               stroke-width="1.5"
               stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round"
                  d="M3 16.5v2.25A2.25 2.25 0 0 0 5.25 21h13.5A2.25 2.25 0 0 0 21 18.75V16.5M16.5 12 12 16.5m0 0L7.5 12m4.5 4.5V3"/>
          </svg>
          SVG
        </button>

      </div>
    </div>

    <div class="resScale flex justify-center grow">
      <div id="sclrx">
        <div class="nisBlock">
          <table class="nisTable" border="0">
            <tbody>
            <tr>
              <td colspan="3">
                <div class="nisTitle">
                  <div>Шкала депрессии Бека</div>
                </div>
              </td>
            </tr>
            <tr>
              <td class="nisScale" :width="chart1MaxWidth">
                <div :style="`max-width: ${chart1MaxWidth}px;`">
                  <svg width="100%" height="25" :viewBox="`0 0 ${chart1MaxWidth} 25`" preserveAspectRatio="none">
                    <defs>
                      <pattern id="pg1" x="0" y="0" width="21" height="25" patternUnits="userSpaceOnUse">
                        <rect x="0" y="0" width="20" height="25" stroke-width="0" fill="whitesmoke"></rect>
                      </pattern>
                      <pattern id="pa1" x="0" y="0" width="21" height="25" patternUnits="userSpaceOnUse">
                        <rect x="0" y="0" width="20" height="25" stroke-width="0" :fill="resultColor"></rect>
                      </pattern>
                    </defs>
                    <rect x="0" y="0" :width="chart1MaxWidth" height="25" stroke-width="0" fill="url(#pg1)"></rect>
                    <rect x="0" y="0" :width="chart1Width" height="25" stroke-width="0" fill="url(#pa1)"></rect>
                  </svg>
                  <svg width="100%" height="15" :viewBox="`0 0 ${chart1MaxWidth} 15`">
                    <text x="113" y="1" class="tex1" text-anchor="end" fill="green">9]</text>
                    <text x="113" y="1" class="tex1" fill="olive">[10</text>
                    <text x="227" y="1" class="tex1" text-anchor="end" fill="olive">18]</text>
                    <text x="227" y="1" class="tex1" fill="darkred">[19</text>
                    <text x="366" y="1" class="tex1" text-anchor="end" fill="darkred">29]</text>
                    <text x="366" y="1" class="tex1" fill="crimson">[30</text>
                    <text :x="chart1MaxWidth" y="1" class="tex1" text-anchor="end" fill="crimson">63]</text>
                    <text x="1" y="1" class="tex1" fill="green">[0</text>
                  </svg>
                </div>
              </td>
              <td class="nisVal">
                <div>{{ value1 }}</div>
              </td>
            </tr>
            <tr>
              <td colspan="2">
                <div class="nisResult text-center" :style="`color: ${resultColor};`">[&nbsp;{{
                    resultText
                  }}&nbsp;]
                </div>
              </td>
            </tr>
            </tbody>
          </table>
        </div>
        <div class="nisBlock">
          <table class="nisTable" border="0">
            <tbody>
            <tr>
              <td class="nisName">
                <div>Когнитивно-аффективные проявления</div>
              </td>
              <td class="nisScale" :width="chart2MaxWidth">
                <div :style="`max-width: ${chart2MaxWidth}px;`">
                  <svg width="100%" height="25" :viewBox="`0 0 ${chart2MaxWidth} 25`" preserveAspectRatio="none">
                    <defs>
                      <pattern id="pg2" x="0" y="0" width="21" height="25" patternUnits="userSpaceOnUse">
                        <rect x="0" y="0" width="20" height="25" stroke-width="0" fill="whitesmoke"></rect>
                      </pattern>
                      <pattern id="pa2" x="0" y="0" width="21" height="25" patternUnits="userSpaceOnUse">
                        <rect x="0" y="0" width="20" height="25" stroke-width="0" :fill="resultColor"></rect>
                      </pattern>
                    </defs>
                    <rect x="0" y="0" :width="chart2MaxWidth" height="25" stroke-width="0" fill="url(#pg2)"></rect>
                    <rect x="0" y="0" :width="chart2Width" height="25" stroke-width="0" fill="url(#pa2)"></rect>
                  </svg>
                  <svg width="100%" height="15" :viewBox="`0 0 ${chart2MaxWidth} 15`">

                    <text :x="chart2MaxWidth" y="1" class="tex2" text-anchor="end" :fill="resultColor">39]</text>
                    <text x="1" y="1" class="tex2" :fill="resultColor">[0</text>
                  </svg>
                </div>
              </td>
              <td class="nisVal">
                <div>{{ value2 }}</div>
              </td>
            </tr>
            <tr>
              <td class="nisName">
                <div>Соматические проявления</div>
              </td>
              <td class="nisScale" :width="chart3MaxWidth">
                <div :style="`max-width: ${chart3MaxWidth}px;`">
                  <svg width="100%" height="25" :viewBox="`0 0 ${chart3MaxWidth} 25`" preserveAspectRatio="none">
                    <defs>
                      <pattern id="pg3" x="0" y="0" width="21" height="25" patternUnits="userSpaceOnUse">
                        <rect x="0" y="0" width="20" height="25" stroke-width="0" fill="whitesmoke"></rect>
                      </pattern>
                      <pattern id="pa3" x="0" y="0" width="21" height="25" patternUnits="userSpaceOnUse">
                        <rect x="0" y="0" width="20" height="25" stroke-width="0" :fill="resultColor"></rect>
                      </pattern>
                    </defs>
                    <rect x="0" y="0" :width="chart3MaxWidth" height="25" stroke-width="0" fill="url(#pg3)"></rect>
                    <rect x="0" y="0" :width="chart3Width" height="25" stroke-width="0" fill="url(#pa3)"></rect>
                  </svg>
                  <svg width="100%" height="15" :viewBox="`0 0 ${chart3MaxWidth} 15`">

                    <text :x="chart3MaxWidth" y="1" class="tex3" text-anchor="end" :fill="resultColor">24]</text>
                    <text x="1" y="1" class="tex3" :fill="resultColor">[0</text>
                  </svg>
                </div>
              </td>
              <td class="nisVal">
                <div>{{ value3 }}</div>
              </td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <footer class="border-t border-t-gray-200 text-gray-700 py-4 px-2 flex flex-wrap w-full justify-center relative">
      <div>
        Created by &nbsp;<a href="http://lexxsoft.ru" class="font-semibold hover:underline cursor-pointer">Aleksey
        Komov</a>&nbsp; in 2024
      </div>
      <div class="flex items-center justify-end gap-2 ms-2 w-28    sm:absolute sm:right-2 sm:top-1/2 sm:-translate-y-1/2">
        <a href="https://github.com/lexxyar"
           class="cursor-pointer"
           title="Профиль на GitHub"
           target="_blank">
          <svg xmlns="http://www.w3.org/2000/svg"
               class="w-5 h-5"
               viewBox="0 0 496 512">
            <!--!Font Awesome Free 6.5.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free Copyright 2024 Fonticons, Inc.-->
            <path
                d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3 .3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5 .3-6.2 2.3zm44.2-1.7c-2.9 .7-4.9 2.6-4.6 4.9 .3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3 .7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3 .3 2.9 2.3 3.9 1.6 1 3.6 .7 4.3-.7 .7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3 .7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3 .7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z"/>
          </svg>
        </a>

        <a href="https://vk.com/lexxyar"
           class="cursor-pointer"
           title="Профиль VK"
           target="_blank">
          <svg xmlns="http://www.w3.org/2000/svg"
               class="w-5 h-5"
               viewBox="0 0 448 512">
            <!--!Font Awesome Free 6.5.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free Copyright 2024 Fonticons, Inc.-->
            <path
                d="M31.5 63.5C0 95 0 145.7 0 247V265C0 366.3 0 417 31.5 448.5C63 480 113.7 480 215 480H233C334.3 480 385 480 416.5 448.5C448 417 448 366.3 448 265V247C448 145.7 448 95 416.5 63.5C385 32 334.3 32 233 32H215C113.7 32 63 32 31.5 63.5zM75.6 168.3H126.7C128.4 253.8 166.1 290 196 297.4V168.3H244.2V242C273.7 238.8 304.6 205.2 315.1 168.3H363.3C359.3 187.4 351.5 205.6 340.2 221.6C328.9 237.6 314.5 251.1 297.7 261.2C316.4 270.5 332.9 283.6 346.1 299.8C359.4 315.9 369 334.6 374.5 354.7H321.4C316.6 337.3 306.6 321.6 292.9 309.8C279.1 297.9 262.2 290.4 244.2 288.1V354.7H238.4C136.3 354.7 78 284.7 75.6 168.3z"/>
          </svg>
        </a>
        <a href="mailto:lexxyar@ya.ru"
           class="cursor-pointer"
           title="Напишите мне письмо"
           target="_blank">
          <svg xmlns="http://www.w3.org/2000/svg"
               class="w-5 h-5"
               viewBox="0 0 512 512">
            <!--!Font Awesome Free 6.5.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free Copyright 2024 Fonticons, Inc.-->
            <path
                d="M48 64C21.5 64 0 85.5 0 112c0 15.1 7.1 29.3 19.2 38.4L236.8 313.6c11.4 8.5 27 8.5 38.4 0L492.8 150.4c12.1-9.1 19.2-23.3 19.2-38.4c0-26.5-21.5-48-48-48H48zM0 176V384c0 35.3 28.7 64 64 64H448c35.3 0 64-28.7 64-64V176L294.4 339.2c-22.8 17.1-54 17.1-76.8 0L0 176z"/>
          </svg>
        </a>
        <a href="http://lexxsoft.ru"
           class="cursor-pointer"
           title="У меня есть сайт"
           target="_blank">
          <svg xmlns="http://www.w3.org/2000/svg"
               class="w-5 h-5"
               viewBox="0 0 512 512">
            <!--!Font Awesome Free 6.5.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free Copyright 2024 Fonticons, Inc.-->
            <path
                d="M352 256c0 22.2-1.2 43.6-3.3 64H163.3c-2.2-20.4-3.3-41.8-3.3-64s1.2-43.6 3.3-64H348.7c2.2 20.4 3.3 41.8 3.3 64zm28.8-64H503.9c5.3 20.5 8.1 41.9 8.1 64s-2.8 43.5-8.1 64H380.8c2.1-20.6 3.2-42 3.2-64s-1.1-43.4-3.2-64zm112.6-32H376.7c-10-63.9-29.8-117.4-55.3-151.6c78.3 20.7 142 77.5 171.9 151.6zm-149.1 0H167.7c6.1-36.4 15.5-68.6 27-94.7c10.5-23.6 22.2-40.7 33.5-51.5C239.4 3.2 248.7 0 256 0s16.6 3.2 27.8 13.8c11.3 10.8 23 27.9 33.5 51.5c11.6 26 20.9 58.2 27 94.7zm-209 0H18.6C48.6 85.9 112.2 29.1 190.6 8.4C165.1 42.6 145.3 96.1 135.3 160zM8.1 192H131.2c-2.1 20.6-3.2 42-3.2 64s1.1 43.4 3.2 64H8.1C2.8 299.5 0 278.1 0 256s2.8-43.5 8.1-64zM194.7 446.6c-11.6-26-20.9-58.2-27-94.6H344.3c-6.1 36.4-15.5 68.6-27 94.6c-10.5 23.6-22.2 40.7-33.5 51.5C272.6 508.8 263.3 512 256 512s-16.6-3.2-27.8-13.8c-11.3-10.8-23-27.9-33.5-51.5zM135.3 352c10 63.9 29.8 117.4 55.3 151.6C112.2 482.9 48.6 426.1 18.6 352H135.3zm358.1 0c-30 74.1-93.6 130.9-171.9 151.6c25.5-34.2 45.2-87.7 55.3-151.6H493.4z"/>
          </svg>
        </a>
      </div>
    </footer>
  </div>
</template>

<style scoped lang="scss">
.w-330px {
  width: 330px;
}


.tex1 {
  font-size: 13px;
  dominant-baseline: hanging;
}

@media (max-width: 630px) {
  .tex1 {
    font-size: 14px;
  }
}

@media (max-width: 540px) {
  .tex1 {
    font-size: 16px;
  }
}

@media (max-width: 480px) {
  .tex1 {
    font-size: 18px;
  }
}

@media (max-width: 360px) {
  .tex1 {
    font-size: 23px;
  }
}

.tex2 {
  font-size: 13px;
  dominant-baseline: hanging;
}

@media (max-width: 630px) {
  .tex2 {
    font-size: 14px;
  }
}

@media (max-width: 540px) {
  .tex2 {
    font-size: 16px;
  }
}

@media (max-width: 480px) {
  .tex2 {
    font-size: 18px;
  }
}

@media (max-width: 360px) {
  .tex2 {
    font-size: 23px;
  }
}

.tex3 {
  font-size: 13px;
  dominant-baseline: hanging;
}

@media (max-width: 630px) {
  .tex3 {
    font-size: 14px;
  }
}

@media (max-width: 540px) {
  .tex3 {
    font-size: 16px;
  }
}

@media (max-width: 480px) {
  .tex3 {
    font-size: 18px;
  }
}

@media (max-width: 360px) {
  .tex3 {
    font-size: 23px;
  }
}
</style>
