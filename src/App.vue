<template>
  <div id="app">
    <div class="title">
      <div v-show="!show">メモ一覧</div>
      <div v-show="show">メモ編集</div>
    </div>

    <div class="editor-wrapper">
      <div class="memo-list-wrapper">
        <memo-list :memos="memos" :show="show" :selectedIndex="selectedIndex" @select="selectList"  @addlist="addList">
        </memo-list>
      </div>
      <div class="btn-and-textarea">
        <memo-form :memos="memos" v-show="show" :selectedIndex="selectedIndex" @edit="editMemo" @delete="deleteMemo">
        </memo-form>
      </div>
    </div>
  </div>
</template>

<script>
import MemoForm from './components/MemoForm'
import MemoList from './components/MemoList'

export default ({
  data: function () {
    return {
      memos: [],
      show: false,
      selectedIndex: 0
    };
  },
  components: {
    'memo-form': MemoForm,
    'memo-list': MemoList
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
    editMemo(int) {
      this.memos.splice(this.selectedIndex, 1, int)
      this.show = false
      this.saveMemos()
    },
    deleteMemo() {
      this.memos.splice(this.selectedIndex, 1)
      this.show = false
      this.saveMemos()
    },
    addList() {
      this.memos.push("")
      this.selectedIndex = this.memos.length -1
      this.show = true
    },
    selectList(int, show) {
      this.selectedIndex = int
      this.show = show
    },
    saveMemos: function () {
      const parsed = JSON.stringify(this.memos);
      localStorage.setItem('memos', parsed);
    }
  }
})
</script>

<style>
.title {
  font-size: 36px;
  margin-left: 26px;
  margin-bottom: 26px;
}

/*一番上の層ここが基準*/
.editor-wrapper {
  display: flex;
  justify-content: space-around;
  font-size: 20px;
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
</style>