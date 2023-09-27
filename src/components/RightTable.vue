<template>
  <a-row justify="center">
    <a-col
    >
      <a-card
          class="card-class"
      >
        <a-form
            :model="formState"
            v-if="currentPage === 1"
            key="form1"
        >
          <h4>What is the name of your mobile app?</h4>
          <a-form-item>
            <a-input
                v-model:value="formState.appName"
                placeholder="App’s Name"
            />
          </a-form-item>

          <a-form-item>
          <h4>What is the name of the developer?</h4>
            <a-input
                v-model:value="formState.developerName"
                placeholder="Developer’s Name"
            />
          </a-form-item>
          <a-form-item>
          <h4>What is the contact information of the developer?</h4>
            <a-input
                v-model:value="formState.developerContactInformation"
                placeholder="Contact Information"
            />
          </a-form-item>
          <a-form-item>
            <h4>What is the effective date of this privacy nutrition label?</h4>
            <a-input
                v-model:value="formState.effectiveDate"
                placeholder="Effective Date"
            />
          </a-form-item>

        </a-form>
        <a-form
            :model="formState"
            v-else-if="currentPage === 2"
            key="form2"
        >
          <a-form-item>
            <h4>Whether users’ data are encrypted and transferred over a secure connection?</h4>
            <a-select
                placeholder="Data Encryption"
                v-model:value="formState.isEncrypted"
                @change="handleAppTypeChange"
            >
              <a-select-option value="Yes">Yes</a-select-option>
              <a-select-option value="No">No</a-select-option>
            </a-select>
          </a-form-item>
          <a-form-item>
            <h4>Whether government entities can request access to users' data for reasons such as national security or criminal investigations?</h4>
            <a-select
                placeholder="Government Requests"
                v-model:value="formState.governmentRequests"
                @change="handleAppTypeChange"
            >
              <a-select-option value="Yes">Yes</a-select-option>
              <a-select-option value="No">No</a-select-option>
            </a-select>
          </a-form-item>
          <h4>In the event of a data breach where users' information is compromised, whether the affected users will be notified promptly?</h4>
          <a-form-item>
            <a-select
                placeholder="Data Breach Notification"
                v-model:value="formState.dataBreachNotification"
                @change="handleAppTypeChange"
            >
              <a-select-option value="Yes">Yes</a-select-option>
              <a-select-option value="No">No</a-select-option>
            </a-select>
          </a-form-item>
          <a-form-item>
            <h4>Whether users have the right to request access to their personal information?</h4>
            <a-select
                placeholder="Right to Access"
                v-model:value="formState.rightToAccess"
                @change="handleAppTypeChange"
            >
              <a-select-option value="Yes">Yes</a-select-option>
              <a-select-option value="No">No</a-select-option>
            </a-select>
          </a-form-item>
          <a-form-item>
            <h4>How long will the user’s data will be stored?</h4>
            <a-input-number
                addon-after="Days"
                :min="0"
                v-model:value="formState.dataRetention"
                placeholder="Data Retention (xx days)"
            />
          </a-form-item>
          <a-form-item>
            <h4>Whether users have the right to delete their personal information?</h4>
            <a-select
                placeholder="Right to be Forgotten"
                v-model:value="formState.forgotten"
                @change="handleAppTypeChange"
            >
              <a-select-option value="Yes">Yes</a-select-option>
              <a-select-option value="No">No</a-select-option>
            </a-select>
          </a-form-item>
        </a-form>
        <a-form
            :model="formState"
            v-else-if="currentPage === 3"
            key="form3"
        >
          <a-form-item>
            <h4>Will the app collect sensitive personal information?</h4>
            <a-select
                placeholder="Sensitive Personal Information"
                v-model:value="formState.sensitiveInformation"
                @change="handleAppTypeChange"
            >
              <a-select-option value="Yes">Yes</a-select-option>
              <a-select-option value="No">No</a-select-option>
            </a-select>
          </a-form-item>
          <a-form-item v-if="formState.sensitiveInformation === 'Yes'">
            <h4>(If Yes) What sensitive personal information could be collected? (Health, Biometric, Gender, Information revealing race or ethnic origin, Government Identifier)</h4>
            <a-input
                v-if="formState.sensitiveInformation === 'Yes'"
                v-model:value="formState.whatInformation"
                placeholder="Sensitive Personal Information"
            />
          </a-form-item>
          <a-form-item>
            <h4>Will users’ data be shared with other companies or organisations?</h4>
            <a-select
                placeholder="Data Sharing"
                v-model:value="formState.dataSharing"
                @change="handleAppTypeChange"
            >
              <a-select-option value="Yes">Yes</a-select-option>
              <a-select-option value="No">No</a-select-option>
            </a-select>
          </a-form-item>
          <a-form-item>
            <h4>Will users’ data be sold to other companies or organisations?</h4>
            <a-select
                placeholder="Data Selling"
                v-model:value="formState.dataSelling"
                @change="handleAppTypeChange"
            >
              <a-select-option value="Yes">Yes</a-select-option>
              <a-select-option value="No">No</a-select-option>
            </a-select>
          </a-form-item>
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
  <Result :formState="formState" :open="open" @close="close"/>
</template>

<script setup>
import Result from './Result.vue'
import { reactive, toRaw, ref } from 'vue';
import html2canvas from "html2canvas";
const formState = reactive({
  appName: '',
  developerName: '',
  developerContactInformation: '',
  effectiveDate: '',
  isEncrypted: null,
  governmentRequests: null,
  dataBreachNotification: null,
  rightToAccess: null,
  dataRetention: 0,
  forgotten: null,
  sensitiveInformation: null,
  whatInformation: '',
  dataSharing: null,
  dataSelling: null,
});
let open = ref(false);
function onSubmit() {
  console.log('submit!', toRaw(formState));
  open.value = true;
  // open = true;
  capture();
}
function fullData() {
  formState.appName = 'appName';
  formState.developerName = 'developerName';
  formState.developerContactInformation = 'developerContactInformation';
  formState.effectiveDate = 'effectiveDate';
  formState.isEncrypted = 'Yes';
  formState.governmentRequests = 'Yes';
  formState.dataBreachNotification = 'Yes';
  formState.rightToAccess = 'Yes';
  formState.dataRetention = 10;
  formState.forgotten = 'Yes';
  formState.sensitiveInformation = 'Yes';
  formState.whatInformation = 'whatInformation';
  formState.dataSharing = 'Yes';
  formState.dataSelling = 'Yes';
  console.log('fullData!', toRaw(formState));
}
function close() {
  open.value = false;
  console.log('close!');
}
function handleAppTypeChange (value) {
  console.log(`selected ${value}`);
}
function handleMobileOs (value) {
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
  return
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