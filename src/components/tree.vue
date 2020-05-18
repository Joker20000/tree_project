<template>
    <div class="tree">

        <treeNode v-bind:treeNodeVal="treeValArr"></treeNode>

        <div class="print">
            <button v-on:click="printJSON">导出</button>
        </div>

    </div>
</template>
<script>
    import {treeObj} from './content_to_display.js'
    import treeNode from './treeNode'
    export default {
        name: 'tree',

        components: {

            treeNode

        },

        data () {

            return {

                treeObj: treeObj,

                treeValArr: [],

            }

        },

        mounted () {

            this.treeValArr = this.treeValArr.concat(this.changeDateType(treeObj));

        },

        methods: {

            //修改数据类型  将对象改为数组
            changeDateType: function (obj) {

                var valArr = [];

                var objKeyArr = Object.keys(obj);

                for(let key of objKeyArr) {

                    valArr.push(this.addVal(key, obj));

                }

                return valArr;

            },

            //将对象改变为key:'', value: '', children:'' 类型  方便迭代处理
            addVal: function (key, obj) {

                var val = {};

                val.key = key;

                val.select = false;

                if(typeof obj[key] === 'object'){

                    val.children = this.changeDateType(obj[key]);

                }else {

                    val.value = obj[key] + '';

                }

                return val;

            },

            //下载选中的数据
            printJSON: function () {

                var JSONVal = this.getValue(this.treeValArr);

                var Link = document.createElement('a');
                Link.download = "数据.json";
                Link.style.display = 'none';
                // 字符内容转变成blob地址
                var blob = new Blob([JSONVal]);
                Link.href = URL.createObjectURL(blob);
                // 触发点击
                document.body.appendChild(Link);
                Link.click();
                // 然后移除
                document.body.removeChild(Link);

            },

            //将下载的数据转换成JSON形式
            getValue: function (obj) {

                var returnVal = {};

                for(let val of obj) {

                    if(val.select){

                        returnVal[val.key] = val.value || this.getValue(val.children);

                    }

                }

                return JSON.stringify(returnVal);

            }

        }
    }
</script>
<style>
    @import url('tree.css');
</style>