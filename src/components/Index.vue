<template>
  <div>
    <section class="section">
      <div class="content">
        <ul>
          <MemoListItem
            v-for="memo in memos"
            :key="memo.id"
            :memo="memo"
            @show="showMemo"
          />
        <li><p @click="newMemo"> + </p></li>
        </ul>
      </div>
      <div v-if="editMode">
        <div class="content">
          <MemoTextArea class="textarea" placeholder="edit memo" v-model="memoText"/>
        </div>
        <div class="content">
          <button class="button is-primary" v-on:click="editMemo">編集</button>
          <button class="button is-danger" v-on:click="deleteMemo">削除</button>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import MemoTextArea from './MemoTextArea.vue'
import MemoListItem from './MemoListItem.vue'

export default {
  components: {
    MemoTextArea, MemoListItem
  },
  data () {
    return {
      nextMemoId: 0,
      editMemoId: 0,
      newMemoflg: false,
      editMode: false,
      memoText: '',
      memos: []
    }
  },
  methods: {
    showMemo (idToShow) {
      this.editMemoId = idToShow
      this.memoText = this.memos.find(memo => memo.id === this.editMemoId).text
      this.editMode = true
    },
    newMemo () {
      this.memoText = '新規メモ'
      this.newMemoFlg = true
      this.editMode = true
    },
    editMemo () {
      if (this.newMemoFlg) {
        this.memos.push({
          id: this.nextMemoId++,
          title: this.memoText.split('\n')[0],
          text: this.memoText
        })
      } else {
        this.memos = this.memos.filter(memo => {
          if (memo.id === this.editMemoId) {
            memo.title = this.memoText.split('\n')[0]
            memo.text = this.memoText
            return memo
          } else {
            return memo
          }
        })
      }
      this.memoText = ''
      this.editMode = false
      this.newMemoFlg = false
      this.saveMemo()
    },
    saveMemo () {
      localStorage.setItem('memos', JSON.stringify(this.memos))
      localStorage.setItem('nextMemoId', this.nextMemoId)
    },
    deleteMemo () {
      this.memos = this.memos.filter(memo => {
        return memo.id !== this.editMemoId
      })
      this.editMode = false
      this.saveMemo()
    },
    loadMemo () {
      this.memos = JSON.parse(localStorage.getItem('memos'))
      this.nextMemoId = localStorage.getItem('nextMemoId')
      if (!this.memos) {
        this.memos = []
      }
      if (!this.nextMemoId) {
        this.nextMemoId = 0
      }
    }
  },
  mounted: function () {
    this.loadMemo()
  }
}
</script>
<style>
li p {
  text-decoration: underline;
  cursor : pointer;
}
li p:hover {
  color: skyblue;
}
</style>
