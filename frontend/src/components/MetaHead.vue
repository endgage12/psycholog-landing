<template>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta
      name="description"
      content="Профессиональные психологические услуги в Анапе. Индивидуальный подход к каждому клиенту."
    />
    <meta
      name="keywords"
      content="психолог Анапа, психологические услуги, консультации психолога"
    />
    <title>{{ title }}</title>
    <link rel="icon" href="favicon.ico" type="image/x-icon" />
  </head>
</template>

<script setup lang="ts">
const props = defineProps({
  title: { type: String, required: true },
});
</script>

<style scoped></style>
