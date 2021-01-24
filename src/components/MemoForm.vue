<template>
    <div >
    <div>{{selectedIndex}}</div>
     <div>{{this.memos[this.selectedIndex]}}</div>
      <div v-show="show">
        <textarea class="memo-textarea" :value="searchText" @input="searchText = $event.target.value" placeholder="ここに見出しを入力してください"></textarea>
        <div class = "bottan-space">
        <button class="edit-memo-btn" @click="editMemo">編集</button>
        <button class="delete-memo-btn" v-if="memos.length > 0" @click="deleteMemo">削除</button>
        </div>
      </div>
  </div>
</template>

<script>

export default({
  props:{
    memos: Array,
    selectedIndex: Number,
    show: Boolean
  },
  data: function() {
return {
    newText: "",
  };
},
  mounted() {
    if (localStorage.getItem('memos')) {
      try {
        this.$memos = JSON.parse(localStorage.getItem('memos'));
      } catch (e) {
        localStorage.removeItem('memos');
      }
    }
  },
  computed: {
  searchText: {
    get () {
      return this.memos[this.selectedIndex]
    },
    set (value) {
      this.newText = value
    }
  }
},
  methods: {
    deleteMemo: function() {
      this.$emit("delete")
      if (this.$selectedIndex > 0) {
        this.$selectedIndex--
      }
      this.newText = ""
      this.saveMemos()
    },
    editMemo: function() {
      if (this.newText === "") return
      let text = this.newText
      this.$emit("edit",text)
      this.newText = ""
      this.saveMemos() 
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

.bottan-space{
  display: flex;
  justify-content: space-between;
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

.add-memo-btn {
  border: none;
  border-radius: 50%;
  background-color: #F0FFFFFF;
  font-size:26px;
}

.edit-memo-btn{
   width: 60%;
   border-radius: 4px;
   border-width:0.5px;
   border-color:#EEEEEE;
   margin-right:10px;
}

.delete-memo-btn{
   width: 35%;
   border-radius: 4px;
   border-width:0.5px;
   border-color:#EEEEEE;
}
</style>
