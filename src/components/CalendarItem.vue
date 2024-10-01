<template>
  <div class="calendar-item-block">
    <div class="calendar-item-time-warper" @dblclick="editTime">
      <div v-if="!isEditingTime" class="task-time">{{ task.time }}</div>
      <input
        v-else
        type="text"
        class="task-time task-time-edit-mode input-style"
        v-model="task.time"
        @blur="saveTime"
        @keyup.enter="saveTime"
        ref="timeInput"
      />
    </div>
    <div class="spacer-between-items"></div>
    <div class="calendar-item-content-warper" @dblclick="editTask">
      <div v-if="!isEditingTask" class="task-content">
        {{ task.task }}
      </div>
      <input
        v-else
        type="text"
        v-model="task.task"
        class="task-content-edit-mode task-content input-style"
        @blur="saveTask"
        @keyup.enter="saveTask"
        ref="taskInput"
      />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    task: {
      required: true,
    },
  },
  data() {
    return {
      isEditingTime: false,
      isEditingTask: false,
      isMoving: false,
      moveTimeout: null,
    };
  },
  methods: {
    editTime() {
      this.isEditingTime = true;
      this.$nextTick(() => {
        this.$refs.timeInput.focus();
      });
    },
    saveTime() {
      this.isEditingTime = false;
      // 这里可以调用一个方法来保存时间到数据库
      this.$emit('update-task', { ...this.task });
    },
    editTask() {
      this.isEditingTask = true;
      this.$nextTick(() => {
        this.$refs.taskInput.focus();
      });
    },
    saveTask() {
      this.isEditingTask = false;
      // 这里可以调用一个方法来保存任务到数据库
      this.$emit('update-task', { ...this.task });
    },
  },
};
</script>

<style scoped>
.task-time {
  font-family: 'Pacifico Regular', cursive;
  display: flex;
  height: 100%;
  font-weight: bold; /* 时间文本加粗 */
  color: #333; /* 设置时间文本颜色 */
  font-size: 2em;
  justify-content: center;
  text-align: center;
  align-items: center;
  padding: auto;
}

.task-content {
  font-family: Arial, 'Helvetica Neue', Helvetica, sans-serif;
  position: relative;
  width: 100%;
  color: #555; /* 设置任务描述文本颜色 */
  font-size: 1em;
  cursor: pointer; /* 鼠标悬停时显示指针 */
  text-align: left;
  padding: 20px;
}

.task-content:hover {
  /*text-decoration: underline;  鼠标悬停时下划线 */
}
.calendar-item-block {
  display: flex;
  width: 100%;
  box-sizing: border-box;
}
.calendar-item-content-warper {
  display: flex;
  width: auto;
  background-color: #f9f9f9; /* 设置背景颜色 */
  border-radius: 8px; /* 设置圆角边框 */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.18); /* 添加阴影效果 */
  color: #555; /* 设置任务描述文本颜色 */
  height: 20vh;
  width: 100%;
  box-sizing: border-box; /* 确保内边距和边框包含在宽度内 */
  align-items: flex-start; /* 垂直对齐到顶部 */
}

.calendar-item-time-warper {
  background-color: #ffffff; /* 设置背景颜色 */
  border-radius: 20px; /* 设置圆角边框 */
  width: 30%;
  box-sizing: border-box; /* 确保内边距和边框包含在宽度内 */
  align-items: center;
  align-items: flex-start; /* 垂直对齐到顶部 */
}
.spacer-between-items {
  display: inline-block; /* 使 spacer 成为行内块级元素 */
  width: 10vw; /* 插入 20px 的空格 */
  height: 1px; /* 设置高度为 1px，确保不占用额外空间 */
}
.task-content-edit-mode {
  background-color: transparent;
  border: 0; /* 无边框 */
  outline: none;
  padding: auto;
}
.task-time-edit-mode {
  background-color: transparent;
  border: 0; /* 无边框 */
  outline: none;
  padding: auto;
  /* overwrite! */
  padding-block: 0; /* 重置块方向的内边距 */
  padding-block-start: 0; /* 重置块方向开始的内边距 */
  padding-block-end: 0; /* 重置块方向结束的内边距 */
  padding-inline: 0; /* 重置行内方向的内边距 */
  padding-inline-start: 0; /* 重置行内方向开始的内边距 */
  padding-inline-end: 0; /* 重置行内方向结束的内边距 */
  border-radius: 4px; /* 设置圆角 */
  width: 100%; /* 宽度占满父容器 */
  box-sizing: border-box; /* 确保内边距和边框不会增加总宽度 */
}
.input-style {
  outline: none; /* 移除默认轮廓 */
  border: none;
  width: 100%;
  vertical-align: top; /* 垂直对齐到顶部 */
}
.input-style:focus {
  outline: none; /* 移除默认轮廓 */
  border: none;
  width: 100%;
}
</style>
