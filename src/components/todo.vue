<template>
    <div class="todo">
        <h1 class="title">Checklist</h1>
        <ui-tabs>
            <ui-tab title="Pending">
                <ul class="tasks">
                    <li v-for="task in tasks.filter(task_find => task_find.complete == false)" :class="{complete : task.complete}">
                        <label>
                            <ui-checkbox type="checkbox" v-model="task.complete"  @change="check($event)">
                                <a>{{task.name}}</a>
                            </ui-checkbox>
                        </label>
                    </li>
                </ul>
            </ui-tab>
            <ui-tab title="Complete">
                <ul class="tasks">
                    <li v-for="task in tasks.filter(task_find => task_find.complete == true)" :class="{complete : task.complete}">
                        <label>
                            <ui-checkbox type="checkbox" v-model="task.complete" >
                                <strike> {{task.name}}</strike>
                            </ui-checkbox>
                        </label>
                    </li>
                </ul>
            </ui-tab>
        </ui-tabs>
        <div class="container">
            <ui-textbox class="input-text" v-on:keydown="EnterkeyPress"  placeholder="Add item here" v-model="newTaskName" v-on:keyup.enter="addTask"></ui-textbox>
            <ui-button class="button-add" color="primary" v-on:click="addTask" icon="add">Add</ui-button>
        </div>
    </div>
</template>

<script>
import tasks from './tasks'
    export default {
        data () {
            return {
                newTaskName : tasks.data().newTaskName,
                tasks : tasks.data().tasks
            }
        },
        computed:{
        },
        mounted () {
            if (localStorage.tasks === undefined){
                localStorage.tasks = this.tasks
            }
        },
        watch:{
          tasks(new_tasks){
              localStorage.tasks = new_tasks
          }
        },
        methods : {
            addTask () {
                if (this.newTaskName.length > 0){
                    this.tasks.push({name : this.newTaskName, complete : false});
                    this.newTaskName = '';
                }
            },
            EnterkeyPress (evt){
                evt = evt ? evt : window.event;
                var charCode = evt.which ? evt.which : evt.keyCode;
                if (charCode===13){
                   this.addTask()
                }
            },
            async check(event){
                ///animation after click that task is done
            }
        }
    };
</script>

<style scoped lang="scss">
    .todo {
        margin: auto;
        background: #fff;
        padding: 20px;
        border-radius: 5px;
        box-shadow: rgba(0, 0, 0, 0.3) 3px 3px 15px;

        .title {
            margin-top: 0;
        }

        .tasks {
            list-style: none;
            padding: 0;
            overflow: scroll;
            overflow-x: hidden;
        }
        .container {
            display: inline-flex;
            min-width: 100%;
        }
        .button-add {
            margin-right: 20%;
        }
        .input-text {
            min-width: 70%;
        }
    }
</style>
