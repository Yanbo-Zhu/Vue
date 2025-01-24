
https://topaz-naranja-ef8.notion.site/Vue-Vorbereitungsaufgabe-TODO-List-1817794f0a6780f18108f0def3fb1da6

1. App.vue: Ergänze `<script setup> `um die notwendigen `imports`.
2. App.vue: Die `InputField` Komponente schickt Informationen über hinzugefügte Tasks an `App.vue`. Ein Task ist ein JS-Objekt, das aus zwei Key-Value-Pairs besteht: `name` und `completed`. Der `name` ist dabei die eingegebene Aufgabe selbst, `completed` sollte per default auf `false` gesetzt sein. Die Tasks sollen in dem Array gespeichert werden, das als Property an `TaskList` Komponente übergeben wird.\
3. Header.vue: Szymon ist ein chaotischer Junge. Leider funktioniert die Komponente nicht so ganz, wie er sich vorgestellt hat. Finde und korrigiere den Fehler.
4. Header.vue: Ergänze `<style scoped>` um die richtige Implementierung des `#flexdiv` Selectors. Wie der Name schon sagt, sollte es sich um einen Flex-Container handeln. Seine Flex-Items sollten horizontal zentriert werden.
5. InputField.vue: Die in das Eingabefeld eingegebene Aufgabe sollte gespeichert werden, um später emittiert zu werden. Wie sollte die Variable heißen, in der die Aufgabe gespeichert wird, damit die Kommunikation zwischen den Komponenten reibungslos funktioniert?
6. InputField.vue: Klickt man auf den Button, soll die Funktion `addTask` aufgerufen werden. Sie schickt die Eingabe an `App.vue`, da dort alle Tasks gespeichert werden und setzt `newTask` auf den ursprünglichen Zustand zurück.

7. InputField.vue: Der Button `btn` sollte einen Abstand von 30px zum Eingabefeld haben. Für bessere Sichtbarkeit sollte er auch einen schwarzen, durchgehenden, 1px dicken Rahmen haben.
8. TaskList.vue: Die `TaskList` Komponente benötigt eine Property und emittiert Daten an `App.vue`. Schau dir noch einmal die `App.vue` an und ergänze `<script setup>` in `TaskList.vue`
9. TaskList.vue: Im `<template>` sollte für jeden Task ein Listenelement `<li>` erstellt werden. Die Liste benötigt dabei nicht nur den Task selbst, sondern auch seinen Index, um einfacher durch die Liste iterieren zu können.
10. TaskList.vue: Nach einem Klick auf den `removeBtn` soll die Information weitergegeben werden, welches List Item gelöscht werden soll. Überlege dir, was am sinnvollsten wäre zu emittieren.
11. App.vue: Zu guter Letzt vervollständige das `removedTask` Event. Wie kann man einen beliebigen Index aus einem Array löschen?



# 1 App.vue 

```vue
<script setup>

import Header from './components/Header.vue';
import InputField from './components/InputField.vue';
import TaskList from './components/TaskList.vue';

import { ref } from 'vue'

const title = ref('TODO List')
const tasks = ref([])


</script>


<template>
  <Header 
    :titleProp="title"
  />

  <InputField
  @newTaskAdded="tasks.push({name: $event, completed: false})"    
  />

  <!-- @removedTask="tasks.splice($event, 1)"   // when a removeTask event vorkommt, loschen xx in array.   $event 包含了 index value. 因为 taskList.vue 中 emit 了 removedTask, 传递了 index .  -->
  <TaskList 
    :tasksProperty="tasks"
    @removedTask="tasks.splice($event, 1)"
  />
</template>

```


# 2 Header.vue

```vue
<script setup>
	defineProps(['title'])
</script>

<template>
	<div id="flexdiv">
	  <img id="vuelogo" src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Vue.js_Logo_2.svg/1024px-Vue.js_Logo_2.svg.png" alt="VueLogo">
    <!-- <h1>{{ title }}</h1>   // 不对 应为 app.vue 中 为 titleProp, 这里也应该用 titleProp  -->
    <h1>{{ titleProp }}</h1>
	</div>
</template>

<style scoped>
#flexdiv {
    display: flex;
    justify-content: center;	
}
#vuelogo {
	height: 50px;
	padding: 5px 20px 0px 0px;
}
h1 {
	margin: 0px;
}
</style>
```


# 3 InputField.vue

```vue
<script setup>
import { ref } from 'vue'

const newTask = ref('')   // newTask 中含有 inputField 的值, 弹射给 app.vue . 但是这里没有定义 emit, 所以不能直接弹射给 app.vue . 但是可以定义一个 emit, 用来告诉 app.vue 有新的 task 被添加了 .
const emit = defineEmits(['newTaskAdded'])  // tell the parent component that a new task has been added. aber nicht definiert , was geschickt wird.  App.vue hat newTaskAdded definiert, so dass es weiß, was es empfangen soll. the name of the event is newTaskAdded, which is defined in App.vue.

function addTask() {
	if (newTask.value.trim()) {
	  emit('newTaskAdded', newTask.value) // 在 app.vue 定义了 newTaskAdded 这个名字 . newTask.value sollte geschickt werden 
  }
}
</script>

<template>
	<form @submit.prevent>
	  <label for="todo">
		  <h2>Add a task to the TODO list!</h2>
		</label>
    <input id="todoInput" type="text" placeholder="Type your task here..."    v-model="newTask"   >
    <button id="btn" @click="$emit('addTask')" >Add</button>
  </form>
</template>

<style scoped>
#todoInput {
	width: 300px;
}
#btn {
    margin-left: 30px;
    border: 1px black solid;
}
</style>
```


# 4 TaskList.vue 

```vue
<script setup>
import { ref } from 'vue';

defineProps(['tasksProperty']);
emit = defineEmits(['removedTask']);



</script>

<template>
	<ul>

    < !-- // :key = "index 特意告诉 vue 用 index 作为 key, 因为这个 list 是动态的, 有可能会有重复的 key.  用key 的值 (就是 index 的值 ) 来区分 task 是相同的还是不通 , 以此来确定是否为 不同的 list-element -> 
    < !-- :class="{ completed: task.completed }"   // : 就是 v-bind:, dynamischr vindung von class.  completed: true 的时候,  completed 这个 clase 被使用 .  completed: false 的时候, completed 这个 class 不被使用 .   completed class 这个css class 在同一个文档最下面被定义,  他用来改变背景颜色 .  .completed .task-text  用来定义 element 中的文字被划横线或者不被画横线 -> 
    < !-- v-model="task.completed"  // 当 checkbox 被选上的时候, task.completed 会变成 true, 从而触发 class 的变化. 他原本是 false  ->
		<li v-for="(task, index) in tasksProperty"
	    :key = "index"  
      class="task-item"
      :class="{ completed: task.completed }"  
	  >
			<input 
		    type="checkbox" 
	      class="checkbox" 
	      v-model="task.completed"
      >

	    <span class="task-text" v-text="task.name"></span>
	    
      <button id="removeBtn"   @click="$emit('removedTask', index )"            >-</button>
	  </li>
  </ul>
</template>

<style scoped>
ul {
	list-style: none;
  padding: 0;
  margin: 0;
}

.task-item {
	display: flex;
  align-items: center;
  margin: 10px 0;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
  transition: background-color 0.3s ease;
}

.checkbox {
	margin-right: 10px;
  width: 20px;
  height: 20px;
}

.task-text {
	flex-grow: 1;
  font-size: 16px;
  color: #333;
}

#removeBtn {
	background-color: #ff4d4d;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s ease;
}

#removeBtn:hover {
	background-color: #ff1a1a;
}

#removeBtn:active {
	background-color: #e60000;
  transform: scale(0.95);
}


<!--  用来定义element中的文字被划横线或者不被画横线->
.completed .task-text {   
	text-decoration: line-through;
	font-style: italic;
  color: gray;
}

.completed {
	background-color: #f0f0f0;
}
</style>
```
