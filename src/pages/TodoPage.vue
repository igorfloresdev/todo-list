<template>
  <q-page q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        class="col"
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Add Task"
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
        class="bg-white"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            color="primary"
            class="no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            round
            dense
            @click.stop="deleteTask(index)"
            color="primary"
            icon="delete"
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
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        // {
        //   title: "Get Bananas",
        //   done: false,
        // },
        // {
        //   title: "Eat Bananas",
        //   done: false,
        // },
        // {
        //   title: "Poo Bananas",
        //   done: false,
        // },
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Would you like to delete this task?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify({
            type: "warning",
            message: "Task Deleted !",
          });
        });
    },
    addTask() {
      if (this.newTask) {
        this.tasks.push({
          title: this.newTask,
          done: false,
        });
        this.$q.notify({
          type: "positive",
          message: this.newTask + " task add !",
        });

        this.newTask = "";
      } else {
        this.$q.notify({
          type: "negative",
          message: "Task name field is empty!",
        });
      }
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

.q-field__label,
input.q-field__native {
  padding: 0 20px;
}

.no-tasks {
  opacity: 0.5;
}
</style>
