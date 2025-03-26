<template>
  <div class="parent">
    <div class="menu">
      <h2>Меню записей</h2>
      <input v-if="pokaz" v-model="searchQuery" @input="filterItems" class="type" placeholder="Поиск...">
      <ul v-if="visible1">
      <Record  
        v-for="(item) in decury"
        :item="item"
        @edit-item="handleEditItem"
        @redact-item="handleRedactItem"
        @finish-editing="handleFinishEditing"
        @remove-item="handleRemoveItem"
      />
      </ul>
      <ul v-if="visible2">
        <Record  
        v-for="(item) in items"
        :item="item"
        @edit-item="handleEditItem"
        @redact-item="handleRedactItem"
        @finish-editing="handleFinishEditing"
        @remove-item="handleRemoveItem"
      />
      </ul>
      <p v-if="items.length > 0">Нажмите на пункт списка чтобы посмотреть или отредактировать его текст</p>
      <p v-if="items.length > 0">Нажмите два раза на пункт списка чтобы отредактировать его название</p>

      <button @click="newNoteBtn">Новая запись</button>

    </div>
    <div class="textarea">
      <textarea v-model="newItem" @keyup.enter="addItem" placeholder="Нажмите на пункт списка чтобы посмотреть или отредактировать его"></textarea>
    </div>
  </div>
</template>

<script>

import Record from './components/Record.vue'

export default {
  components: { Record },
  data() {
    return {
      items: [],
      newItem: '',
      searchQuery: '',
      decury: [],
      visible1: false,
      visible2: true,
      currentItem: null,
    };
  },
  computed: {
    pokaz() {
      return this.items.length > 0;
    }
  },
  methods: {
     addItem() {
      if (this.currentItem) {
        this.currentItem.content = this.newItem;
        this.newItem = '';
        this.currentItem = null;  // Сброс текущего редактируемого элемента
      }
    },
    newNoteBtn() {
      const noteTitle = prompt("Введите название новой записи:");
      if (noteTitle.trim() !== '') {
        const newNote = { 
          title: noteTitle, 
          content: "", 
          isEditing: false, 
          id: Date.now()  // Используем уникальный id для каждого элемента
        };
        this.items.push(newNote);
      }
    },
    handleRedactItem(item) {
      this.newItem = item.content;
      this.currentItem = item;
    },
    handleRemoveItem(item) {
      // Ищем элемент по id
      const itemIndex = this.items.findIndex(i => i.id === item.id);
      if (itemIndex !== -1) {
        this.items.splice(itemIndex, 1);  // Удаляем элемент по индексу
      }
       const itemIndex2 = this.decury.findIndex(i => i.id === item.id);
      if (itemIndex2 !== -1) {
        this.decury.splice(itemIndex2, 1);  // Удаляем элемент по индексу
      }
    },
    filterItems() {
      if (this.searchQuery !== '') {
        const filtered = this.items.filter(item =>
          item.title.toLowerCase().startsWith(this.searchQuery.toLowerCase())
        );
        this.decury = filtered;
        this.visible1 = true;
        this.visible2 = false;
        return filtered;
      } else {
        this.visible1 = false;
        this.visible2 = true;
        return this.items;
      }
    },
    handleEditItem() {
      this.currentItem.isEditing = true;
    },
    handleFinishEditing() {
      this.currentItem.isEditing = false;
    }
  }
};
</script>



<style scoped>
* {
    margin: 0;
    padding: 0;
}
.parent {
    width: 100%;
    display: flex;

}
.menu {
    width: 250px;
    height: 900px;
    background-color: #f4f4f4;
    margin-right: 20px;
    padding: 10px;
}
.menu h2 {
    font-family: Arial, Helvetica, sans-serif;
    padding-top: 30px;
    padding-left: 15px;
}
.type {
  margin-top: 20px;
  margin-left: 15px;
  width: 200px;
  height: 30px;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 17px;
}
ul {
  width: 220px;
  margin-top: 15px;
  font-family: Arial, Helvetica, sans-serif;
  list-style-type: none;
  font-size: 17px;
  padding: 0;
}
p {
  margin-top: 10px;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 18px;
}
button {
  padding: 10px;
  margin-top: 10px;
  border: none;
  background-color: #4CAF50;
  color: white;
  cursor: pointer;
  border-radius: 5px;
}

button:hover {
  background-color: #45a049;
}

.textarea {
    flex-grow: 1;
    padding: 10px;
}
textarea {
    width: 100%;
    height: 100%;
    border: 1px solid #ccc;
    padding: 10px;
    font-size: 16px;
    border-radius:5px;
    font-family: Arial, Helvetica, sans-serif;
}
</style>