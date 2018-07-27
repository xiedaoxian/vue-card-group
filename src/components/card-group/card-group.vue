<template>
  <div class="edit-card-group card-bordered">
    <div class="card-group-head">
        <slot name="title"></slot>
    </div>
    <div class="card-group-extra">
      <!-- <a href="#" @click.prevent="addCard">
        <Icon type="plus-round"></Icon>
        添加
      </a> -->
      <Button type="text" icon="plus-round" @click="addCard">添加</Button>
    </div>
    <div class="edit-card card-bordered" :key='index' v-for="(formData, index) in dataList" v-show="!formData.isDelete">
      <div class="card-head">{{title}}-{{index + 1}}</div>
      <div class="card-extra">
        <!-- <a href="#" @click.prevent="deleteCard(index)">
          <Icon type="trash-a"></Icon>
          删除
        </a> -->
        <Button type="text" icon="trash-a" @click="deleteCard(index)">删除</Button>
      </div>
      <div>
        <slot></slot>
        <!-- <edit-form-renderer :ref="'form'+index" :items="items" :itemData="formData"></edit-form-renderer> -->
      </div>
    </div>
  </div>
</template>
<style lang="less">
  @import './card-group.less';
</style>
<script>
export default {
  name: 'CardGroup',
  components:{
  },
  props:{
    title:{
      type: String,
      default(){
        return 'title';
      }
    },
    itemData:{
      type: Array,
      default(){
        return [];
      }
    },
  },
  data(){
    return{
      dataList: this.itemData
    }
  },
  computed:{

  },
  created:function(){
    console.log("created card")
    console.log(this.itemData)
  },
  beforeMount:function(){

  },
  mounted:function(){

  },
  beforeDestroy:function(){

  },
  destroyed:function(){

  },
  methods:{
    // 提供给父组件调用，返回卡片中表单值
    getModel(){
      console.log(this.$refs);
      // 卡片控件的数据为数组，每项对应一个卡片
      let cardData = [];
      for (const key in this.$refs) {
        if (this.$refs.hasOwnProperty(key) && !this.isDelete(key)) {
          const editForm = this.$refs[key];
          cardData.push(editForm[0].getModel());
        }
      }
      return cardData;
    },
    // 提供给父组件调用，校验卡片中表单项
    validate(func){
      // 校验卡片中的子表单
      let cardValid = true;
      for (const key in this.$refs) {
        if (this.$refs.hasOwnProperty(key) && !this.isDelete(key)) {
          const editForm = this.$refs[key];
          cardValid = editForm[0].validate() && cardValid;
        }
      }
      // 返回卡片校验结果
      return cardValid;
    },
    addCard() {
      console.log("addCard");
      this.dataList.push(
        {
          isDelete: false
        }
      );
      console.log(this.dataList);
    },
    deleteCard(index) {
      console.log("deleteCard" + index);
      new $confirm(this.$t('module.flexField.warning.confirmToDelete'), this).then(() => {
        // this.dataList.splice(index, 1);
        this.dataList[index].isDelete = true;
        this.dataList = this.dataList.slice(0);
      });
      console.log(this.dataList);
    },
    isDelete(key) {
      // key == formXXX，截取XXX
      let index = key.substring(4);
      return this.dataList[index].isDelete;
    }
  },
  watch:{

  },
  directives:{

  }
}
</script>
