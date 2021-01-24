<template>
  <div>
    <div>
      <textarea class="memo-textarea" :value="searchText" @input="searchText = $event.target.value"
        placeholder="ここに見出しを入力してください"></textarea>
      <div class="bottan-space">
        <button class="edit-memo-btn" @click="editMemo">編集</button>
        <button class="delete-memo-btn" v-if="memos.length > 0" @click="deleteMemo">削除</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default ({
  props: {
    memos: Array,
    selectedIndex: Number,
  },
  data: function () {
    return {
      newText: "",
    };
  },
  computed: {
    searchText: {
      get() {
        return this.memos[this.selectedIndex]
      },
      set(value) {
        this.newText = value
      }
    }
  },
  methods: {
    deleteMemo: function () {
      this.$emit("delete")
      if (this.$selectedIndex > 0) {
        this.$selectedIndex--
      }
    },
    editMemo: function () {
      if (this.newText === "") return
      this.$emit("edit", this.newText)
      this.newText = ""
    }
  }
})
</script>


<style>
.bottan-space {
  display: flex;
  justify-content: space-between;
}

.memo-textarea {
  height: 500px;
  width: 100%;
  margin-bottom: 10px;
  border-radius: 8px;
  font-size: 20px;
}

.edit-memo-btn {
  width: 60%;
  border-radius: 4px;
  border-width: 0.5px;
  border-color: #EEEEEE;
  margin-right: 10px;
}

.delete-memo-btn {
  width: 35%;
  border-radius: 4px;
  border-width: 0.5px;
  border-color: #EEEEEE;
}
</style>