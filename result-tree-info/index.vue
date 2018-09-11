<template>
    <section class='result-tree-info'>
        <div class='tree-img'>
            <template v-for='(item, index) in [1, 2, 3, 4, 5, 6]'>
                <div :class='"tree-img" + index' v-show='isShow(index)' :key='index'></div>
            </template>
        </div>
        <div class='tree-status'>
            <div class='tree-status-item' v-for='(img, index) in treeUi.statusImgs' :key='index'>
                <img :src='img'/>
            </div>
            <div :class='["tree-status-select-circle", "circle" + treeStatus]'></div>
        </div>
        <div class='tree-msg'>
            <p v-for='(text, index) in treeUi.texts' v-show='isShow(index)' :key='index'>{{text}}</p>
        </div>
    </section>
</template>

<script>

export default {
    data() {
      return { 
        treeUi: {
           treeImg: require(`@/assets/img/disciplineTree/tree2.png`),
           texts: [
               '真不错，下节课请继续保持哦',
               '同学们，下节课争取让纪律更好点吧',
               '太可惜，小树冰冻次数太多，下节课继续努力吧',
               '下节课时,各位同学一定要准时到哦',
               '下节课大家要准时到，并且保护好我们的小树哦，加油！'
           ],
           statusImgs: [0, 1, 2, 3, 4, 5].map((item) => require(`@/assets/img/discipline-result/tree${item}.png`))
        },
        treeStatus:3
      }
    },
    computed: {
        msgIndex() {
            return 3;
        }
    },
    methods: {
        isShow(index) {
            return index === this.treeStatus;
        }
    }
}
</script>

<style scoped src='./index.css' />

