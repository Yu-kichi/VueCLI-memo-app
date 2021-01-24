<template>
<div id="app">
  <div class="title">
    <div @test="add1"></div>
  <div>{{selectedIndex}}</div>
 
    <div v-show="!show">メモ一覧</div>
    <div v-show="show">メモ編集</div>
  </div>

  <div class="editor-wrapper">
    <div class="memo-list-wrapper">
        <button @click="addMemo" class="add-memo-btn">+</button>
      <memo-list v-show="show" :memos = "memos" :selectedIndex = "selectedIndex" @test="add1" :show = "show"></memo-list>
    </div>
    <div class="btn-and-textarea">
      <memo-form v-show="show" :memos = "memos" :selectedIndex = "selectedIndex" :show = "show" @edit="edit1" @delete = "delete1"></memo-form>
    </div>
  </div>
</div>
</template>

<script>
import MemoForm from './components/MemoForm'
import MemoList from './components/MemoList'

export default({
  data: function() {
return {
    memos: [],
    show: true,
    newText: "",
    selectedIndex: 0
  };
},
components:{
  'memo-form':MemoForm,
  'memo-list':MemoList
},
  mounted() {
    if (localStorage.getItem('memos')) {
      try {
        this.memos = JSON.parse(localStorage.getItem('memos'));
      } catch (e) {
        localStorage.removeItem('memos');
      }
    }
  },
  methods: {
    addMemo: function() {
      this.show = true
      this.memos.push("")
      this.selectedIndex = this.memos.length - 1
      this.newText = ""
      this.saveMemos()
    },
    add1(int){
       this.selectedIndex = int
    },
    edit1(int){
      this.memos.splice(this.selectedIndex, 1, int)
    },
    delete1(){
      this.memos.splice(this.selectedIndex,1)
    },
    selectMemo: function(index) {
      if (index === this.selectedIndex) {
        this.switchBetweenIndexAndEdit()
      }
      this.selectedIndex = index
      this.newText = this.memos[this.selectedIndex]
    },
    displayTitle: function(text) {
      return text.split(/\n/)[0]
    },
    switchBetweenIndexAndEdit: function() {
      this.show = !this.show
    },
    saveMemos: function() {
      const parsed = JSON.stringify(this.memos);
      localStorage.setItem('memos', parsed);
    }
  }
})


</script>


<style>

.title{
  font-size:36px;
  margin-left:26px;
  margin-bottom:26px;
}
/*一番上の層ここが基準*/
.editor-wrapper {
  display: flex;
  justify-content: space-around;
  font-size:20px;
}

/* +ボタンとメモのタイトルを表示する */
.memo-list-wrapper {
  border-top: 1px;
  width: 30%;
}

/* 右側のテキストエリアとボタンの位置、階層はmemo-list-wrapperと同じ層 */
.btn-and-textarea {
  width: 60%;
}


.memo-textarea {
  height: 500px;
  width:  100%;
  margin-bottom: 10px;
  border-radius: 8px;
  font-size:20px;
}

.memo-list {
  padding: 10px;
  box-sizing: border-box;
  text-align: left;
  border-bottom: 1px solid #000;
}

.memo-list[data-selected="true"] {
  background-color: #F0FFFFFF;
}

</style>
