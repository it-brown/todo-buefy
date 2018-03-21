<template lang='pug'>
.vue-task-item
    p.task-title(:class='{ hidden: todo.editing}' @click='triggerEditTask(uniqueID)') {{ todo.name }}
    b-input.task-title(v-if='todo.editing' @keyup.native.enter='triggerEditTask(uniqueID)' v-model='todo.name')

    .edit-bar
        .toggle-edit-bar
            img(:src='imgs["right"]')
        .erase-button
            img(:src='imgs["trash"]' @click='triggerDeleteTask(uniqueID)')
        .move-to-ongoing(v-if='imgs["begin"]')
            img(:src='imgs["begin"]' @click='triggerMoveTask(uniqueID)')
        .move-to-ongoing(v-if='imgs["done"]')
            img(:src='imgs["done"]' @click='triggerArchiveTask(uniqueID)')
</template>

<script lang='ts'>
import { Vue, Component, Prop } from 'vue-property-decorator';
import VueUtil from '@/scripts/util/VueUtil';

/**
 * Vue Component
 */
@Component
export default class TaskItem extends Vue {
    @Prop({type: Number})
    private uniqueID?: number;
    @Prop({type: Object})
    private imgs?: object;
    @Prop({type: Object})
    private todo?: object;

    private check(): boolean {
        if(this.$props.todo.doing == true) {
            return true;
        }else {
            console.log("else");
            return false;
        }
    }

    private triggerEditTask(key: number) {
        if(this.check() == false) {
            console.log(this.$props)
            this.$emit('editTask', this.$props.uniqueID);
        }else if (this.check() == true){
            console.log(this.$props.uniqueID);
            this.$emit('editTask', this.$props.uniqueID, 'ongoing');
        }
    }

    private triggerDeleteTask(key: number) {
        if(this.check() == false) {
            this.$emit('deleteTask', this.$props.uniqueID);
        }else if (this.check() == true){
            console.log(this.$props.uniqueID);
            this.$emit('deleteTask', this.$props.uniqueID, 'ongoing');
        }
    }

    private triggerMoveTask(key: number) {
        if(this.check() == false) {
            this.$emit('moveTask', this.$props.uniqueID);
        }else if (this.check() == true){
            this.$emit('moveTask', this.$props.uniqueID, 'ongoing');
        }
    }

    private triggerArchiveTask(key: number) {
        this.$emit('archiveTask', this.$props.uniqueID, 'ongoing');
    }
}
</script>

<style lang='sass' scoped>
@import 'variable'

.vue-task-item
    display: flex
    height: 50px
    display: flex
    border: 1px solid black
    overflow: hidden
    margin: 3px 0
    border-radius: 5px

    & > .task-title
        flex: 5
        overflow: scroll

    & > .edit-bar
        flex: 2
        display: flex
        $a: calc(100vw * 3 / 5  / 2 * 2 / 7 * 2 / 3)
        transform: translateX($a)

        & >  .toggle-edit-bar, .move-to-ongoing, .erase-button
            display: flex
            justify-content: center
            align-items: center
            flex: 1
            border-left: 1px dashed black

    & > .edit-bar:hover
        transform: translateX(0)
        .toggle-edit-bar
            display: none

.hidden
    display: none</style>
