<template>
  <div class="mainTable" id="capture">
  </div>
  <a-row>

  </a-row>
</template>

<script setup>
let props = defineProps({
  formState: {
    type: Object,
    required: true
  }
});

</script>

<style scoped>
.mainTable {
  padding: 1px;
}
</style>