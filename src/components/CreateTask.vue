<template lang='pug'>
.vue-create-task
    .container
        section.task-input.columns
            b-field.input-field.column.is-2.has-text-centered
                | {{ formItems[0].title }}
            b-input.column.is-10(v-if='formItems[0].type == "text"' v-model='todoInput'
                @keyup.native.enter='addTodo(todoInput)' placeholder='add your task' rounded)

        section.tasks.columns.is-multiline
            task-item.card.column.is-4(v-for='todo, i in todos' :key='i' :uniqueID='i' :todo='todo' :imgs='todoImgs'
                @editTask='editTask' @deleteTask='deleteTask' @moveTask='moveToOngoingTask')
</template>

<script lang='ts'>
import { Vue, Component, Prop } from 'vue-property-decorator';
import VueUtil from '@/scripts/util/VueUtil';
import { InputFormItemProps, taskItemProps } from '@/components/interface';



/**
 * Vue Component
 */
@Component
export default class CreateTask extends Vue {
    private todoInput: string = '';
    private uniqueID: number = 0;
    private todoImgs: Imgs = {
        right: require('@/resources/img/right.png'),
        trash: require('@/resources/img/trash-can.png'),
        begin: require('@/resources/img/right-arrow.png')
    }

    private todos: taskItemProps[] = [];
    private ongoingTodos: taskItemProps[] = [];
    private formItems: InputFormItemProps[] = [
        {title: 'Task Name', type: 'text'}
    ];

    private addTodo(inputVal: string) {
        const time: Date = new Date();
        this.todos.push({
            id: this.todos.length,
            time_stamp: time,
            name: inputVal,
            doing: false,
            editing: false
        })

        localStorage.todos = JSON.stringify(this.todos);
        console.log(this.todos);
    }

    private moveToOngoingTask (id: number) {
        const target: taskItemProps = this.todos[id];
        if(target.editing == true) {alert('finish editing bofore you move you task');return;}
        target.doing = !target.doing;
        this.ongoingTodos.push(this.todos[id]);
        localStorage.ongoingTodos = JSON.stringify(this.ongoingTodos);
        console.log(this.ongoingTodos);
        this.deleteTask(id);
    }

    private deleteTask(id: number, text?: string) {
        const target: taskItemProps = this.todos[id];
        this.todos = this.todos.filter(function(v) {
            return v != target;
        })

        localStorage.todos = JSON.stringify(this.todos)
        console.log('todo deleted!');
    }

    private editTask(id: number, text?: string) {
        const target: taskItemProps = this.todos[id];
        target.editing = !target.editing;
        console.log('todo edited');
    }

    protected beforeMount(): void {
        if(localStorage.todos) {
            this.todos = JSON.parse(localStorage.todos);
        }
    }
}
</script>

<style lang='sass' scoped>
@import 'variable'

.vue-create-task
    .container
        section.task-input
            align-items: center

            .input-field
                margin: 0

        // section.tasks
        //     overflow: scroll
</style>
