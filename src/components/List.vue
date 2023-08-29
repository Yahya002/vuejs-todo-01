<template>

  <div class="list">
    <TaskItem 
    v-for="task in list" 
    :task="task"
    @taskEdited="(id) => toggleCreateEdit(id)"
    @taskDeleted="(id) => deleteTask(id)"
    />
  </div>

  <div v-if="showCreateEdit">
    <CreateEdit
    :task="preset_task"
    @closeCreateEdit="toggleCreateEdit"
    @taskCreated="addTask"
    />
  </div>

  <div @click="toggleCreateEdit(0)" class="add-button">+ New Task</div>

</template>

<script>
import { onMounted, ref, reactive } from 'vue';
import TaskItem from './TaskItem.vue';
import CreateEdit from './CreateEdit.vue';

export class Task {
  id = 0;
  title = '';
  is_completed = false;
}

export default{
  name: 'List',
  setup(){
    const list = ref([]);
    const id_ctr = ref(0);

    const showCreateEdit = ref(false);
    const preset_task = reactive({id: 0, title: '', is_completed: false});

    function toggleCreateEdit(id = 0){

      /*
        there seems to be an issue because the mouse event is being passed as an argument.
        figure that out and then make sure the right props are being passed to the task item.
      */

      if (id != 0){
        preset_task.value = list.value.find((object) => object.id = id);
        console.log(id);
      }
      else {
        // figure out how to reset the preset_task without breaking reactivity.
      }
      // console.log("id is:", id, preset_task);
      showCreateEdit.value = !showCreateEdit.value;
    }

    function addTask(task){
      if (task.id == 0){
        id_ctr.value++;
        task.id = id_ctr.value;
        list.value.push(task);
      }
    }
    
    function deleteTask(id){
      let i = list.value.findIndex(object => object.id === id);
      list.value.splice(i, 1);
    }
    
    onMounted(() => {})

    return {
      list,
      showCreateEdit,
      toggleCreateEdit,
      preset_task,
      addTask,
      deleteTask
    }
  },
  components: {
    TaskItem,
    CreateEdit
  }
}
</script>

<style>

.list {
  
  max-width: 90%;
  margin: 1em auto;
  padding: 0.5em 0;

  overflow: scroll;
  overflow-x: hidden;

  background-color: aliceblue;
  border-radius: 2px;
}

.add-button{
  z-index: 1;

  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translate(-50%, -25%);
  padding: 0.5em 0.5em;
  text-align: center;
  
  color:white;
  background-color: #8e81f3;
  border-radius: 30px;
}
</style>
