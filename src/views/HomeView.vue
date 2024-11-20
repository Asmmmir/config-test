<template>
  <main>
    <FormBuilder :onSubmit="handleOnSubmit" :config="formConfig" />
  </main>
</template>

<script setup>
import FormBuilder from "../components/FormBuilder.vue";
import formConfig from "../../form-config.json";

async function handleOnSubmit(data) {
  try {
    const response = await fetch("fakedata/api", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(data),
    });
    const result = await response.json();
  } catch (error) {
    console.error("Ошибка в отправке формы:", error);
  }
}
</script>
