<template lang='pug'>
.vue-archived-task
    .container
        section.archived-tasks.columns.is-multiline
            task-item.card.column.is-4(v-for='archivedTodo, i in archivedTodos' :key='i' :uniqueID='i' :todo='archivedTodo' :imgs='archivedTodoImgs'
                @deleteTask='deleteTask')
</template>

<script lang='ts'>
import { Vue, Component } from 'vue-property-decorator';
import VueUtil from '@/scripts/util/VueUtil';
import { InputFormItemProps, taskItemProps } from '@/components/interface';

/**
 * Vue Component
 */
@Component
export default class ArchivedTask extends Vue {
    private uniqueID: number = 0;
    private archivedTodoImgs: Imgs = {
        right: require('@/resources/img/right.png'),
        trash: require('@/resources/img/trash-can.png'),
        done: require('@/resources/img/fireworks.png')
    }
    private archivedTodos: taskItemProps[] = [];

    private deleteTask(id: number, text?: string) {
        const target: taskItemProps = this.archivedTodos[id];
        this.archivedTodos = this.archivedTodos.filter(function(v) {
            return v != target;
        })

        localStorage.archivedTodos = JSON.stringify(this.archivedTodos)
        console.log('ongoingTodo deleted!');
    }

    protected beforeMount(): void {
        if(localStorage.archivedTodos) {
            this.archivedTodos = JSON.parse(localStorage.archivedTodos);
        }
    }
}
</script>

<style lang='sass' scoped>
@import 'variable'

.vue-archived-task
</style>
