<template>
  <q-page class="bg-grey-3 column">
    <q-list
    class="bg-white"
    separator
    bordered
    >
      <q-item v-ripple color="primary">
        <q-item-section>
          <q-input
          filled  v-model="newTask" label="Add task"
          @keydown.enter="addTask"
          />
        </q-item-section>
        <q-item-section side>
          <q-btn round push color="info" icon="add" @click="addTask"></q-btn>
        </q-item-section>
      </q-item>
      <q-item
      v-ripple v-for="(task, index) in tasks" v-bind:key="task.title"
      clickable @click="task.done = !task.done"
      :class="{'done bg-blue-1': task.done}"
      >
        <q-item-section avatar>
          <q-checkbox
          v-model="task.done" color="primary"
          class="no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
          <q-item-label caption v-if="task.done">
            Done
          </q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn round push color="negative" icon="delete" @click.stop="deleteTask(index)"></q-btn>
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-task absolute-center" v-if="tasks.length == 0">
      <q-icon
      name="check"
      size="100px"
      color="primary"
      class="center"
      />
      <div class="text-h5 text-primary text-center">
        no task
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'TodoPage',
  methods: {
    deleteTask (index) {
      this.$q.dialog({
        title: 'Detele task',
        message: 'Would you like to delete this task?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
        this.$q.notify({
          message: 'Task deleted.',
          color: 'primary'
        })
      })
    },
    addTask () {
      this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.newTask = ''
    }
  },
  data () {
    return {
      newTask: '',
      tasks: [
        // {
        //   title: 'Hái chuối',
        //   done: false
        // },
        // {
        //   title: 'Ăn chuối',
        //   done: false
        // },
        // {
        //   title: 'Vứt vỏ chuối',
        //   done: false
        // }
      ]
    }
  }
})
</script>

<style lang="scss">
  .done {
    .q-item__label{
      text-decoration: line-through;
      color: #bbb;
    }
  }
  .no-task {
    opacity: 0.5;
  }
</style>
