<template>
  <a-card
      style="width: 600px"
      id="capture"
  >
    <a-form
        :model="formState"
        :label-col="labelCol"
        :wrapper-col="wrapperCol">
      <h4>App Name</h4>
      <a-form-item>
        <a-input
            style="width: 400px"
            v-model:value="formState.appName"
            placeholder="Awesome App Name!"
            width="600px"/>
      </a-form-item>

      <h4>Contact Information</h4>
      <a-form-item>
        <a-input
            style="width: 400px"
            v-model:value="formState.contractInformation"
            placeholder="Email Adress"
        />
      </a-form-item>
      <h4>Personally Identifiable Information</h4>
      <a-form-item>
        <a-input
            style="width: 400px"
            v-model:value="formState.personllyIdentInfo"
            placeholder="Personally Identifiable Information you collect (comma separated)"
        />
      </a-form-item>
      <h4>App Type</h4>
      <a-select
          ref="appType"
          placeholder="Select App Type"
          v-model:value="formState.appType"
          style="width: 400px"
          @change="this.handleAppTypeChange"
      >
        <a-select-option value="Select app type"  disabled>Select app type</a-select-option>
        <a-select-option value="Free">Free</a-select-option>
        <a-select-option value="Open Source">Open Source</a-select-option>
        <a-select-option value="Freemium">Freemium</a-select-option>
        <a-select-option value="Ad Supported">Ad Supported</a-select-option>
        <a-select-option value="Commercial">Commercial</a-select-option>
      </a-select>
      <h4>Mobile OS</h4>
      <a-select
          ref="mobileOs"
          style="width: 400px"
          v-model:value="formState.mobileOs"
          @change="this.handleMobileOs"
      >
        <a-select-option value="Select mobile OS" disabled>Select mobile OS</a-select-option>
        <a-select-option value="Android">Android</a-select-option>
        <a-select-option value="iOS">iOS</a-select-option>
      </a-select>
      <h4>Policy Effective Date</h4>
      <a-date-picker
          style="width: 400px"
          v-model:value="policyEffectiveDate"
      />
    </a-form>
  </a-card>
</template>

<script setup>
import { reactive, toRaw, ref } from 'vue';
const formState = reactive({
  appName: '',
  contractInformation: '',
  personllyIdentInfo: '',
  appType: 'Select app type',
  mobileOs: 'Select mobile OS',
  delivery: false,
  type: [],
  resource: '',
  desc: '',
});
const policyEffectiveDate = ref();
const onSubmit = () => {
  console.log('submit!', toRaw(formState));
};
const labelCol = {
  style: {
    width: '150px',
  },
};
const wrapperCol = {
  span: 14,
};
const handleAppTypeChange = value => {
  console.log(`selected ${value}`);
};
const handleMobileOs = value => {
  console.log(`selected ${value}`);
};
</script>
