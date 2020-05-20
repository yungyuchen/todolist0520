<template>
  <div id="app" class="small-container">
    <h1>ToDoList</h1>

	<todolist-form @add:todolist="addTodolist" /> <!--  create the addTodolist method on App.vue, which will modify the todolists array by adding a new item to it. -->
    <todolist-table v-bind:todolists="todolists" @delete:todolist="deleteTodolists" @edit:todolist="editTodolists" /> 
	<!-- 利用v-bind:來傳遞 todolists Array -->
	<!-- add the delete:todolist event And create the deleteTodolists method -->
	<!-- add the edit:todolist event And create the editTodolists method -->
  </div>
</template>

<script>
  import TodolistTable from '@/components/ToDoListTable.vue' // 添加導入 ToDoListTable.vue 子組件
  import TodolistForm from '@/components/ToDoListForm.vue' // 添加導入 ToDoListForm.vue 子組件

  export default {
    name: 'app',
    components: { // App.vue通過components屬性(property)知道可以使用哪些組件 (要先 improt 導入才可使用) 必須在此處添加所有導入 (import) 的組件 (components)
		TodolistTable, // 導入的組件名稱
		TodolistForm, // 導入的組件名稱
	},
	data(){ // add a data() method，and return an todolists array，add IDs to each one to make them uniquely (唯一地) identifiable (可識別的)
		return{ // Data is like React (反應) state (狀態).
			todolists:[ 
				{
					id: 1,
					table:'熟悉Git指令',
				},
				{
					id: 2,
					table:'安裝Vue建置環境',
				},
				{
					id: 3,
					table:'整理技術筆記',
				},
			]
		}
	},
	methods: {
		addTodolist(todolist) { // 傳進ToDoListForm 的 todolist 參數
		// Since I have to make an id as well, I'll just write some code to get the new todolists ID based on(基於) table of items in the array. Note that in a real database, this ID would be uniquely (唯一地) generated (產生) or auto incremented (自動增長).
			const lastId = this.todolists.length > 0 ? this.todolists[this.todolists.length - 1].id : 0; // 判斷原有的 todolists 是否有資料, 有 取得最後一個ID , 無 則等於0
			const id = lastId + 1; // 定義一個ID變數取 新字串的ID
			const newTodolist = { ...todolist, id }; // newTodolist = ...todolist輸入的字串格式 (table) , id  |  把傳進來todolist原有資料型態 + ID 塞進 newTodolist
			// const newTodolist = { id , ...todolist }; 傳資料型態時 裡面的變數可調換位置 (實驗後可執行)
			this.todolists = [...this.todolists, newTodolist]; // this.todolists 新字串格式 = ...this.todolists 字串格式 (原有的todolist字串格式 table , id ) + newTodolist 字串格式 
		},

		deleteTodolists(id) { // 傳進ToDoListTable 的 id 參數
			// this.todolists = this.todolists.filter( todolist => todolist.id !== id ) // ES6寫法 直接篩選掉不等於點擊ID的每個項目,挑選不為ID的顯示
			this.todolists = this.todolists.filter(function(todolist){
					return todolist.id !== id
				});
		},
		
		editTodolists(id,updatedTodolist){ // 傳進ToDoListTable 的 id updatedTodolist 參數 map (映射) through the todolists array, and update the correct (正確的) todolist.
			//  map builds a new array

			// this.todolists = this.todolists.map( todolist => todolist.id === id ? updatedTodolist : todolist ) ES6寫法 
			// 傳入處理中 array 物件 的 value index
			this.todolists = this.todolists.map(function(todolist){ // 跑迴圈執行 為每個 array element  執行一次提供的 function 功能
			// 類似 forEach() method executes (執行) a provided(供給) function (功能) once for each (每個各一次) array (陣列) element(元素).
			// 類似 forEach() 為每一個陣列元素都提供了一次執行的功能 都run過一次
					console.log('todolist.id + ' + todolist.id); // 1 2 3
					console.log('id + ' + id); // 1
					console.log('updatedTodolist + ' + updatedTodolist); // object array 物件
					console.log('todolist + ' + todolist); // object  array 物件
					return todolist.id === id ? updatedTodolist : todolist // 判斷 todolist.id === id 則等於 updatedTodolist 否則為 原本的 todolist
				});
		}
	}
	
  }
</script>

<style>
/* 添加了一些全局樣式 */
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 680px;
  }
</style>