<template>
  <div class="calendar-table calendar-table-container">
    <div class="calendar-inner-container">
      <draggable
        :list="tasks"
        :disabled="!state.enabled"
        ghost-class="ghost"
        chosen-class="chosen"
        @start="state.dragging = true"
        @end="state.dragging = false"
        animation="300"
      >
        <template #item="{ element }">
          <div class="calendar-item-warper" :key="element.id">
            <calendar-item :task="element" />
          </div>
        </template>
      </draggable>
    </div>
  </div>
</template>

<script>
import CalendarItem from './CalendarItem.vue';
import draggable from 'vuedraggable';
export default {
  components: {
    CalendarItem,
    draggable,
  },
  props: {
    tasks: {
      required: true,
    },
  },
  data() {
    return {
      state: {
        enabled: true,
        dragging: false,
      },
    };
  },
  methods: {
    updateTasks(newTasks) {
      this.$emit('update:tasks', newTasks);
    },
  },
};
</script>

<style scoped>
.calendar-table {
  width: 100%; /* 确保表格宽度为100% */
  background-color: #fff; /* 设置背景颜色 */
  overflow: hidden;
  overflow-y: scroll;
}

.calendar-table-container {
  display: flex;
  justify-content: center; /* 水平居中对齐 */
  overflow: hidden;
  overflow-y: scroll;
}

.calendar-table {
  padding: 30px; /* 为单元格添加内边距 */
  border-right: 1px solid #ddd; /* 添加列之间的分隔线 */
  text-align: center;
}
.calendar-inner-container {
  width: 100%;
}
.calendar-item-warper {
  margin-top: 5vh;
  margin-bottom: 5vh;
  padding-left: 5vw;
  padding-right: 5vw;
}
.ghost {
  opacity: 0.5;
  border: 0px solid #18a058;
}
.not-draggable {
  cursor: no-drop;
}
.chosen {
  border: 0px solid #18a058;
}
</style>
