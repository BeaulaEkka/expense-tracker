<template>
  <h4>Your Balance</h4>
  <h1 id="balance">&euro;{{ total }}</h1>
</template>
<script setup>
import { defineProps } from "vue";
const Props = defineProps({
  total: { Number, required: true },
});
</script>
