<template>
 <div>
 <div>{{selectedIndex}}</div>
      <div class="memo-list" v-for="(memo,index) in memos" :key="index" @click="selectMemo(index)" :data-selected="index === selectedIndex">
        <p class="memoTitle">{{displayTitle(memos[index])}}</p>
      </div>
  </div>
</template>

<script>

export default({
  props:{
    memos: Array,
    selectedIndex: Number,
    show: Boolean,
  },
  data: function() {
return {
    newText: ""
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
  methods: {
    addMemo: function() {
      this.show = true
      this.$memos.push("")
      this.$selectedIndex = this.memos.length - 1
      this.newText = ""
      this.saveMemos()
    },
    selectMemo: function(index) {
      if (index === this.selectedIndex) {
        this.switchBetweenIndexAndEdit()
      }
      this.$selectedIndex = index
      this.newText = this.memos[this.$selectedIndex]
      this.$emit("test",this.$selectedIndex)
    },
    displayTitle: function(text) {
      return text.split(/\n/)[0]
    },
    switchBetweenIndexAndEdit: function() {
      this.show = !this.show
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
