<template>
  <div class="row">

    <AddTask @createTask="createTask" />

    <Panel title="Back-Log" @deleteTask="deleteTask" @moveRight="moveRight" :tasks="backlog" />
    <Panel title="Todo" @deleteTask="deleteTask" @moveLeft="moveLeft" @moveRight="moveRight" :tasks="todo" />
    <Panel title="Doing" @deleteTask="deleteTask" @moveLeft="moveLeft" @moveRight="moveRight" :tasks="doing" />
    <Panel title="Done" @deleteTask="deleteTask" @moveLeft="moveLeft" @moveRight="moveRight" :tasks="done" />

  </div>
</template>

<script>
  import AddTask from '@/components/AddTask'
  import Panel from '@/components/Panel'
  import * as firebase from 'firebase'

  const config = {
    databaseURL: 'https://uikanban.firebaseio.com/',
    projectId: 'uikanban'
  }

  const firebaseApp = firebase.initializeApp(config)
  const db = firebaseApp.database()
  const tasksRef = db.ref('tasks')

  export default {
    components: {
      AddTask,
      Panel
    },
    firebase: {
      tasks: tasksRef
    },
    name: 'Boards',
    computed: {
      backlog () {
        return this.tasks.filter(task => task.status === 0);
      },
      todo () {
        return this.tasks.filter(task => task.status === 1);
      },
      doing () {
        return this.tasks.filter(task => task.status === 2);
      },
      done () {
        return this.tasks.filter(task => task.status === 3);
      }
    },
    methods: {
      createTask (newTask) {
        tasksRef.push(newTask);
        this.$swal('Saved!', 'Task successfully saved', 'success');
      },
      deleteTask (task) {
        tasksRef
          .child(task['.key'])
          .remove();
        this.$swal('Deleted!', 'Task successfully deleted', 'success');
      },
      moveLeft (task) {
        tasksRef
          .child(task['.key'])
          .child('status')
          .set(task['status'] - 1);
      },
      moveRight (task) {
        tasksRef
          .child(task['.key'])
          .child('status')
          .set(task['status'] + 1);
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
