<template>
    <li @dblclick="editItems" @click="redacte(item)">

      <span v-if="!item.isEditing">{{ item.title }}</span>
      <input v-if="item.isEditing" v-model="item.title" class="input" @blur="finishEditing" 
      @keyup.enter="finishEditing" />

      <a @click.stop="remove(item)">&#10006;</a>
    </li>
</template>

<script>
export default {
  props: {
    item: Object,  // Пропс для передачи объекта item
  },
  emits: ['edit-item', 'redact-item', 'finish-editing', 'remove-item'],  // Указываем события
  methods: {
    editItems() {
      this.$emit('edit-item'); // Эмитим событие редактирования
    },
    redacte(item) {
      this.$emit('redact-item', item); // Эмитим событие редактирования контента
    },
    finishEditing() {
      this.$emit('finish-editing'); // Эмитим событие завершения редактирования
    },
    remove(item) {
      this.$emit('remove-item', item); // Эмитим событие удаления элемента
    },
  },
};
</script>


<style scoped>
.input {
  width: 200px;
  height: 20px;
  font-size: 17px;
  font-family: Arial, Helvetica, sans-serif;
}
li {
  width: 100%;
  padding-left: 5px;
  cursor: pointer;
  border-bottom: 1px solid #ccc;
  white-space: normal; 
  word-wrap: break-word;
  overflow-wrap: break-word; 
  display: flex;
}
li:hover {
  background-color: #ddd;
}
li a {
  font-size: 16px;
  margin-left: auto;
}
</style>