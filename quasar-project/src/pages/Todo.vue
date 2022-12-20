<template>
  <q-page class="q-pa-lg bg-red-3 column">
    <!-- <h5 class="q-mt-none">Todo</h5> -->
    <div class="row q-pa-sm bg-primary">
      <q-input
        class="col"
        square
        bg-color="cyan"
        v-model="newTask"
        placeholder="Add task"
        dense
        @keyup.enter="addTask"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-yellow" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        v-ripple
        @click="task.done = !task.done"
        clickable
        :class="{ 'done bg-blue-1': task.done }"
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section side v-if="task.done">
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            color="primary"
            icon="delete"
            dense
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">No tasks</div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  data() {
    return {
      newTask: "",
      tasks: [
        // {
        //   title: "Get apple",
        //   done: false,
        // },
        // {
        //   title: "Eat apple",
        //   done: false,
        // },
        // {
        //   title: "Poo apple",
        //   done: true,
        // },
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          dark: true,
          title: "Delete task",
          message: "Are you sure?",
          cancel: true,
          persistent: false,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Task deleted.");
        });
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.$q.notify(this.newTask);
      this.newTask = "";
    },
  },
});
</script>
<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: lightgreen;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
