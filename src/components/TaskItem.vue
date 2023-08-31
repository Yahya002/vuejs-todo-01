<template>

    <div class="taskCard" @mouseenter="toggleShowOptions" @mouseleave="toggleShowOptions">
        <i v-if="!task.is_completed" class="fa-solid fa-checkmark" />
        <h3 class="taskTitle">{{ task.title }}</h3>
        <div v-if="showOptions">
            <i class="fa-solid fa-pen icon" @click="edit"></i><i class="fa-solid fa-xmark icon"  @click="remove"></i>
        </div>
    </div>

</template>

<script>
import { ref } from 'vue';

export default{
    name: 'TaskItem',
    setup(props, { emit }){

        const showOptions = ref(false);

        function toggleShowOptions(){
            showOptions.value = !showOptions.value
        }

        function edit(){
            emit('taskEdited', props.task.id)
        }
        function remove(){  
            emit('taskDeleted', props.task.id)
        }

        return {
            edit,
            remove,
            showOptions,
            toggleShowOptions,
        }
    },
    props: ['task']

}
</script>

<style>
.taskCard {
    
    max-width: 90%;
    margin: 0.3em auto;

    border-top: 3px solid #8e81f3;
    border-radius: 1px;
    
}

.taskTitle {    

    font-size: x-large;

    text-align: left;
    margin: 0 0.5em;

    overflow: hidden;

}

.icon {

    margin-left: 1em;
    
}
</style>