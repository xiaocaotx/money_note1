<template>
  <div  class="numberPad">
    <div class="output"> {{output}}</div>
    <div class="buttons">
      <button @click="inputContent">1</button>
      <button @click="inputContent">2</button>
      <button @click="inputContent">3</button>
      <button @click="remove">删除</button>
      <button @click="inputContent">4</button>
      <button @click="inputContent">5</button>
      <button @click="inputContent">6</button>
      <button @click="clear">清空</button>
      <button @click="inputContent">7</button>
      <button @click="inputContent">8</button>
      <button @click="inputContent">9</button>
      <button class="ok" @click="ok">ok</button>
      <button class="zero" @click="inputContent">0</button>
      <button @click="inputContent">.</button>
    </div>
  </div>
</template>

<script lang = "ts">
import Vue from 'vue'
import {Component, Prop} from 'vue-property-decorator';

@Component
export default class NumberPad extends Vue{
  @Prop() readonly value!: number;//数字面板的历史值
  output = this.value.toString();

  inputContent(event: MouseEvent) {
    const button = (event.target as HTMLButtonElement);
    const input = button.textContent!;
    if (this.output.length === 16) { return; }
    if (this.output === '0') {//0开头情况
      if ('0123456789'.indexOf(input) >= 0) {//如果以0开头后面加别的数字直接取代0
        this.output = input;
      } else {//0后面接.可以
        this.output += input;
      }
      return;
    }
    if (this.output.indexOf('.') >= 0 && input === '.') {return;}//点后加点

    this.output += input;//正常情况，正常衔接
  }

  remove() {
    if (this.output.length === 1) {
      this.output = '0';
    } else {
      this.output = this.output.slice(0, -1);
    }
  }

  clear() {
    this.output = '0';
  }

  ok() {
    this.$emit('update:value', this.output);
    this.$emit('submit', this.output);
    this.output = '0';
  }

}
</script >

<style lang = 'scss' scoped>
@import "~@/assets/styles/helper.scss";
.numberPad{
  .output{
    @extend %innerShadow;
    font-size: 26px;
    text-align: right;
    padding: 9px 16px;
  }
  .buttons{
    @extend %clearFix;
    > button{
      width: 25%;
      height:60px;
      float: left;
      background: transparent;
      border: none;
      &.ok{
        height: 60*2px;
        float: right;
      }
      &.zero{
        width: 25*2%;
      }
      $bg: #f2f2f2;
      &:nth-child(1){
        background: $bg;
      }
      &:nth-child(2), &:nth-child(5) {
        background: darken($bg, 4%);
      }
      &:nth-child(3), &:nth-child(6), &:nth-child(9) {
        background: darken($bg, 4*2%);
      }
      &:nth-child(4), &:nth-child(7), &:nth-child(10) {
        background: darken($bg, 4*3%);
      }
      &:nth-child(8), &:nth-child(11), &:nth-child(13) {
        background: darken($bg, 4*4%);
      }
      &:nth-child(14) {
        background: darken($bg, 4*5%);
      }
      &:nth-child(12) {
        background: darken($bg, 4*6%);
      }
    }
  }
}
</style>