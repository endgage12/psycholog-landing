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
            <el-button @click="pickTime('11:00')">11:00</el-button>
            <el-button @click="pickTime('12:00')">12:00</el-button>
            <el-button @click="pickTime('13:00')">13:00</el-button>
            <el-button @click="pickTime('14:00')">14:00</el-button>
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
import axios from "axios";

const value = ref(new Date());
const pickedDate = ref(null);
const pickedTime = ref(null);

const isTimeModalVisible = ref(false);
const isMobile = ref(false);

const openTimeModal = (pDate) => {
  pickedDate.value = pDate;
  isTimeModalVisible.value = true;
};

const pickTime = (time) => {
  pickedTime.value = time;
  sendTelegramMessage();
};

const sendTelegramMessage = async () => {
  const token = "8119592694:AAFioIvcvcKhUNWb25negaSh2eNHH_2OPM0";
  if (pickedDate.value && pickedTime.value) {
    const formattedDate = formatDate(pickedDate.value);
    const message = `Клиент записался на ${pickedTime.value} ${formattedDate}`;
    const url = `https://api.telegram.org/bot${token}/sendMessage`;

    try {
      const response = await axios.post(
        url,
        {
          chat_id: "852081426",
          text: message,
          parse_mode: "HTML",
        },
        {
          headers: {
            "Content-Type": "application/json",
          },
        },
      );
      console.log("Сообщение отправлено в Telegram", response.data);
    } catch (error) {
      console.error("Ошибка при отправке сообщения в Telegram:", error);
    }
  }
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
