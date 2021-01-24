<template>
  <div>
    <button @click="addMemo" class="add-memo-btn">+</button>
    <div class="memo-list" v-for="(memo,index) in memos" :key="index" @click="selectMemo(index)"
      :data-selected="index === selectedIndex">
      <p>{{displayTitle(memos[index])}}</p>
    </div>
  </div>
</template>

<script>
export default ({
  props: {
    memos: Array,
    selectedIndex: Number,
    show: Boolean,
  },
  data: function () {
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
    addMemo: function () {
      this.$emit("addlist")
    },
    selectMemo: function (index) {
      if (index === this.selectedIndex) {
        this.switchBetweenIndexAndEdit()
      }
      this.$selectedIndex = index
      this.$emit("select", this.$selectedIndex, this.$show)
    },
    displayTitle: function (text) {
      return text.split(/\n/)[0]
    },
    switchBetweenIndexAndEdit: function () {
      this.$show = !this.$show
    }
  }
})
</script>


<style>
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
  font-size: 26px;
}
</style>