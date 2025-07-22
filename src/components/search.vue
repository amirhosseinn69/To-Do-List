<template>
  <div class="search">
    <form @submit.prevent="">
      <input v-model="searchValue" placeholder="search">
      <button>
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m15 15l6 6m-11-4a7 7 0 1 1 0-14a7 7 0 0 1 0 14"/></svg>
      </button>
    </form>
  </div>
</template>
<script setup>
import { defineProps, defineEmits, ref, watch } from 'vue';
const props = defineProps({
  modelValue: String
});
const emit = defineEmits(['update:modelValue']);
const searchValue = ref(props.modelValue || '');
watch(searchValue, val => emit('update:modelValue', val));
watch(() => props.modelValue, val => { if (val !== searchValue.value) searchValue.value = val; });
</script>