<template>
    <section class="need">
        <section class="hood">
            <section class="select">
                <span>Сортировать по</span>
                <select v-model="select">
                    <option :value="1"> Все</option>
                    <option :value="2">Не исполненые</option>
                    <option :value="3">Исполненые</option>
                </select>
            </section>
<!--            Мне искренне стыдно за этот кусок кода, но как делать по другому я не знаю                           -->
            <section v-for="task in tasks"
                     v-bind:key="task.id">
                <article v-if="select === 1">
                    <ToDoItem
                            :task="task"
                            @viewDetails="view"
                            @deleteTask="deleteTaskSwap">
                    </ToDoItem>
                </article>

                <article v-if="select === 2">
                    <ToDoItem
                            v-if="task.all > task.success || task.all === 0"
                            :task="task"
                            @viewDetails="view"
                            @deleteTask="deleteTaskSwap">

                    </ToDoItem>
                </article>
                <article v-if="select === 3">
                    <ToDoItem
                            v-if="task.all === task.success && task.all !== 0"
                            :task="task"
                            @viewDetails="view"
                            @deleteTask="deleteTaskSwap">

                    </ToDoItem>
                </article>
<!--                                                                                                                 -->
            </section>
        </section>
        <section class="create">
            <input type="text" v-model="newTask" placeholder="Название списка">
            <input type="button" value="Создать новый список дел" @click="createTaskSwap()">
        </section>
        <!--        создание задания        -->
        <section class="Modal" v-if="this.QuestInformation[0]">
            <article class="CreateQuest">
                <p>Вы уверены что хотите добавить </p>
                <p>"{{this.QuestInformation[0]}}"</p>
                <p> в список заданий?</p>
                <article class="row">
                    <input class="btn-success" type="button" value="Да" @click="NewQuest">
                    <input class="btn-danger" type="button" value="Нет" @click="Cancel">
                </article>
            </article>
        </section>

        <!--        создание списка дел-->
        <section class="Modal" v-if="createTask">
            <article class="CreateTask">
                <p>Вы уверены что хотите добавить</p>
                <p>"{{this.newTask}}"</p>
                <p>в список дел?</p>
                <article class="row">
                    <input class="btn-success" type="button" value="Да" @click="createNewTask">
                    <input class="btn-danger" type="button" value="Нет" @click="cancelNewTask">
                </article>
            </article>
        </section>
        <!--        удаление списка дел-->
        <section class="Modal" v-if="deleteTask">
            <article class="DeleteTask">
                <p>Вы уверены что хотите удалить</p>
                <p>"{{this.deleteTaskName}}"</p>
                <p>из списка дел?</p>
                <article class="row">
                    <input class="btn-success" type="button" value="Да" @click="deleteTaskList">
                    <input class="btn-danger" type="button" value="Нет" @click="deleteTaskSwap">
                </article>
            </article>
        </section>
        <!--        удаление задания-->
        <section class="Modal" v-if="deleteQuestInformation[0]">
            <article class="DeleteQuest">
                <p>Вы уверены что хотите удалить</p>
                <p>"{{this.deleteQuestInformation[1]}}"</p>
                <p>из списка заданий?</p>
                <article class="row">
                    <input class="btn-success" type="button" value="Да" @click="deleteQuest">
                    <input class="btn-danger" type="button" value="Нет" @click="cancelDeleteQuest">
                </article>
            </article>
        </section>
        <!--        Пометить квест как выполненый-->
        <section class="Modal" v-if="markQuestId">
            <article class="MarkQuest">
                <h4>Вы уверены?</h4>
                <article class="row">
                    <input class="btn-success" type="button" value="Да" @click="mark">
                    <input class="btn-danger" type="button" value="Нет" @click="cancelMark">
                </article>
            </article>
        </section>
    </section>
</template>

<script>
    import ToDoItem from './ToDoItem.vue';

    export default {
        name: 'ToDoList',
        props: ['QuestInformation', "SelectedTask", "deleteQuestInformation", "markQuestId"],
        data: function () {
            return {
                select: 1,
                createTask: false,
                deleteTask: false,
                questIdCounter: 5,
                deleteTaskName: '',
                deleteId: '',
                newTask: '',
                counter: 4,
                tasks: [
                    {
                        TaskName: 'Выполнить это задание',
                        id: 1,
                        date: '1-10-2020',
                        quests: [
                            {
                                questId: 1,
                                urgency: false,
                                success: true,
                                quest: 'оно вроде работает',
                                time: '10:10',
                            },
                            {
                                questId: 2,
                                urgency: false,
                                success: true,
                                quest: 'но без костылей не обошлось',
                                time: '12:10',
                            },

                        ],
                        success: 2,
                        all: 2,
                    },
                    {
                        TaskName: 'Для примера',
                        id: 2,
                        date: '1-10-2022',
                        quests: [
                            {
                                questId: 3,
                                urgency: true,
                                success: false,
                                quest: 'Покормить кота',
                                time: '23:30',
                            },
                            {
                                questId: 4,
                                urgency: false,
                                success: false,
                                quest: 'Покормить себя',
                                time: '23:40',
                            },

                        ],
                        success: 0,
                        all: 2,
                    },
                    {
                        TaskName: 'Тест',
                        id: 3,
                        date: '1-10-2023',
                        quests: [],
                        success: 0,
                        all: 0,
                    }
                ]
            }
        },
        components: {
            ToDoItem,
        },
        methods: {
            //пометить задние как выполненое
            mark() {
                for (let i = 0; i < this.tasks.length; i++) {
                    for (let j = 0; j < this.tasks[i].quests.length; j++) {
                        if (this.tasks[i].quests[j].questId === this.markQuestId) {
                            this.tasks[i].quests[j].success = true;
                            this.tasks[i].success += 1;
                        }
                    }
                }
                this.cancelMark();
            },

            cancelMark() {
                this.$emit('cancelMark');
            },

            // Показать / скрыть модальное окно по удалению квеста
            deleteTaskSwap(arr) {
                this.deleteTask = !this.deleteTask;
                this.deleteId = arr[0];
                this.deleteTaskName = arr[1];
            },
            // Показать / скрыть модальное окно по созданию квеста
            createTaskSwap() {
                if (this.newTask.length > 0) {
                    this.createTask = !this.createTask;
                }
            },
            // Удаление задания
            deleteQuest() {
                for (let i = 0; i < this.tasks.length; i++) {
                    for (let j = 0; j < this.tasks[i].all; j++) {
                        if (this.tasks[i].quests[j].questId === this.deleteQuestInformation[0]) {
                            if (this.tasks[i].quests[j].success === true) {
                                this.tasks[i].success--;
                            }
                            this.tasks[i].quests.splice(j, 1);
                            this.tasks[i].all--;
                        }
                    }
                }
                this.cancelDeleteQuest();
            },

            // Отмена удаления квеста
            cancelDeleteQuest() {
                this.$emit('cancelDelete');
            },

            // Удаление списка
            deleteTaskList() {
                for (let i = 0; i < this.tasks.length; i++) {
                    if (this.tasks[i].id === this.deleteId) {
                        this.tasks.splice(i, 1);
                    }
                }
                this.deleteTask = !this.deleteTask;
                if (this.SelectedTask.id === this.deleteId) {
                    this.$emit('deleteRequest');
                }
            },

            // Создание нового списка
            createNewTask() {
                let d = new Date();
                let curr_date = d.getDate();
                let curr_month = d.getMonth() + 1;
                let curr_year = d.getFullYear();
                d = curr_date + "-" + curr_month + "-" + curr_year;

                this.tasks.unshift({
                    TaskName: this.newTask,
                    id: this.counter,
                    date: d,
                    quests: [],
                    success: 0,
                    all: 0
                });
                this.counter++;
                this.createTaskSwap();
                this.newTask = '';
            },

            // Отмена создания нового списка
            cancelNewTask() {
                this.createTaskSwap();
                this.newTask = '';
            },

            // Просмотр деталей выбраного списка
            view(id) {
                for (let i = 0; i < this.tasks.length; i++) {
                    if (this.tasks[i].id === id) {
                        let taskToView = this.tasks[i];
                        this.$emit('request', taskToView);
                    }
                }

            },

            // Создание нового квеста
            NewQuest() {
                let d = new Date();
                let curr_hours = d.getHours();
                let curr_minutes = d.getMinutes() + 1;
                if (curr_hours <= 9) {
                    curr_hours = '0' + curr_hours;
                }
                if (curr_minutes <= 9) {
                    curr_minutes = '0' + curr_minutes;
                }

                for (let i = 0; i < this.tasks.length; i++) {
                    if (this.tasks[i].id === this.QuestInformation[1]) {
                        this.tasks[i].all += 1;
                        this.tasks[i].quests.unshift({
                            questId: this.questIdCounter,
                            urgency: this.QuestInformation[2],
                            success: false,
                            quest: this.QuestInformation[0],
                            time: curr_hours + ":" + curr_minutes,
                        });
                    }
                }
                this.questIdCounter++;
                this.Cancel();

            },
            Cancel() {
                this.$emit('Clear', this.QuestInformation);
            },
        },
    };
</script>


<style scoped>
    .Modal {
        z-index: 99999;
        position: absolute;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background: rgba(0, 0, 0, 0.78);
    }

    .CreateQuest, .DeleteTask, .CreateTask, .DeleteQuest, .MarkQuest {
        position: absolute;
        top: 30vh;
        left: calc(50% - 350px);
        width: 700px;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        border-radius: 3px;
        font-size: 20px;
        background: whitesmoke;
        border: 1px solid #000000;
        box-shadow: 0 0 10px grey;
        padding: 10px;
        text-align: center;
        animation: show linear .6s;
    }

    .CreateQuest h2 {
        margin-top: 20px;
        margin-bottom: 40px;
    }

    .need {
        height: 100vh;
    }

    .hood {
        width: 25%;
        height: 85%;
        overflow: auto;
        float: left;
        background: #edeef0;
        border-right: 1px solid #edeef0;
    }

    .create {
        clear: both;
        height: 15vh;
        width: 25%;
        display: flex;
        flex-direction: column;
    }

    .create input[type='text'] {
        height: 5vh;
        font-size: 20px;
        text-align: center;
        border: none;
        outline: none;
        border-top: 1px solid #B2B2B2;

    }

    .create input[type='button'] {
        width: 100%;
        height: 10vh;
        background: #28A745;
        border: 1px solid #28a745;
        color: whitesmoke;
        font-size: 20px;
        transition: .3s;
        font-weight: 600;
    }

    .create input[type='button']:hover {
        background: #23813d;
        border: 1px solid #23813d;
        transition: .3s;
    }

    .list span {
        margin-left: 10px;
    }

    input[type='checkbox'] {
        width: 30px;
        height: 30px;
    }

    h4 {
        padding: 20px;
    }

    .select {
        width: 100%;
        height: 100px;
        background: #455be2;
        margin-bottom: 10px;
        color: #edeef0;
        transition: .3s;
        font-size: 25px;
        display: flex;
        justify-content: space-between;
        flex-direction: column;
    }

    select {
        width: 100%;
        height: 50px;
        font-size: 20px;
        border: none;
        border-bottom: 1px solid #B2B2B2;
        outline: none;
    }

    option {
        outline: none;
    }

    .select span {
        padding: 5px;
    }

</style>
