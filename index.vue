<template>
    <div class='discipline-result'>
        <section class='result-content'>
            <result-tree-info class='result-tree-info'/>
            <result-list class='result-list'/>
            <share-btn class='share-btn'/>
        </section>
    </div>
</template>

<script>
import ResultList from './result-list';
import ResultTreeInfo from './result-tree-info/index';
import ShareBtn from '@/components/share-btn'

export default {
    data: () => ({
    }),
    components: {
        ResultList,
        ResultTreeInfo,
        ShareBtn
    }
}
</script>

<style scoped>
    .discipline-result {
        width:100%;
        height:100%;
        display:flex;

        background:url('../../assets/img/discipline-result/title-img.png') no-repeat top center;
        background-color: #3b0f68;
        color:#fff;
    }
    .discipline-result .result-content {
        margin:250px auto 0;
        width:880px;

        display:flex;
        justify-content: space-between;
        align-items:flex-start;
    }
    .discipline-result .result-list {

    }
    .discipline-result .result-tree-info {
        margin-top: 130px;
    }
    .discipline-result .share-btn {
        position:fixed;
        right:40px;
        bottom:40px;
    }
</style>
