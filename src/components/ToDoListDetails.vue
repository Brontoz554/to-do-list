<template>
    <section class="main">
        <article class="tasks">
            <h2>{{selectedTask.TaskName}}</h2>
            <section class="card" v-for="item in selectedTask.quests" :key="item.id">
                <div class="row">
                    <h3>{{item.quest}}</h3>
                    <span class="fast" v-if="item.urgency">СРОЧНО</span>
                </div>
                <div class="row">
                    <p v-if="!item.success" class="mark" @click="Mark(item.questId)">пометить как выполненое</p>
                    <p v-else>Успешно выполнено!</p>
                    <p>{{item.time}}</p>
                    <p class="delete" @click="DeleteQuest(item.questId, item.quest)">удалить</p>
                </div>
            </section>
        </article>
        <article class="createTask" v-show="selectedTask.TaskName">
                <input class="urgency" type="checkbox" v-model="urgency">
                <input type="text" placeholder="Название нового дела" v-model="NameNewQuest">
                <input type="button" value="Создать" @click="CreateNewQuest">
        </article>
    </section>
</template>

<script>
    export default {
        name: "tasks",
        props: ['selectedTask'],
        data: function () {
            return {
                NameNewQuest: '',
                urgency: '',
            };
        },
        methods:
            {
                //создание нового квеста
                CreateNewQuest() {
                    let arr = [this.NameNewQuest, this.selectedTask.id, this.urgency];
                    this.$emit('CreateNewQuest', arr);
                    this.NameNewQuest = '';
                    this.urgency = false;
                },
                //Удаление квеста
                DeleteQuest(id, quest) {
                    let arr = [id, quest];
                    this.$emit('DeleteQuest', arr);
                },
                //Пометить квест как выполненый
                Mark(id) {
                    this.$emit('MarkQuest', id);
                }
            },
    }
</script>
<style scoped>
    .main {
        position: absolute;
        top: 0;
        left: 25%;
        height: 100vh;
        width: 75vw;
        background: #fefefe;
    }

    .row {
        justify-content: space-between;
        padding: 30px;
    }

    .row:nth-of-type(2n) {
        background: #eeeeee;
        padding: 10px 30px;
        align-items: center;

    }

    h3 {
        width: 80%;
    }

    h2 {
        text-align: center;
        margin-top: 10px;
        margin-bottom: 5px;
    }

    .card {
        width: 60%;
        background: white;
        border-radius: 4px;
        margin: 30px auto;
        box-shadow: 0 0 4px #ccc;
        animation: show linear .3s;

    }

    .tasks {
        height: 85%;
        overflow: auto;
    }


    .card input[type='checkbox'] {
        width: 40px;
        height: calc(4vh + 5px);

    }

    .createTask input[type='text'] {
        width: 40%;
        height: 30%;
        font-size: 20px;
        padding-left: 10px;
        border-radius: 3px;
        border: 1px solid grey;

    }

    .createTask input[type='button'] {
        width: 20%;
        height: 30%;
        background: #293682;
        border: 1px solid #293682;
        color: whitesmoke;
        font-size: 20px;
        transition: .3s;
    }

    input[type='button']:hover {
        background: #31409c;
        border-color: #31409c;
        transition: .3s;
    }

    .urgency {
        height: 40px;
        width: 40px;
        box-shadow: 0 0 4px grey inset;

    }

    .createTask {
        border-radius: 3px;
        margin: 0 auto;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: row;
        height: 15vh;
        background: #edeef0;

    }

    .fast {
        color: #ff000f;
    }

    .mark {
        background: #249a20;
        cursor: pointer;
        padding: 5px 10px;
        border-radius: 5px 10px;
        color: white;
        transition: .3s;
    }

    .mark:hover {
        background: #2dcd29;
        transition: .3s;

    }

    .delete {
        background: firebrick;
        cursor: pointer;
        padding: 5px 10px;
        border-radius: 5px 10px;
        color: white;
        transition: .3s;
    }

    .delete:hover {
        background: #e3000e;
        transition: .3s;
    }
</style>