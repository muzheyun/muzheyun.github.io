<template>
  <a-row class="sub-title">
    {{ props.title }}
  </a-row>
</template>

<script setup>
let props = defineProps({
  title: {
    type: String,
    required: true
  },
});

</script>

<style scoped>
.sub-title {
  color: white;
  background: #181818;
  font-weight: 700;
  width: 100%;
  padding: 5px;
  border: 3px solid #181818;
}
</style>