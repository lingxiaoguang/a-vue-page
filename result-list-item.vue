<template>
    <div class='result-list-item'>
        <div class='result-list-item-iconInfo'>
            <img :src='iconImg'/>
            {{iconText}}
        </div>
        <div class='result-list-item-numInfo'>
            <p v-html='numInfo'></p>
        </div>
    </div>
</template>

<script>
import _ from 'lodash';

//图标类型
export const ICON_TYPES = {
  TREE: 'tree',
  MEDAL: 'medal',
  ICE: 'ice'  
};

//文本占位符
export const TEXT_PLACEHOLDER = '$$$';
const TEXT_PLACEHOLDER_REGEXP = /\$\$\$/;

//图标url map
const ICON_MAP = {
    [ICON_TYPES.TREE]: require('@/assets/img/discipline-result/tree4.png'),
    [ICON_TYPES.MEDAL]: require('@/assets/img/discipline-result/tree4.png'),
    [ICON_TYPES.ICE]: require('@/assets/img/discipline-result/freeze.png')
};

export default {
    props: {
        // 图标
        icon: {
          type: String,
          required: false,
          default: ICON_TYPES.TREE
        },
        // 图标旁边的文字
        iconText: {
          type:String,
          required: false,
          default: ''  
        },
        // 主体文字
        numText: {
          type:String,
          required: false,
          default: ''
        },
        // 主体文字中的数字
        nums: { 
          type:Array,
          required: false,
          default: []  
        }
    },
    computed: {
        // 图标图片
        iconImg() {
            return ICON_MAP[this.icon];
        },
        //主体信息
        numInfo() {
            let numInfo = this.numText;
            let index = 0;
            while (numInfo.match(TEXT_PLACEHOLDER_REGEXP) !== null) {
                const num = this.nums[index ++];
                const numStyle = 'font-size:24px;color:#ffe60e;padding:4px;';
                const numHtml = `<span style="${numStyle}">${num}</span>`;
                numInfo = numInfo.replace(TEXT_PLACEHOLDER_REGEXP, numHtml);
            }
            return numInfo;
        }
    }
}
</script>

<style scoped>
    .result-list-item {
        width:470px;
        height:100px;
        box-sizing: border-box;
        display:flex;
        justify-content: flex-start;
        align-items: center;

        line-height:30px;
        padding: 0 30px;

        background:#4f2777;
        color:#fff;
        font-size:18px;
    }
    .result-list-item .result-list-item-iconInfo {
        margin-right:30px;
        width:110px;
        display:flex;
        justify-content: flex-start;
        align-items: center;
        
        font-size:24px;
    }
    .result-list-item .result-list-item-iconInfo img{
        padding-right:8px;
    }
    .result-list-item .result-list-item-numInfo {
    }
</style>
