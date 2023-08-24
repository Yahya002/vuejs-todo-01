<template>
  <div class="list">
    <TaskItem 
    v-for="task in list" 
    :id="task.id" 
    :title="task.title"
    @taskEdited="(id) => toggleCreateEdit(id)"
    />
  </div>
  <div v-if="showCreateEdit">
    <CreateEdit
    :id="preset_task.id"
    :title="preset_task.title" 
    @closeCreateEdit="toggleCreateEdit"
    @taskCreated="addTask"
    />
  </div>
  <div @click="toggleCreateEdit(0)" class="add-button">+ New Task</div>
</template>

<script>
import { onMounted, ref } from 'vue';
import axios from 'axios';
import TaskItem from './TaskItem.vue';
import CreateEdit from './CreateEdit.vue';

export default{
  name: 'List',
  setup(){
    const list = ref([]);
    const id_ctr = ref(0);

    const showCreateEdit = ref(false);
    const preset_task = {id: 0, title: ''};

    function toggleCreateEdit(id = 0){

      /*
        there seems to be an issue because the mouse event is being passed as an argument.
        figure that out and then make sure the right props are being passed to the task item.
      */
      if (id != 0){
        preset_task.value = list.value.find((object) => object.id = id);
        console.log("id is set:", id, preset_task);
      }
      else {
        preset_task.value = {id: 0, title: ''};
      }
      showCreateEdit.value = !showCreateEdit.value;
    }

    function addTask(task){
      if (task.id == 0){
        id_ctr.value++;
        task.id = id_ctr.value;
      }
      list.value.push(task);
    }

    function getTasks(){
      axios.get('https://jsonplaceholder.typicode.com/posts')
        .then((response) => {
          console.log(response.data)
        })
        .catch((error) => {
          console.log(error)
        })
    }
 
    onMounted(() => {
      // getTasks()
      // console.log(list);
      // console.log(list.value.find);
    })

    return {
      list,
      showCreateEdit,
      toggleCreateEdit,
      preset_task,
      addTask,
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
