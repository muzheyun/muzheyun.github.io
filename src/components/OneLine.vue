<template>
  <a-row class="one-line">
    <a-col :span="12" class="one-line-title">
      {{ props.oneLine.desc }}
    </a-col>
    <a-col :span="12" class="one-line-right">
      {{ props.oneLine.value }}
      {{ props.oneLine.addOnAfter }}
    </a-col>
  </a-row>
</template>

<script setup>
let props = defineProps({
  oneLine: {
    type: Object,
    required: true
  },
});

</script>

<style scoped>
.one-line {
  color: #141B2B;
  width: 100%;
  padding: 10px;
  border: 3px solid #181818;
}
.one-line-title {
  text-align: left;
  font-weight: 700;
}
.one-line-right {
  text-align: right;
}
</style>