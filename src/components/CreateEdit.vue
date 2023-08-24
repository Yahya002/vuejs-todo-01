<template>
<div class="backdrop" @click.self="close">
    <div class="modal">
        {{ task.title }}
        <input 
        class="task-input" 
        placeholder="enter task" 
        v-bind:value="task.title"
        @input="event => task.title = event.target.value"
        >
        <button @click="save">save</button>
    </div>
</div>

</template>

<!-- 
    implement autofocus for input
    implement "enter" to save tasks
-->

<script>
import { onMounted, reactive } from 'vue';

export default {
    setup(props, { emit }){
        const task = reactive({
            id: props.id,
            title: props.title
        })

        function save(){
            emit('taskCreated', task); // distinguish this from the edit
            emit('closeCreateEdit');
        }

        function close(){
            emit('closeCreateEdit');
        }

        onMounted(() => {
            console.log(props.id, props.title, task.id, task.title);
        })

        return {
            save,
            close,
            task
        }
    },

    props: {
        id: Number,
        title: String,
    },
}
</script>

<style>

.backdrop{

    position: fixed;
    z-index: 2;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;

    background-color: rgb(0, 0, 0); /* fallback colour */
    background-color: rgba(0, 0, 0, 0.4);
}

.modal {
    
    margin: 15% auto;
    padding: 20px;
    width: 60%;

    border-radius: 10px;
    color: white;
    background-color: #8e81f3;
}

.task-input {

    border: none;
    border-bottom: 1px solid #756ac7;
    background-color: #756ac7;
    color: white;
    border-radius: 5px;
    margin: 0 auto;
}
.task-input:focus {

    background-color: #8e81f3;
    border-bottom-width: 3px;   
    outline: none;
    color: white;
    border-radius: 2px;
}

</style>