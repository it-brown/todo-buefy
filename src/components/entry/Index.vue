<template lang='pug'>
.vue-index
    .reactive-title {{ reactiveTitle() }}

    .hero.is-success.is-fullheight
        .hero-head
            header.navbar
                .container
                    .navbar-brand
                        h1.navbar-item Get things done
                        span.navbar-burger.burger(data-target='navbarMenuHero')
                            span(v-for='i in 3' :key='i')

                    #navbarMenuHero.navbar-menu
                        .navbar-end
                            a.navbar-item(:class='{ "is-active": (navbarMode == "HOME") }' @click='navbarMode = "HOME"') Home
                            a.navbar-item(:class='{ "is-active": (navbarMode == "APP") }' @click='navbarMode = "APP"') App

        .hero-body(v-if='navbarMode == "APP"')
            .container
                create-task(v-if='tabMode == "TASKS"')
                ongoing-task(v-if='tabMode == "ONGOING"')
                archived-task(v-if='tabMode == "ARCHIVE"')

        .hero-foot(v-if='navbarMode == "APP"')
            nav.tabs.is-boxed.is-fullwidth
                .container
                    ul
                        li(:class='{ "is-active": (tabMode == "TASKS") }' @click='tabMode = "TASKS"'): a Tasks
                        li(:class='{ "is-active": (tabMode == "ONGOING") }' @click='tabMode = "ONGOING"'): a Ongoing Tasks
                        li(:class='{ "is-active": (tabMode == "ARCHIVE") }' @click='tabMode = "ARCHIVE"'): a Archived Tasks

</template>

<script lang='ts'>
import { Vue, Component } from 'vue-property-decorator';
import VueUtil from '@/scripts/util/VueUtil';
import RootVue from '@/components/base/RootVue';
import CreateTask from '@/components/CreateTask.vue';
import OngoingTask from '@/components/OngoingTask.vue';
import ArchivedTask from '@/components/ArchivedTask.vue';
import TaskItem from '@/components/common/TaskItem.vue';


import Buefy from 'buefy';

Vue.use(Buefy);

/**
 * Vue Component
 */
@Component
export default class Index extends RootVue {
    public title: string = 'index';
    protected navbarMode: string = 'APP';
    protected tabMode: string = 'TASKS';

    protected beforeCreate(): void {
        // Inner Vue 登録
        VueUtil.registerComponents([CreateTask, OngoingTask, ArchivedTask, TaskItem]);
    }

}
</script>

<style lang='sass'>
@import 'all'

.vue-index
</style>
