<template>
  <main>
    <FormBuilder :config="formConfig" />
  </main>
</template>

<script setup>
import FormBuilder from '../components/FormBuilder.vue'
import formConfig from '../../form-config.json'
</script>
