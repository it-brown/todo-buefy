<template lang='pug'>
.vue-ongoing-task
    .container
        section.ongoing-tasks.columns.is-multiline
            task-item.card.column.is-4(v-for='ongoingTodo, j in ongoingTodos' :key='j' :uniqueID='j' :todo='ongoingTodo' :imgs='ongoingTodoImgs'
                @editTask='editTask' @deleteTask='deleteTask' @archiveTask='archiveTask')
</template>

<script lang='ts'>
import { Vue, Component } from 'vue-property-decorator';
import VueUtil from '@/scripts/util/VueUtil';
import { InputFormItemProps, taskItemProps } from '@/components/interface';

/**
 * Vue Component
 */
@Component
export default class OngoingTask extends Vue {
    private uniqueID: number = 0;
    private ongoingTodoImgs: Imgs = {
        right: require('@/resources/img/right.png'),
        trash: require('@/resources/img/trash-can.png'),
        done: require('@/resources/img/fireworks.png')
    }
    private ongoingTodos: taskItemProps[] = [];
    private archivedTodos: taskItemProps[] = [];

    private deleteTask(id: number, text?: string) {
        const target: taskItemProps = this.ongoingTodos[id];
        this.ongoingTodos = this.ongoingTodos.filter(function(v) {
            return v != target;
        })

        localStorage.ongoingTodos = JSON.stringify(this.ongoingTodos)
        console.log('ongoingTodo deleted!');
    }

    private editTask(id: number, text?: string) {
        const target: taskItemProps = this.ongoingTodos[id];
        target.editing = !target.editing;
        console.log('ongoingTodo edited!');
    }

    private archiveTask(id: number, text: string) {
        const target: taskItemProps = this.ongoingTodos[id];
        target.doing = !target.doing;
        this.archivedTodos.push(this.ongoingTodos[id]);
        console.log('archivedTodos -> ', this.archivedTodos);
        this.deleteTask(id, text);
        console.log('ongoingTodos -> ', this.ongoingTodos);
        localStorage.archivedTodos = JSON.stringify(this.archivedTodos);
    }

    protected beforeMount(): void {
        if(localStorage.ongoingTodos) {
            this.ongoingTodos = JSON.parse(localStorage.ongoingTodos);
        }
    }
}
</script>

<style lang='sass' scoped>
@import 'variable'

.vue-ongoing-task
</style>
