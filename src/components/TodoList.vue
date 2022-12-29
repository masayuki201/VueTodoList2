<template>
  <h1>{{ title }}</h1>
  <input type="text" v-model="newList" placeholder="内容"/>
  <button class="addBtn" @click.prevent="addTodo">追加</button>
  <button class="deleteBtn" @click="deleteTodo">削除</button>
  <br>
  <input class="searchText" type="text" v-model="keyword" placeholder="検索">

  <div class="contents">
    <p v-if="lists.length === 0" class="warningMessage">Todoがありません！</p>
    <ul class="contents_ul" v-else>
      <li class="contents_li" v-for="(list, index) in filteredLists" :key="index">
        <input type="checkbox" v-model="list.isDone"/>
        <span :class="{'list-done':list.isDone }">{{ list.text }}</span>
        <div v-if="list.isActive">
        <span>
          <input type="text" v-model="list.text">
        </span>
          <button class="updateBtn" @click="updateTodo(index)">完了</button>
        </div>
        <button class="editBtn" v-show="!list.isActive" @click="editTodo(index)">編集</button>
      </li>
    </ul>
  </div>

</template>

<script>
import { defineComponent, reactive, toRefs, computed } from "vue";

class State {
  keyword = ''
  newList = ''
  editTodo = false
  lists = [
      {
        text: 'スーパーへ行く'
      },
    {
      text: 'クリーニングを出す'
    },
    {
      text: 'ジムへ行く'
    }
  ]
}


export default defineComponent({
  setup() {
    const state = reactive(new State())

    const addTodo = () => {
      if (!state.newList) {
        alert('文字を入力して下さい')
        return
      }
      state.lists.push({
        isDone: false,
        text: state.newList,
      })
      state.newList = ''
    }

    const editTodo = (index) => {
      state.lists[index].isActive = true
      state.lists[index].text = state.list[index].text
    }

    const updateTodo = (index) => {
      state.lists[index].isActive = false
    }

    const deleteTodo = () => {
      state.lists = state.lists.filter((list) => !list.isDone)
    }

    const filteredLists = computed(() => {
      const searchKeyword = state.keyword;
      if (searchKeyword === '') return state.lists;
      return state.lists.filter((list) => list.text.includes(searchKeyword));
    });

    // const filteredLists = computed(() =>
    // const lists = [];
    // for (const i in this.lists) {
    //   const list = this.lists[i];
    //   if (list.text.indexOf(this.keyword) !== -1) {
    //     lists.push(list);
    //   }
    // }
    // return lists;
    // )

    return {
      ...toRefs(state),
      addTodo,
      editTodo,
      updateTodo,
      deleteTodo,
      filteredLists,
    }
  }
})
  // name: "TodoList",
  // data() {
  //   return {
  //     keyword: '',
  //     newList: '',
  //     lists: [
  //       {
  //         text: 'スーパーに行く',
  //       },
  //       {
  //         text: 'クリーニングを出す',
  //       },
  //       {
  //         text: 'ジムへ行く',
  //       },
  //     ],
  //     editTodo: false,
  //   }
  // },
  // computed: {
  //   filteredLists: function () {
  //     const lists = [];
  //     for (const i in this.lists) {
  //       const list = this.lists[i];
  //       if (list.text.indexOf(this.keyword) !== -1) {
  //         lists.push(list);
  //       }
  //     }
  //     return lists;
  //   }
  // },
  // methods: {
    //追加
    // addTodo() {
    //   if (!this.newList) {
    //     alert('文字を入力して下さい')
    //     return
    //   }
    //   this.lists.push({
    //     isDone: false,
    //     text: this.newList,
    //   })
    //   this.newList = ''
    // },
    //編集
    // updateTodo(index) {
    //   this.lists[index].isActive = true
    //   this.lists[index].text = this.list[index].text
    // },
    // //完了
    // updateDone(index) {
    //   this.lists[index].isActive = false
    // },
    // //削除
    // deleteTodo() {
    //   this.lists = this.lists.filter((list) => !list.isDone)
    // },
  // },
  // props: {
  //   title: String,
  // },
// }
</script>

<style scoped>
.list-done {
  text-decoration: line-through;
}

.contents {
  margin: 16px auto;
  text-align: center;
}

.contents_ul {
  padding-left: 0;
  list-style: none;
  display: inline-block;
}

.contents_li {
  list-style: none;
  text-align: left;
}

.searchText {
  margin: 5px;
}

.addBtn {
  background-color: deepskyblue;
  margin: 5px;
}

.deleteBtn {
  background-color: red;
  color: white;
  margin: 5px;
}

.editBtn {
  margin: 5px;
}

.updateBtn {
  background-color: deepskyblue;
  margin: 5px;
}

.warningMessage {
  color: red;
}

</style>
