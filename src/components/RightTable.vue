<template>
  <a-row justify="center">
    <a-col
    >
      <a-card
          class="card-class"
      >
        <a-form
            :model="formState1"
            v-if="currentPage === 1"
            key="form1"
        >
          <div v-for="item in formState1" :key="item.name">
            <div v-if="item.type !== 'subTitle'">
              <h4> {{ item.question }}</h4>
              <a-form-item>
                <a-input
                    v-model:value="item.value"
                    :placeholder="item.desc"
                />
              </a-form-item>
            </div>
          </div>
        </a-form>
        <a-form
            :model="formState2"
            v-else-if="currentPage === 2"
            key="form2"
        >
          <div v-for="item in formState2" :key="item.name">
            <div v-if="item.type !== 'subTitle'">
              <h4> {{ item.question }}</h4>
              <a-form-item>
                <a-input
                    v-if="item.type === 'input'"
                    v-model:value="item.value"
                    :placeholder="item.desc"
                />
                <a-select
                    v-else-if="item.type === 'choice'"
                    :placeholder="item.desc"
                    v-model:value="item.value"
                >
                  <a-select-option value="Yes">Yes</a-select-option>
                  <a-select-option value="No">No</a-select-option>
                </a-select>

                <a-input-number
                    v-else-if="item.type === 'number'"
                    addon-after="Days"
                    :min="0"
                    :placeholder="item.desc"
                    v-model:value="item.value"
                />
              </a-form-item>
            </div>
          </div>
        </a-form>
        <a-form
            :model="formState3"
            v-else-if="currentPage === 3"
            key="form3"
        >
          <div v-for="item in formState3" :key="item.name">
            <div v-if="item.type !== 'subTitle'">
              <h4> {{ item.question }}</h4>
              <a-form-item>
                <a-input
                    v-if="item.type === 'input'"
                    v-model:value="item.value"
                    :placeholder="item.desc"
                />
                <a-select
                    v-else-if="item.type === 'choice'"
                    :placeholder="item.desc"
                    v-model:value="item.value"
                >
                  <a-select-option value="Yes">Yes</a-select-option>
                  <a-select-option value="No">No</a-select-option>
                </a-select>

                <a-input-number
                    v-else-if="item.type === 'number'"
                    addon-after="Days"
                    :min="0"
                    :placeholder="item.desc"
                    v-model:value="item.value"
                />
              </a-form-item>
            </div>
          </div>
          <a-form-item>
            <a-button type="primary" @click="fullData">fullData</a-button>
          </a-form-item>
          <a-form-item>
            <a-button type="primary" @click="onSubmit">Generate</a-button>
          </a-form-item>
        </a-form>
      </a-card>
    </a-col>
  </a-row>
  <Result :formState="formState1" :open="open" :models="models" @close="close"/>
</template>

<script setup>
import Result from './Result.vue'
import {reactive, toRaw, ref} from 'vue';
import html2canvas from "html2canvas";

const formState1 = reactive([
  {name: 'subTitle', value: 'General Information', type: 'subTitle'},
  {name: 'appName', value: '', question: 'What is the name of your mobile app?', desc: 'App’s Name', type: 'input'},
  {name: 'developerName', value: '', question: 'What is the name of the developer?', desc: 'Developer’s Name', type: 'input'},
  {name: 'developerContactInformation', value: '', question: 'What is the contact information of the developer?', desc: 'Contact Information', type: 'input'},
  {name: 'effectiveDate', value: '', question: 'What is the effective date of this privacy nutrition label?', desc: 'Effective Date', type: 'input'},
]);
const formState2 = reactive([
  {name: 'subTitle', value: 'Privacy Practice', type: 'subTitle'},
  {name: 'isEncrypted', value: null, question: 'Whether users’ data are encrypted and transferred over a secure connection?', type: 'choice', desc: 'Data Encryption'},
  {name: 'governmentRequests', value: null, question: 'Whether government entities can request access to users\' data for reasons such as national security or criminal investigations?', type: 'choice', desc: 'Government Requests'},
  {name: 'dataBreachNotification', value: null, question: 'In the event of a data breach where users\' information is compromised, whether the affected users will be notified promptly?', type: 'choice', desc: 'Data Breach Notification'},
  {name: 'rightToAccess', value: null, question: 'Whether users have the right to request access to their personal information?', type: 'choice', desc: 'Right to Access'},
  {name: 'dataRetention', question: 'How long will the user’s data will be stored?', value: 0, desc: 'Data Retention', type: 'number', addOnAfter: ' Days'},
  {name: 'forgotten', value: null, question: 'Whether users have the right to delete their personal information?', type: 'choice', desc: 'Right to be Forgotten'},
]);
const formState3 = reactive([
  {name: 'subTitle', value: 'Data Practice', type: 'subTitle'},
  {name: 'sensitiveInformation', value: null, question: 'Will the app collect sensitive personal information?', type: 'choice', desc: 'Sensitive Personal Information'},
  {name: 'whatInformation', value: '', question: 'What sensitive personal information could be collected? (Health, Biometric, Gender, Information revealing race or ethnic origin, Government Identifier)', desc: 'Sensitive Personal Information', type: 'input'},
  {name: 'dataSharing', value: null, question: 'Will users’ data be shared with other companies or organisations?', type: 'choice', desc: 'Data Sharing'},
  {name: 'dataSelling', value: null, question: 'Will users’ data be sold to other companies or organisations?', type: 'choice', desc: 'Data Selling'},
]);

const models = reactive([
    formState1,
    formState2,
    formState3
])
let open = ref(false);

function onSubmit() {
  console.log('submit!', toRaw(formState1));
  open.value = true;
  // open = true;
  capture();
}

function fullData() {
  formState1.forEach((item) => {
    console.log(item)
    if (item.type === 'input') {
      item.value = item.desc;
    }
    if (item.type === 'choice') {
      item.value = 'Yes';
    }
  })
  formState2.forEach((item) => {
    if (item.type === 'input') {
      item.value = item.desc;
    }
    if (item.type === 'choice') {
      item.value = 'Yes';
    }
  })
  formState3.forEach((item) => {
    if (item.type === 'input') {
      item.value = item.desc;
    }
    if (item.type === 'choice') {
      item.value = 'Yes';
    }
  })
  console.log('fullData!', toRaw(formState1));
}

function close() {
  open.value = false;
  console.log('close!');
}

function handleAppTypeChange(value) {
  console.log(`selected ${value}`);
}

function handleMobileOs(value) {
  console.log(`selected ${value}`);
}

defineProps({
  currentPage: {
    type: Number,
    required: true
  }
})

function capture() {
  console.log('capture')
  console.log(document.querySelector("#capture"))
  // return
  setTimeout(() => {
    html2canvas(document.querySelector("#capture"), {
      dpi: 1000,
    }).then((itsCanvas) => {
      // document.body.appendChild(itsCanvas);
      downloadQRImg(itsCanvas, name);
    });
  }, 100);
}

/** 根据URL下载图片 */
function downloadQRImg(canvas, name) {
  /** 新Image对象，可以理解为DOM */
  const image = new Image();
  /** 解决跨域 Canvas 污染问题 */
  // image.setAttribute('crossOrigin', 'anonymous');
  // image.onload = function() {
  /** canvas.toDataURL 返回的是一串Base64编码的URL,指定格式 PNG */
  const imgUrl = canvas.toDataURL('image/png');
  image.src = imgUrl;
  /** 生成一个a元素,并创建一个单击事件 */
  const a = document.createElement('a');
  a.download = name || 'photo'; // 设置图片名称
  a.href = imgUrl; // 将生成的URL设置为a.href属性
  a.setAttribute('id', 'myLink');
  // 开发者工具查看生成的链接href的值是否是一个完整的base64格式图片
  // console.log('标签', a);
  document.body.appendChild(a);
  exportCodeConfirm();
  // }
}

function exportCodeConfirm() {
  setTimeout(() => {
    const event = new MouseEvent('click');
    /** 触发a的单击事件 */
    document.getElementById('myLink').dispatchEvent(event);
  }, 0);
}
</script>

<style>

.card-class {
  width: 500px;
}
</style>