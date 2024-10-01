<template>
  <div class="app-container" @mousemove="handleMouseMove">
    <div class="app-inner-container" ref="calendarInnerContainer">
      <div class="nav-header-warper">
        <NavHeader
          :currentDate="currentDate"
          @update:currentDate="updateDate"
          class="header-bar"
          :class="{ visible: headerVisible }"
          ref="headerBar"
        />
      </div>

      <div class="calendar-block-group-warper content">
        <MyCalendar :tasks="tasks" @update:tasks="updateTasks"></MyCalendar>
      </div>
    </div>
  </div>
</template>

<script>
import NavHeader from './components/NavHeader.vue';
import MyCalendar from './components/MyCalendar.vue';
import uniqueId from 'lodash.uniqueid';

export default {
  components: { NavHeader, MyCalendar },
  data() {
    return {
      currentDate: new Date().toISOString().split('T')[0],
      scrollInterval: null,
      scrollDirection: null,
      currentY: 0,
      alpha: 1 / 6, //控制滑动区域
      scrollOn: false,
      scrollDown: true,
      tasks: [
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '8:00',
          task: 'make lunch',
        },
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '9:00',
          task: 'eat lunch',
        },
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '10:00',
          task: 'learn lunch',
        },
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '11:00',
          task: 'simachi lunch',
        },
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '11:00',
          task: 'simachi lunch',
        },
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '11:00',
          task: 'simachi lunch',
        },
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '11:00',
          task: 'simachi lunch',
        },
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '11:00',
          task: 'simachi lunch',
        },
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '11:00',
          task: 'simachi lunch',
        },
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '11:00',
          task: 'simachi lunch',
        },
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '11:00',
          task: 'simachi lunch',
        },
        {
          id: uniqueId('task-'),
          date: '2024-9-27',
          time: '11:00',
          task: 'simachi lunch',
        },
      ],
    };
  },
  computed: {
    speed() {
      return (
        Math.pow(
          Math.abs(
            Math.abs(this.currentY - window.innerHeight / 2) -
              window.innerHeight * Math.abs(this.alpha, 1 / 2)
          ),
          1
        ) * 1
      );
    },
  },
  methods: {
    updateTasks(newTasks) {
      this.tasks = newTasks;
    },
    updateDate(newDate) {
      this.currentDate = newDate;
    },
    handleMouseMove(event) {
      const windowHeight = window.innerHeight;
      this.currentY = event.clientY;
      const container = this.$refs.calendarInnerContainer;

      if (this.currentY < windowHeight * this.alpha) {
        // 鼠标位于上方屏幕
        this.scrollDirection = 'up';
      } else if (this.currentY > windowHeight * (1 - this.alpha)) {
        // 鼠标位于下方屏幕
        this.scrollDirection = 'down';
      } else {
        this.scrollDirection = null;
      }

      if (this.scrollDirection) {
        this.startScrolling(event, container);
      } else {
        this.stopScrolling();
      }
    },
    startScrolling(event, container) {
      if (this.scrollInterval) return;

      this.scrollInterval = setInterval(() => {
        if (container) {
          if (this.scrollDirection === 'up' && this.scrollOn) {
            container.scrollTop -= this.speed;
          } else if (this.scrollDirection === 'down' && this.scrollDown) {
            container.scrollTop += this.speed;
          }
        }
      }, 100);
      // 添加一个方法来停止滚动
      this.stopScrolling = () => {
        if (this.scrollInterval) {
          clearInterval(this.scrollInterval);
          this.scrollInterval = null;
        }
      };
    },
    stopScrolling() {
      if (this.scrollInterval) {
        clearInterval(this.scrollInterval);
        this.scrollInterval = null;
      }
    },
  },
  mounted() {
    window.addEventListener('mousemove', this.handleMouseMove);
  },
  beforeDestroy() {
    window.removeEventListener('mousemove', this.handleMouseMove);
    this.stopScrolling();
  },
};
</script>

<style scoped>
.app-container {
  height: 96vh; /* 使容器高度为视口高度 */
  width: 99vw;
  display: flex;
  flex-direction: column;
  text-align: center; /* 使子元素居中 */
  ::-webkit-scrollbar {
    width: -0px;
    display: none;
  }
  scrollbar-color: auto;
}

.content {
  flex: 1;
  overflow-y: auto;
}

.content::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
}
.app-inner-container::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
}

.calendar-block-group-warper,
.nav-header-warper {
  display: flex; /* 改为 flex 布局 */
  justify-content: center; /* 水平居中 */
  max-width: 1200px; /* 限制最大宽度，可以根据需要调整 */
  margin: 0 auto; /* 水平居中 */
  width: 100%; /* 使容器占据父容器的全宽 */
}

.app-inner-container {
  overflow: auto;
  scroll-behavior: smooth;
  scrollbar-color: auto;
}
</style>
