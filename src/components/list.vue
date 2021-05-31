<template>
  <div class="list">
    <slot name="spantext"><h3>111</h3></slot>
    <div>
      <span>起：</span>
      <input type="number" name="" id="" v-model="start" @blur="blur()">
      <span>末：</span>
      <input type="number" name="" id="" v-model="end" @blur="blur()">
      <button @click="add()">添加</button>
      <h5>显示：{{xianshi}}</h5>
    </div>
    <div>
      <ul>
        <li v-for="(item,index) in this.list" :key="index"><span>{{item.text}}</span><button @click="del(item.lid)">删除</button></li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  data(){
    return{
      start:0,
      end:0,
      xianshi:'',
      list:[],
      id:1,
    }
  },
  watch:{
    clist(n,o){
      this.list = [];
    }
  },
  props:{
    clist:{
      type:Array,
    },
  },
  methods:{
    blur(){
      this.xianshi='';
      if(this.end!=0){
        for(var i=this.start;i<=this.end;i++){
          this.xianshi = this.xianshi + this.clist[i][String(i)];
        }
      }
    },
    add(){
      this.list.push({start:this.start,end:this.end,text:this.xianshi,lid:this.id++});
      console.log(this.list);
      this.btnclick();
    },
    del(lid){
      console.log(lid);
      for(var i=0;i<this.list.length;i++){
        if(this.list[i].lid==lid){
          console.log('查找到');
          this.list.splice(i,1);
          break;
        }
      }
      console.log(this.list);
      this.btnclick();
    },
    btnclick(){
      this.$emit('itemclick',this.list);
    }
  }
}
</script>
<style scoped>
.list{
  margin:2rem 1rem;
  width: 400px;
  height: 200px;
  background-color: rgb(209, 242, 243);
  display: inline-block;
}
h3,h5{
  margin:0 0;
}
input{
  width: 3rem;
}
h5{
  margin-top: 1rem;
}
</style>