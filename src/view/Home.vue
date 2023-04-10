<template>
    <AddTaskComponent v-show=showAddTask @add-task="addTask" />
    <div :key="task.id" v-for="task in tasks">
        <TaskComponent @delete-task=deleteTask @toogle-reminder=toggleReminder :task=task />
    </div>
</template>

<script>
import TaskComponent from '../components/Task.vue';
import AddTaskComponent from '../components/AddTask.vue';


export default {
    name: 'HomePage',
    components: {
        TaskComponent,
        AddTaskComponent,
    },
    props: {
        showAddTask: Boolean
    },
    data() {
        return {
            tasks: [],
        }
    },
    methods: {
        async addTask(newTask) {
            const res = await fetch('api/tasks', {
                method: 'POST',
                body: JSON.stringify(newTask),
                headers: {
                    'Content-type': 'application/json',
                }
            });
            const data = await res.json();

            this.tasks.push(data);
        },
        async deleteTask(id) {
            if (confirm('Are you sure?')) {
                const res = await fetch(`api/tasks/${id}`, {
                    method: 'DELETE',
                });
                res.status === 200 ? (
                    this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error occcured deleting task');
            }
        },
        async toggleReminder(id) {
            const index = this.tasks.findIndex((task) => task.id === id);
            const selectedTask = this.tasks[index];
            const updatedTask = { ...selectedTask, reminder: !selectedTask.reminder };
            const res = await fetch(`api/tasks/${id}`, {
                method: 'PUT',
                body: JSON.stringify(updatedTask),
                headers: {
                    'Content-type': 'application/json',
                }
            });
            const data = await res.json();

            this.tasks = this.tasks.map((task) => task.id === id ? {
                ...task, reminder: data.reminder
            } : task);
        },
        async fetchTasks() {
            const request = await fetch('api/tasks');
            const response = await request.json();
            return response;
        },
    },
    async created() {
        this.tasks = await this.fetchTasks();
    }
}
</script>