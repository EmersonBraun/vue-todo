<template>
    <div class="task-grid">
        <template v-if="tasks.length">
            <div class="row task">
                <Task v-for="(task, i) in tasks" 
                    :key="task.name" 
                    :task="task" 
                    @taskDeleted="$emit('taskDeleted', i)"
                    @taskStateChanged="$emit('taskStateChanged', i)" 
                />
            </div>
        </template>
        <p v-else class="no-task">Your life is up to date !!!</p>
    </div>
</template>

<script>
import Task from './Task.vue'
export default {
    components: { Task },
    props: {
        tasks: { type: Array, required: true }
    }
}
</script>

<style scoped>
.task {
    gap: 12px;
    padding: 0 16px;
}
.no-task {
    color: #ddd;
    font-size: 1.5rem;
    text-align: center;
}
</style>
