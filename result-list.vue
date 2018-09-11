<template>
    <div class='result-list'>
        <result-list-item
          class='result-list-item'
          v-for="(result, index) in resultList"
          :icon='result.icon'
          :iconText='result.iconText'
          :numText='result.numText'
          :nums='result.nums'
          :key='index'
        ></result-list-item>
    </div>
</template>
<script>
import ResultListItem, {TEXT_PLACEHOLDER, ICON_TYPES} from './result-list-item';

export default {
    data() {
        return {
            treeInfo: {
                icon: ICON_TYPES.TREE,
                iconText: '88%',
                numText: '这节课结束啦，小树成长到$$$',
                nums: ['88%']
            },
            medalInfo: {
                icon: ICON_TYPES.MEDAL,
                iconText: 'X 6',
                numText: '每位同学可获得$$$枚纪律奖章',
                nums: [6]
            },
            iceInfo: {
                icon: ICON_TYPES.ICE,
                iconText: 'X 9',
                numText: '出勤率大于$$$，被冻结$$$次',
                nums: ['80%', 7]
            }
        }
    },
    computed: {
        resultList() {
            return [
                this.treeInfo,
                this.medalInfo,
                this.iceInfo
            ]
        }
    },
    components: {
        ResultListItem
    }
}
</script>

<style scoped>
    .result-list .result-list-item{
        line-height:100px;
        padding:0 30px;       
    }
    .result-list .result-list-item:not(:last-child){
        margin-bottom:30px;
    }
</style>
