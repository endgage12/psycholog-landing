<template>
  <el-config-provider :locale="ru">
    <el-calendar v-model="value">
      <template #date-cell="{ data }">
        <div class="w-full h-full" @click="openTimeModal(data.date)">
          {{ formatDate(data.date, isMobile) }}
        </div>
      </template>
    </el-calendar>

    <ElDrawer v-model="isTimeModalVisible" direction="btt" :z-index="2001">
      <template #header>
        <h4>Записаться на прием</h4>
      </template>

      <template #default>
        <div class="flex flex-col gap-4">
          <span>{{ formatDate(pickedDate) }}</span>

          <div class="flex items-center justify-center gap-2">
            <el-button>11:00</el-button>
            <el-button>12:00</el-button>
            <el-button>13:00</el-button>
            <el-button>14:00</el-button>
          </div>
        </div>
      </template>
    </ElDrawer>
  </el-config-provider>
</template>

<script setup lang="ts">
import {
  ElCalendar,
  ElDialog,
  ElButton,
  ElDrawer,
  ElConfigProvider,
} from "element-plus";
import ru from "element-plus/es/locale/lang/ru";
import { onBeforeMount, ref } from "vue";

const value = ref(new Date());
const pickedDate = ref(null);

const isTimeModalVisible = ref(false);
const isMobile = ref(false);

const openTimeModal = (pDate) => {
  pickedDate.value = pDate;
  isTimeModalVisible.value = true;
};

const formatDate = (date: Date, mini = false) => {
  return mini
    ? date.getDate()
    : new Intl.DateTimeFormat("ru-RU", {
        weekday: "long",
        day: "numeric",
        month: "long",
      }).format(date);
};

onBeforeMount(() => {
  isMobile.value = window.innerWidth < 768;
});
</script>

<style scoped></style>
