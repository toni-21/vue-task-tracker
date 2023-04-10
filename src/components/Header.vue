<template>
    <header>
        <h1>{{ title }}</h1>
        <ButtonComponent v-show=isHomePage @toggle-add-task="$emit('toggle-add-task')"
            :text="showAddTask ? 'Close' : 'Add task'" :color="showAddTask ? 'red' : 'green'" />
    </header>
</template>

<script>
import ButtonComponent from './Button.vue';
export default {
    name: 'HeaderComponent',
    components: { ButtonComponent },
    props: {
        title: String,
        showAddTask: Boolean
    },
    computed: {
        isHomePage() {
            if (this.$route.path === '/') {
                return true
            } else {
                return false
            }
        }
    }
}
</script>
<style scoped>
header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 20px;
}
</style>