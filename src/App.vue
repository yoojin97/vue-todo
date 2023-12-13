<script setup>
// vue3 컴포넌트 api
import { reactive, computed } from 'vue';

// reactive함수: 객체를 반응성으로 만들며 값이 변경되면 리렌더링이 됨
const data = reactive({
  newItem: '',
  items: [],
});

// computed함수: data.items가 변경될때만 함수가 실행
const totalItems = computed(() => data.items.length);
const isCompleted = computed(() => data.items.filter((item) => item.completed).length);
// 할 일 완료 여부

// 할일 추가, 함수표현식
const addItem = () => {
  if (data.newItem !== '') {
    data.items.push({
      id: data.items.length + 1,
      text: data.newItem,
      completed: false,
    });
    data.newItem = '';
  }
};

const deleteItem = (id) => {
  const itemToDelete = data.items.find((item) => item.id === id);

  let index = data.items.indexOf(itemToDelete);
  console.log(itemToDelete);

  data.items.splice(index, 1);
};
</script>

<template>
  <main class="app">
    <h1>Simple to-do list</h1>
    <div class="todo_count">완료: {{ isCompleted }} / 할 일: {{ totalItems }}</div>
    <div class="todo_add">
      <!-- v-model 디렉티브로 폼요소와 데이터를 양방향 연결 -->
      <input
        type="text"
        v-on:keyup.enter="addItem()"
        v-model="data.newItem"
        placeholder="할 일을 입력하세요"
        title="할 일을 입력하세요"
      />
      <button type="button" class="add_btn" v-on:click="addItem()">Add</button>
    </div>
    <ul class="todo_list">
      <!-- 리스트마다 고유 id를 key속성에 단방향 연결 -->
      <li v-for="(item, index) in data.items" v-bind:key="item.id" v-bind:class="{ completed: item.completed }">
        <!-- 변수데이터와 속성연결시 v-bind 사용 -->
        <!-- 라벨클릭시 for로 연결되어 체크박스가 클릭되며
        true, false가 발생하며 v-model로 연결된 속성에 들어감-->
        <input v-bind:id="`check${item.id}`" v-model="item.completed" type="checkbox" />
        <label v-bind:for="`check${item.id}`">{{ item.text }}</label>
        <button v-on:click="deleteItem(item.id)" class="remove_btn" type="button">Remove</button>
      </li>
    </ul>
  </main>
</template>

<style scoped>
.app {
  padding: 40px;
}
.app h1 {
  font-size: 30px;
  font-weight: 700;
  color: var(--text-color-900);
}

.todo_count {
  margin: 10px 0;
}

.todo_add {
  display: flex;
}
.todo_add input[type='text'] {
  height: 40px;
  border: 1px solid #ddd;
  flex-grow: 1;
  width: calc(100% - 60px);
  padding: 0 10px;
  border-radius: 4px;
  margin-right: 10px;
}
.todo_add .add_btn {
  height: 40px;
  padding: 0 10px;
  background: #333;
  color: var(--text-color-100);
  border: none;
  border-radius: 4px;
}

.todo_list {
  margin-top: 20px;
}
.todo_list li {
  margin-bottom: 10px;
  display: flex;
}
.todo_list li.completed label {
  color: #ccc;
  text-decoration: line-through;
}
.todo_list label {
  flex-grow: 1;
  line-height: 32px;
}
.remove_btn {
  height: 32px;
  border: 1px solid var(--primary-color);
  color: var(--primary-color);
  border-radius: 4px;
  padding: 0 5px;
  background: none;
  margin-left: 20px;
}
</style>
