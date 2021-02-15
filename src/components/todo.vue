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
    export default {
        data () {
            return {
                tasks :  [
                    {name : 'create skeleton of todo', complete : true},
                    {name : 'add ability to add tasks', complete : true},
                    {name : 'clear task name after clicking "Add"', complete : false},
                    {name : 'put "Add" button in one line with input', complete : false},
                    {name : 'add new task by hitting Enter instead of clicking "Add"', complete : false},
                    {name : 'replace <input> with <ui-checkbox> in tasks list', complete : false},
                    {name : 'when task is complete cross it out', complete : false},
                    {name : 'split tasks into "pending" and "complete" tabs using keen-ui component <ui-tabs>', complete : false},
                    {name : 'don\'t allow to add empty tasks', complete : false},
                    {name : 'make list of tasks scrollable, if there\'re are a lot of tasks', complete : false},
                    {name : 'extract list item into a separate vue.js component', complete : false},
                    {name : 'persist tasks list in a local storage', complete : false},
                    {name : 'add animation on task completion', complete : false},
                ],
                newTaskName : ''
            }
        },
        computed:{
        },
        mounted () {
            if (localStorage.tasks === undefined){
                localStorage.setItem("tasks", JSON.stringify(this.tasks));
            }
            else {
                console.log(JSON.parse(localStorage.getItem("tasks")))
                this.tasks = JSON.parse(localStorage.getItem("tasks"));
            }
        },
        watch:{
          tasks(new_tasks){
              localStorage.setItem("tasks", JSON.stringify(this.tasks));
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
            max-height: 15em; /* 1.5 x 10 */
            overflow-y: auto;
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
