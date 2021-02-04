<template>
    <div v-if="list.length">
        <h2>Suas tarefas:</h2>
        <template>
            <transition-group name="slide-fade">
                <task 
                    v-for="(list, i) in list" 
                    :key="list.description" 
                    :list="list" 
                    @taskDeleted="$emit('taskDeleted', i)"
                    @taskChanged="$emit('taskChanged', i)"
                > {{ list.description }} </task>
            </transition-group>
        </template>
    </div>
</template>

<script>
import Task from './Task.vue'

export default {
    props: {
        list: { type: Array, required: true }
    },
    components: { Task }
}
</script>

<style>
    .slide-fade-enter-active {
        transition: all .7s ease;
    }
    .slide-fade-leave-active {
        transition: all .2s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    }
    .slide-fade-enter, .slide-fade-leave-to {
        transform: translateX(10px);
        opacity: 0;
    }
</style>