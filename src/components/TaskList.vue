<template>
    <div class="comtainer">
        <div class="add-task">
            <input id="new-task" type="text" v-model="newTask">
            <button type="button" @click="addTask(newTask)" >Add New Task</button>
        </div>
        <div class="task-zone">
            <div class="drop-zone" @drop="onDrop($event, 'todo')" @dragenter.prevent @dragover.prevent>
                <h1>To-Do</h1> 
                <div class="drag-el" draggable @dragstart="onStart($evnt,task)" v-for="task in tasks" :key="task.id" >
                    <!--{{ task.title}} -->
                    <span v-if="editTask != task.id">{{ task.title}}</span>
                    <input v-else class="edit-task" type="text" v-model="task.title">
                    <br>
                    <button v-if="editTask != task.id" type="button" @click="onEdit(task)">Edit</button>
                    <button v-else type="button" @click="editedTask(task)">Save</button>
                    <button type="button" @click="deleteTask(task)">Delete</button>
                </div>
            </div>
            <div class="drop-zone" @drop="onDrop($event, 'going')" @dragenter.prevent @dragover.prevent>
                <h1>Doing</h1>
                <div class="drag-el" draggable @dragstart="onStart($evnt,task)" v-for="task in tasks" :key="task.id" > 
                   <span v-if="editTask != task.id">{{ task.title}}</span>
                    <input v-else class="edit-task" type="text" v-model="task.title">
                    <br>
                    <button v-if="editTask != task.id" type="button" @click="onEdit(task)">Edit</button>
                    <button v-else type="button" @click="editedTask(task)">Save</button>
                    <button type="button" @click="deleteTask(task)">Delete</button>
                </div>
            </div>
            <div class="drop-zone" @drop="onDrop($event, 'done')" @dragenter.prevent @dragover.prevent>
                <h1>Done</h1>
                <div class="drag-el" draggable @dragstart="onStart($evnt,task)" v-for="task in tasks" :key="task.id" >
                    <span v-if="editTask != task.id">{{ task.title}}</span>
                    <input v-else class="edit-task" type="text" v-model="task.title">
                    <br>
                    <button v-if="editTask != task.id" type="button" @click="onEdit(task)">Edit</button>
                    <button v-else type="button" @click="editedTask(task)">Save</button>
                    <button type="button" @click="deleteTask(task)">Delete</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default{
    name: 'TaskList',
    data(){
        return{
            tasks:[
                {
                    id: 1,
                    title: 'Item A',
                    status: 'todo'
                },
                {
                    id: 2,
                    title: 'Item B',
                    status: 'todo'
                },
                {
                    id: 3,
                    title: 'Item C',
                    status: 'todo'
                },
                {
                    id: 4,
                    title: 'Item D',
                    status: 'todo'
                }
            ],
            newTask:"",
            editTask:""
        }
    },
    computed:{
        todoList(){
            return this.tasks.filter(tasks => tasks.status =="todo")
        },
        doingList(){
            return this.tasks.filter(tasks => tasks.status =="doing")
        },
        doneList(){
            return this.tasks.filter(tasks => tasks.status =="done")
        }
    },
    methods:{
        onStart(e,task){
            e.dataTransfer.dropEffect = "move"
            e.dataTransfer.effectAllowed = "move"
            e.dataTransfer.setData('taskId',task.id)
        },
        onDrop(e,newStatus){
            const taskId = e.dataTransfer.getData('taskId')
            const task = this.tasks.find(task =>task.id == taskId)
            task.status = newStatus
        },
        addTask(newTask){
            let newId = this.tasks.length + 1
            this.tasks.push({ id: newId, title: newTask, status: 'todo' })
            this.newTask = ""
        },
        onEdit(task){
            this.editTask = task.id
        },
        editedTask(updateTask){
            const task = this.tasks.find(task => task.id == updateTask.id)
            task.title = updateTask.title
            this.editTask=""
        },
        deleteTask(deleteTask){
            this.tasks = this.tasks.filter(task => task.id != deleteTask.id)
            
        }
    }
}
</script>

<style scoped>
.container{
    margin: 30px 0;
}
.task-zone{
    display: flex;
    justify-content: space-around;
}
.drop-zone{
    border: 1px solid black;
    width: 250px;
    height: 400px;
    padding: 10px 0;
}
.drag-el{
    border: 1px solid black;
    width: 200px;
    height: 40px;
    margin: 5px auto;
    padding-top: 15px;
}
.add-task{
    margin: 30px 0;
}
</style>