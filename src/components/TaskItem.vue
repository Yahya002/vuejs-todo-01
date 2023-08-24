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
        
        // defineProps({
        //     task: Object,
        // })

        const showOptions = ref(false);

        function toggleShowOptions(){
            showOptions.value = !showOptions.value
        }

        function edit(){
            console.log("should emit edit")
            // emit('taskEdited', this.id)
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
    margin: 0.5em auto;

    border: 2px solid #8e81f3;
    border-radius: 5px;
}

.taskTitle {

    text-align: left;
    margin: 0.5em 1em;

    overflow: hidden;
}

.icon {
    margin-left: 1em;
}
</style>