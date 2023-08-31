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

      if (id != 0){

        let t = list.value.find(object => object.id === id);
        preset_task.id = t.id;
        preset_task.title = t.title;
        preset_task.is_completed = t.is_completed;

      }
      else {

        // is there a better way to write this? review 'reactive'
        preset_task.id = 0;
        preset_task.title = '';
        preset_task.is_completed = false;

      }

      showCreateEdit.value = !showCreateEdit.value;
    }

    function addTask(task){
      if (task.id == 0){
        id_ctr.value++;
        task.id = id_ctr.value;
        list.value.push(task);
      } else {
        list.value.find(object => object.id === task.id).title = task.title;
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

  overflow: hidden; /* Hide scrollbars */

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
