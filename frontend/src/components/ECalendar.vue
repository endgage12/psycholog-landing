<template>
  <el-config-provider :locale="ru">
    <el-calendar v-model="value">
      <template #date-cell="{ data }">
        <div @click="console.log(data)">{{ formatDate(data.date) }}</div>
      </template>
    </el-calendar>
  </el-config-provider>
</template>

<script setup lang="ts">
import { ElCalendar, ElConfigProvider } from "element-plus";
import ru from "element-plus/es/locale/lang/ru";
import { ref } from "vue";

const value = ref(new Date());

const formatDate = (date: Date) => {
  return new Intl.DateTimeFormat("ru-RU", {
    weekday: "long",
    day: "numeric",
    month: "long",
  }).format(date);
};
</script>

<style scoped></style>
