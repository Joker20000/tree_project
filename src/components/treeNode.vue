<template>
    <div class="tree_node">

        <ul>

            <li v-for="(val, index) in treeNodeVal" v-bind:key="val.key">

                <input type="checkbox" v-model="val.select" v-on:click="checkBoxSelect(index)">

                <span v-on:click="changeFn(index, 'up')" class="button" v-if="index !== 0">↑</span>

                <span v-on:click="changeFn(index, 'down')" class="button" v-if="index !== treeNodeVal.length-1">↓</span>

                <span v-on:click="del(index)" class="del">删除</span>

                <span v-on:click="add(index)" class="del" v-if="val.children">添加</span>

                <input type="text" v-model="val.key" class="key">

                <input v-if="val.value" class="value" v-model="val.value">

                <treeNode v-if="val.children" v-bind:treeNodeVal="val.children"></treeNode>

            </li>

        </ul>

        <div class="input_popup" v-if="inputBoxShow">
            <div class="popup_bg"></div>
            <div class="input_box">
                <div class="title">请输入你想要添加的值</div>
                <div class="key_input">
                    <span>请输入key</span>
                    <input type="text" v-model="inputKey">
                </div>
                <div class="val_input">
                    <span>请输入val</span>
                    <input type="text" v-model="inputVal">
                </div>
                <div class="btn">
                    <div v-on:click="addVal">确认</div>
                    <div v-on:click="closePopup">取消</div>
                </div>
            </div>
        </div>

    </div>
</template>
<script>
    import treeNode from './treeNode'
    export default {

        name: 'treeNode',

        props: {

            treeNodeVal: Array

        },

        data () {

            return {

                inputKey: '',

                inputVal: '',

                inputBoxShow: false,

                addIndex: '',

            }

        },

        components: {

            treeNode

        },

        methods: {

            //数据顺序更换
            changeFn: function (index, type) {

                var val = this.treeNodeVal[index];

                this.treeNodeVal.splice(index, 1);

                (type === 'up') && this.treeNodeVal.splice(index-1, 0, val);

                (type === 'down') && this.treeNodeVal.splice(index + 1, 0, val);

            },

            //删除按钮点击事件
            del: function (index) {

                this.treeNodeVal.splice(index, 1);

            },

            //添加按钮点击事件
            add: function (index) {

                this.addIndex = index;

                this.inputBoxShow = true;

            },

            //关闭弹窗
            closePopup: function () {

                this.inputKey = '';

                this.inputVal = '';

                this.inputBoxShow = false;

            },

            //添加子值
            addVal: function () {

                if(this.checkKey()) {

                    console.log('有相同的key值存在');

                }else {

                    var addVal = {

                        key: this.inputKey,

                        value: this.inputVal,

                        select: true

                    };

                    this.treeNodeVal[this.addIndex]['children'].push(addVal);

                }

                this.closePopup();

            },

            //判断添加的key值是否重复
            checkKey: function () {

                var obj = this.treeNodeVal[this.addIndex]['children'];

                var checkVal = false;

                for(let val of obj){

                    if(val.key === this.inputKey){

                        checkVal = true;

                    }

                }

                return checkVal;

            },

            //将选中的值对应的子值都选中
            checkBoxSelect: function (index) {

                if(!this.treeNodeVal[index].select){

                    this.selectAll(this.treeNodeVal[index]);

                }

            },

            //选中子值
            selectAll: function (obj) {

                if(obj.children){

                    for(let val of obj.children){

                        val.select = true;

                        if(val.children){

                            this.selectAll(val);

                        }

                    }

                }

            }

        }

    }
</script>