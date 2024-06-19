<template>
  <h4>Your balance</h4>
  <h1 id="balance">${{ total }}</h1>
</template>

<script setup>
import {defineProps} from "vue";

const props = defineProps({
  total: {
    type: String,
    required: true
  }
});

</script>
