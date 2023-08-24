<template>

    <div class="taskCard" @mouseenter="toggleShowOptions" @mouseleave="toggleShowOptions">
        <h3 class="taskTitle">{{ title }}</h3>
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
            emit('taskEdited', this.id)
        }
        function remove(){
            emit('taskDeleted', this.id)
        }

        return {
            edit,
            remove,
            showOptions,
            toggleShowOptions
        }
    },
    props: {
        id: Number,
        title: String,
    }
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