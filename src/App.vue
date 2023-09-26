<template>
  <div class="grid-background"/>
  <a-row style="height: 10px"></a-row>
  <a-row justify="center">
      <h1>App Privacy Policy Generator</h1>
  </a-row>
  <a-row justify="center">
      <h3>Generate a generic Privacy Policy and Terms & Conditions for your apps</h3>
  </a-row>
  <a-row justify="center">
      <h4>Built with
        <a-icon :style="{ color: 'hotpink' }">
          <svg width="1em" height="1em" fill="currentColor" viewBox="0 0 1024 1024">
            <path
                d="M923 283.6c-13.4-31.1-32.6-58.9-56.9-82.8-24.3-23.8-52.5-42.4-84-55.5-32.5-13.5-66.9-20.3-102.4-20.3-49.3 0-97.4 13.5-139.2 39-10 6.1-19.5 12.8-28.5 20.1-9-7.3-18.5-14-28.5-20.1-41.8-25.5-89.9-39-139.2-39-35.5 0-69.9 6.8-102.4 20.3-31.4 13-59.7 31.7-84 55.5-24.4 23.9-43.5 51.7-56.9 82.8-13.9 32.3-21 66.6-21 101.9 0 33.3 6.8 68 20.3 103.3 11.3 29.5 27.5 60.1 48.2 91 32.8 48.9 77.9 99.9 133.9 151.6 92.8 85.7 184.7 144.9 188.6 147.3l23.7 15.2c10.5 6.7 24 6.7 34.5 0l23.7-15.2c3.9-2.5 95.7-61.6 188.6-147.3 56-51.7 101.1-102.7 133.9-151.6 20.7-30.9 37-61.5 48.2-91 13.5-35.3 20.3-70 20.3-103.3 0.1-35.3-7-69.6-20.9-101.9z"
            />
          </svg>
        </a-icon>
        by Nishant and contributors.</h4>
  </a-row>

  <a-row style="height: 30px"></a-row>
  <a-row>
    <a-col :span="11">
      <a-row justify="end">
        <a-col>
          <img v-if="currentPage === 1" alt="Vue logo" class="logo" src="./assets/step_1.svg" width="400" height="400" />
          <img v-else-if="currentPage === 2" alt="Vue logo" class="logo" src="./assets/step_2.svg" width="400" height="400" />
          <img v-else-if="currentPage === 3" alt="Vue logo" class="logo" src="./assets/step_3.svg" width="400" height="400" />
        </a-col>
      </a-row>

      <a-row style="height: 30px"></a-row>
      <a-row justify="end">
        <a-col
            :span="3"
            :pull="1"
        >
          <a-button
              @click="currentPage -= 1"
              type="primary"
              :disabled="currentPage === 1"
              key="prePage"
              shape="round"
          >
            <template #icon>
              <LeftOutlined />
            </template>
          </a-button>
        </a-col>
        <a-col
            :span="3"
            key="button2"
        >
          <a-button
              @click="currentPage += 1"
              type="primary"
              :disabled="currentPage === 3"
              key="nextPage"
              shape="round">
            <template #icon>
              <RightOutlined />
            </template>
          </a-button>
        </a-col>
      </a-row>
    </a-col>
    <a-col :span="13">
      <TheWelcome :current-page="currentPage" />
    </a-col>
  </a-row>
</template>


<script setup>
import html2canvas from 'html2canvas'
import TheWelcome from './components/RightTable.vue'
import {ref} from 'vue'
import { LeftOutlined, RightOutlined } from '@ant-design/icons-vue'
const currentPage = ref(1);

</script>
