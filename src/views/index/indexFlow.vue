<template>
    <div class="flow" v-infinite-scroll="'cc'">
        <div class="flow-item" v-for="item in flows" :key="item.id">
            <QuestionModule :question="item" v-if="(item.type.indexOf('expert_question') !== -1)" />
            <ArticleModule :article="item" v-if="(item.type.indexOf('article') !== -1)" />
            <RewardModule :reward="item" v-if="(item.type.indexOf('reward') !== -1)"/>
            <LiveModule :live="item" v-if="(item.type.indexOf('live') !== -1)"/>
            <FineModule  :fine="item" v-if="(item.type.indexOf('fine') !== -1)"/>
        </div>
    </div>
</template>

<script>

    import QuestionModule from '../../components/module/question'
    import ArticleModule from '../../components/module/article'
    import RewardModule from '../../components/module/reward'
    import LiveModule from '../../components/module/live'
    import FineModule from '../../components/module/fine'
    import Focus from '../../components/common/focus'
    import {mapGetters} from 'vuex'

    export default {
        name: 'index-flow',
        data () {
            return {
                subnavs: ['首页', '问答', '精选', '文章']
            }
        },
        computed: mapGetters({
            flows: 'flowList',
            loading: 'getLoading',
            flowLastId: 'flowLastId'
        }),
        methods: {
            getScrollTop: function (element) {
                if (element) {
                    return element.scrollTop
                } else {
                    return document.documentElement.scrollTop
                }
            },
            getVisibleHeight: function (element) {
                if (element) {
                    return element.offsetHeight
                } else {
                    return document.documentElement.offsetHeight
                }
            }
        },
        beforeRouteEnter: function (to, from, next) {
            // transition.next()
            // alert('l')
            next(vm => {
                // console.log('vm', vm.$store.state)
                vm.$store.dispatch('getFlowList')
                // console.log('vm2', vm.$store.state)
            })
            console.log(to, from)
        },
        components: { QuestionModule, Focus, ArticleModule, LiveModule, RewardModule, FineModule }
    }
</script>

<style type="text/sass" lang="scss">
    .flow {
        margin-bottom: 54px;

        &-item {
            margin-top: 10px;
        }
    }
    * {
        padding: 0;
        margin: 0;
    }
</style>
