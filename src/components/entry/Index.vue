<template lang='pug'>
.vue-index
    .reactive-title {{ reactiveTitle() }}
    sample
</template>

<script lang='ts'>
import { Vue, Component } from 'vue-property-decorator';
import VueUtil from '@/scripts/util/VueUtil';
import RootVue from '@/components/base/RootVue';

import Buefy from 'buefy';

Vue.use(Buefy);

/**
 * Vue Component
 */
@Component
export default class Index extends RootVue {
    public title: string = 'index';

    protected beforeCreate(): void {
        // Inner Vue 登録
        VueUtil.registerComponents([]);
    }
}
</script>

<style lang='sass'>
@import 'all'

.vue-index
</style>
