<template>
    <div class="comtainer">
        <div class="task-zone">
            <div class="drop-zone" @drop="onDrop($event, 'todo')" @dragenter.prevent @dragover.prevent>
                <h1>To-Do</h1> 
                <div class="drag-el" draggable @dragstart="onStart($evnt,task)" v-for="task in tasks" :key="task.id" >
                    {{ task.title}}
                </div>
            </div>
            <div class="drop-zone" @drop="onDrop($event, 'going')" @dragenter.prevent @dragover.prevent>
                <h1>Doing</h1>
                <div class="drag-el" draggable @dragstart="onStart($evnt,task)" v-for="task in tasks" :key="task.id" > 
                   {{ task.title}} 
                </div>
            </div>
            <div class="drop-zone" @drop="onDrop($event, 'done')" @dragenter.prevent @dragover.prevent>
                <h1>Done</h1>
                <div class="drag-el" draggable @dragstart="onStart($evnt,task)" v-for="task in tasks" :key="task.id" >
                    {{ task.title}} 
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
            ]
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
</style>