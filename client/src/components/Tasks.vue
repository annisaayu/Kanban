<template>
<div>
  <div v-bind:class="util" style="width: 18rem;">
    <div class="card-body">
      <h5 class="card-title">{{ task.title }}</h5>
      <p class="card-text">{{ task.description }}</p>
      <p class="card-text"><b>{{ task.point }}</b> points</p>
      <p class="card-text">in charge: <b>{{ task.assigned }}</b></p>
      <a v-on:click="goLeft" class="card-link">
        <i class="fa fa-angle-double-left grow"></i>
      </a>
      <a v-on:click="goRight" class="card-link">
        <i class="fa fa-angle-double-right grow"></i>
      </a>
      <a class="card-link" data-toggle="modal" :data-target="dataTarget">
        <i class="fa fa-edit grow"></i>
      </a>
      <a v-on:click="remove" class="card-link">
        <i class="fa fa-remove grow"></i>
      </a>
    </div>
  </div>
    <!-- update task modals -->
  <div class="modal fade" tabindex="-1" role="dialog" v-bind:id="modalId">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">Update Task</h5>
          <button type="button" class="close" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body">
            <div class="form-group">
              <input type="text" v-model="updatedTask.title" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="Enter Task Title">
            </div>
          <div class="form-group">
            <input type="text" v-model="updatedTask.description" class="form-control" placeholder="Task Description">
          </div>
          <div class="form-group">
            <input type="number" v-model="updatedTask.point" class="form-control" placeholder="point" min="0">
          </div>
          <div class="form-group">
            <input type="text" v-model="updatedTask.assigned" class="form-control" placeholder="Assigned to">
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-primary" v-on:click="updateTask" data-dismiss="modal">Update</button>
          <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'
import * as firebase from 'firebase'

export default {
  props: ['task', 'util'],
  data () {
    return {
      modalId: this.task['.key'],
      dataTarget: '',
      updatedTask: {
        title: this.task.title,
        description: this.task.description,
        point: this.task.point,
        assigned: this.task.assigned
      }
    }
  },
  methods: {
    remove: function () {
      tasksRef.child(this.task['.key']).remove()
    },
    updateTask: function () {
      alert('task updated !')
      tasksRef.child(this.task['.key']).update(this.updatedTask)
    },
    goLeft: function () {
      if(this.task.status !== 0) {
        tasksRef.child(this.task['.key']).update({
        status: this.task.status - 1
        })
      }
    },
    goRight: function () {
      if (this.task.status !== 3) {
        tasksRef.child(this.task['.key']).update({
        status: this.task.status + 1
        })
      }
    }
  },
  created () {
    this.dataTarget = '#'+this.task['.key']
  }
}
</script>

<style>
.grow {
  -webkit-transition:all 0.5s ease-out;
  -moz-transition:all 0.5s ease-out;
  -ms-transition:all 0.5s ease-out;
  -o-transition:all 0.5s ease-out;
  transition:all 0.5s ease-out;
}
.grow:hover {
  -webkit-transform:scale(1.3);
  -moz-transform:scale(1.3);
  -ms-transform:scale(1.3);
  -o-transform:scale(1.3);
  transform:scale(1.3);
}
.size {
 max-width: 300px;
 max-height: 300px;
}
</style>
