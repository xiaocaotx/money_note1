<template>
<layout>
  <div class="title-wrapper">
    <Icon  class="leftIcon" name="left" @click.native="goBack"/>
    <span class="title">编辑标签</span>
    <span class="rightIcon"></span>
  </div>
  <div class="form-wrapper">
    <Notes label-name="标签名"
           placeholder="请输入标签名"
           :value="tag.name"
          @update:value="update"
    >
    </Notes>
  </div>
  <div class="button-wrapper">
    <Button @click="remove">删除标签</Button>
  </div>
</layout>
</template>

<script lang = "ts">
import Notes from '@/components/money/Notes.vue';
import Button from '@/components/Button.vue';
import Vue from 'vue'
import {Component} from 'vue-property-decorator';


@Component({
  components: {Button, Notes}
})
export default class EditLabel extends Vue{
 tag?: TagLabel =undefined;

beforeCreate(){
  this.$store.commit("fetchTags");
}
 created(){
   const id = this.$route.params.id;
   const tags = this.$store.state.tagList as TagLabel[];
   const findtag = tags.filter(t => t.id===id)[0];

   if (findtag) {
     this.tag = findtag;
   } else {
     this.$router.replace('/404');
   }
 }
  update(name: string) {
    if (this.tag) {
      this.$store.commit("updateTag",{"id":this.tag.id,"name":name})
    }
  }
  remove() {
    if (this.tag) {
      this.$store.commit("removeTag",this.tag.id)
    }
  }

  goBack() {
    this.$router.back();
  }
}
</script >

<style lang = 'scss' scoped>
.title-wrapper {
  text-align: center;
  font-size: 16px;
  padding: 12px 16px;
  background: white;
  display: flex;
  align-items: center;
  justify-content: space-between;
  > .title {
  }
  > .leftIcon {
    width: 24px;
    height: 24px;
  }
  > .rightIcon {
    width: 24px;
    height: 24px;
  }
}
.form-wrapper {
  background: white;
  margin-top: 8px;
}
.button-wrapper {
  text-align: center;
  padding: 16px;
  margin-top: 44-16px;
}
</style>