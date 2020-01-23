<template>
    <section id="app">
        <ToDoList @Clear="Clear"
                  @deleteRequest="DeleteRequest"
                  @request="ShowTask"
                  @cancelDelete="cancelDelete"
                  @cancelMark="cancelMark"
                  :QuestInformation="QuestInformation"
                  :SelectedTask="selectedTask"
                  :deleteQuestInformation="deleteQuestInformation"
                  :markQuestId="markQuestId">

        </ToDoList>
        <ToDoListDetails
                @CreateNewQuest="CreateNewQuest"
                @DeleteQuest="DeleteQuest"
                @MarkQuest="MarkQuest"
                :selectedTask="selectedTask"
                :NameNewQuest="NameNewQuest"
        >
        </ToDoListDetails>
    </section>
</template>

<script>

    import ToDoList from './components/ToDoList.vue';
    import ToDoListDetails from './components/ToDoListDetails.vue';

    export default {
        name: 'app',

        components: {
            ToDoList,
            ToDoListDetails,

        },

        data: function () {
            return {
                selectedTask: [],
                NameNewQuest: '',
                urgency: false,
                QuestInformation: [],
                deleteQuestInformation: [],
                markQuestId: '',
            }
        },
        methods: {
            //просмотр заданий выбранного списка
            ShowTask(taskToView) {
                this.selectedTask = taskToView;
            },

            // создание нового задания
            CreateNewQuest(arr) {
                this.QuestInformation = arr;
            },
            // отчистка информации о квесте
            Clear() {
                this.QuestInformation = '';
            },
            //очистка информации о выбранном списке задания
            DeleteRequest() {
                this.selectedTask = [''];
            },
            // удаление задания
            DeleteQuest(arr) {
                this.deleteQuestInformation = arr;
            },
            // отмена удаления задания
            cancelDelete() {
                this.deleteQuestInformation = [];
            },
            // отметить квест как выполненый
            MarkQuest(id) {
                this.markQuestId = id;
            },
            // отмена метки
            cancelMark()
            {
                this.markQuestId = false;
            }
        },

    };

</script>