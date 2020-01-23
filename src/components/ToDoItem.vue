<template>
    <article class="card-body"
             v-bind:class="{noQuest:task.all === 0,inProcess: task.success < task.all,success : task.all > 0 && task.all === task.success}">
        <h3 class="name-text" @click="viewDetailsClicked">{{task.TaskName}}</h3>
        <p class="card-text">выполнено {{task.success}} из {{task.all}}</p>
        <div class="row">
            <p class="delete-text" @click="deleteTask(task.id, task.TaskName)">Удалить</p>
            <p class="date-text">{{task.date}}</p>
        </div>
    </article>
</template>

<script>
    export default {
        name: 'ToDoItem',
        props: ['task'],
        data: function () {
            return {};
        },

        methods: {
            //просмотр выбранного списка дел
            viewDetailsClicked() {
                this.$emit("viewDetails", this.task.id);
            },

            //удаление списка дел
            deleteTask(id, name) {
                let deleteArray = [id, name];
                this.$emit("deleteTask", deleteArray);
            },

        },

    }
</script>

<style scoped>
    .card-body {
        background: white;
        color: black;
        border-bottom: 1px solid #ccc;
        box-shadow: 0 2px 3px grey;
        width: calc(100% - 20px);
        font-size: 20px;
        padding-left: 10px;
        cursor: pointer;
        margin-bottom: 10px;
        animation: show linear .6s;
    }

    h3, p, div {
        padding: 10px 0 5px 0;
    }

    .row {
        justify-content: space-between;
    }

    .date-text {
        padding-right: 10px;
        color: #B2B2B2;
    }

    .delete-text {
        z-index: 99;
    }

</style>