<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <!--task is added by pressing the enter key or clicking on the button-->
      <q-input
        @keyup.enter="addTask"
        v-model="newTask"
        class="col"
        filled
        square
        placeholder="Add a task"
        dense
        bg-color="white"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-deep-purple-1': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>
            {{ task.title }}
          </q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
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
    <div v-if="!tasks.length" class="noTasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">No tasks</div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        /* {
          title: "Task 1",
          done: false,
        },

        {
          title: "Task 2",
          done: false,
        },

        {
          title: "Task 3",
          done: false,
        }, */
      ],
    };
  },

  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Delete task",
          message: "Are you sure you want to delete this task?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify({
            message: "Task deleted",
            color: "negative",
            icon: "delete",
            timeout: 800,
            group: false,
          });
        });
    },

    addTask() {
      this.tasks.push({ title: this.newTask, done: false });
      this.newTask = "";
      this.$q.notify({
        message: "Task added",
        color: "positive",
        icon: "check",
        timeout: 800,
        group: false,
      });
    },
  },
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.noTasks {
  opacity: 0.5;
}
</style>
