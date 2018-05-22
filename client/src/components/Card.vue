<template>
  <div class="panel panel-default">
    <div class="panel-heading">
      <h3 class="panel-title">{{ task.title }}</h3>
    </div>
    <div class="panel-body">
      <strong> Points:</strong> {{ task.points }} <br />
      <strong> Assigned To:</strong> {{ task.assigned_to }} <br />
      <strong> Description:</strong> {{ task.description }}
    </div>
    <div class="panel-footer">
      <button class="btn btn-danger" @click="deleteTask">Delete Task</button>
      <div class="pull-right">
        <button v-if="task.status" class="btn btn-danger" @click="moveLeft">&laquo;</button>
        <button v-if="task.status < 3" class="btn btn-success" @click="moveRight">&raquo;</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['task'],
    name: 'Card',
    methods: {
      deleteTask () {
        this.$emit('deleteTask', this.task)
      },
      moveLeft () {
        this.$emit('moveLeft', this.task)
      },
      moveRight () {
        this.$emit('moveRight', this.task)
      }
    }
  }
</script>

<style></style>
