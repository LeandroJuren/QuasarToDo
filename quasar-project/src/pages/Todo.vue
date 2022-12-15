<template>
  <q-page class="q-pa-lg bg-red-3 column">
    <!-- <h5 class="q-mt-none">Todo</h5> -->
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
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  data() {
    return {
      tasks: [
        {
          title: "Get apple",
          done: false,
        },
        {
          title: "Eat apple",
          done: false,
        },
        {
          title: "Poo apple",
          done: true,
        },
      ],
    };
  },
  methods: {
    deleteTask(i) {
      this.$q
        .dialog({
          dark: true,
          title: "Delete task",
          message: "Are you sure?",
          cancel: true,
          persistent: false,
        })
        .onOk(() => {
          this.tasks.splice(i, 1);
        });
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
</style>
