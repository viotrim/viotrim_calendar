<template>
  <header class="nav-header-container">
    <div class="nav-button-warper">
      <button
        @click="goToPreviousDay"
        class="nav-button-left"
        id="nav-button-left"
      >
        <label for="nav-button-left" class="nav-button-left"> ←</label>
      </button>
    </div>
    <div class="current-date-warper" @click="onChooseDate">
      <div v-if="!isEditingDate" class="current-date">{{ currentDate }}</div>
    </div>
    <div class="nav-button-warper">
      <button
        @click="goToNextDay"
        class="nav-button-right"
        id="nav-button-right"
      >
        <label for="nav-button-right" class="nav-button-right">→</label>
      </button>
    </div>
  </header>
  <div v-if="isEditingDate" class="overlay" @click="closeCalendar"></div>
  <div
    v-if="isEditingDate"
    class="calendar-modal"
    ref="datechoose"
    tabindex="0"
    @keydown="handleKeydown"
  >
    <DatePicker
      v-model.string="localDate"
      transparent
      borderless
      expanded
      :masks="masks"
    />
  </div>
</template>

<script>
import { ref, watch } from 'vue';
import { Calendar, DatePicker } from 'v-calendar';
import 'v-calendar/style.css';

export default {
  components: { DatePicker },
  props: {
    currentDate: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      isEditingDate: false,
      localDate: this.currentDate,
      masks: ref({
        modelValue: 'YYYY-MM-DD',
      }),
    };
  },
  methods: {
    goToPreviousDay() {
      const currentDate = new Date(this.currentDate);
      currentDate.setDate(currentDate.getDate() - 1);
      const newDate = currentDate.toISOString().split('T')[0]; // 将日期转换为 YYYY-MM-DD 格式
      this.localDate = newDate;
      this.updateDate(newDate);
    },
    goToNextDay() {
      const currentDate = new Date(this.currentDate);
      currentDate.setDate(currentDate.getDate() + 1);
      const newDate = currentDate.toISOString().split('T')[0]; // 将日期转换为 YYYY-MM-DD 格式
      this.localDate = newDate;
      this.updateDate(newDate);
    },
    onChooseDate() {
      this.isEditingDate = true;
      this.$nextTick(() => {
        this.$refs.datechoose.focus();
      });
    },
    closeCalendar(event) {
      if (event.target.className === 'overlay') {
        this.saveDate();
      }
    },
    handleKeydown(event) {
      if (event.key === 'Enter') {
        event.preventDefault();
        this.saveDate();
      }
    },
    saveDate() {
      this.isEditingDate = false;
      this.updateDate(this.localDate || this.currentDate); // 确保 localDate 有效
    },
    updateDate(newDate) {
      this.$emit('update:currentDate', newDate);
    },
  },
  watch: {
    currentDate(newVal) {
      this.localDate = newVal;
    },
  },
};
</script>

<style scoped>
.nav-header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #fffff1;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.18);
  border-bottom: 2px solid #ddd; /* 添加下划线 */
  width: 100%; /* 确保宽度为100% */
  margin: 0 auto; /* 居中对齐 */
  position: sticky;
  top: 0; /* 使头部固定在顶部 */
  z-index: 100; /* 确保头部在其他内容之上 */
  cursor: pointer;
}

.nav-button-left,
.nav-button-right {
  font-size: 18px;
  width: auto;
  padding: 5px 10px;
  cursor: pointer;
  background-color: transparent;
  border: none;
  color: #333;
}

.nav-button-left:hover {
  color: #007bff;
  cursor: pointer;
}

.nav-button-right:hover {
  color: #007bff;
  cursor: pointer;
}
.nav-button-warper:hover {
  cursor: pointer;
}

.current-date {
  font-size: 20px;
  font-weight: bold;
  margin: 0 10px; /* 为日期文本添加一些间距 */
}

.current-date-warper {
  flex-grow: 1;
  text-align: center;
  cursor: pointer;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1000;
}

.calendar-modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  padding: 20px;
  border-radius: 5px;
  z-index: 1001;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  width: 100%; /* 增加宽度 */
  height: 38%; /* 增加高度 */
  max-width: 600px; /* 最大宽度限制 */
  max-height: 600px; /* 最大高度限制 */
}
</style>
