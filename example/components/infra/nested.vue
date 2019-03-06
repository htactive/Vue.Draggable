<template>
  <draggable
    class="dragArea"
    tag="ul"
    :list="tasks"
    :group="{ name: level+'', put: [(level)+'',(level-1)+''] }"
    v-bind="dragOptions"
    @start="drag = true"
    @end="drag = false"
  >
    <transition-group type="transition" :name="!drag ? 'flip-list' : null">
      <li v-for="el in tasks" :key="el.name">
        <p>{{ el.name }}</p>
        <nested-draggable :tasks="el.tasks" :level="level+1"/>
      </li>
    </transition-group>
  </draggable>
</template>
<script>
import draggable from "@/vuedraggable";

export default {
  data() {
    return {
      drag: false
    };
  },
  props: {
    tasks: {
      required: true,
      type: Array
    },
    level: Number
  },
  components: {
    draggable
  },
  computed: {
    dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost"
      };
    }
  },
  name: "nested-draggable"
};
</script>
<style scoped>
.dragArea {
  min-height: 50px;
  outline: 1px dashed;
}
.button {
  margin-top: 35px;
}
.flip-list-move {
  transition: transform 0.5s;
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.list-group {
  min-height: 20px;
}
.list-group-item {
  cursor: move;
}
.list-group-item i {
  cursor: pointer;
}
</style>
