<template>
      <ul v-if="visible1">
        <li v-for="(item) in decury" @dblclick="editItems" @click="redacte(item)">

          <span v-if="!item.isEditing" >{{ item.title }}</span>
          <input v-if="item.isEditing" v-model="item.title" class="input" @blur="finishEditing" 
          @keyup.enter="finishEditing"/>

          <a @click.stop="remove(item)">&#10006;</a>
        </li>
      </ul>
      <ul v-if="visible2">
        <li v-for="(item) in items" @dblclick="editItems(item)" @click="redacte(item)">
          <span v-if="!item.isEditing" >{{ item.title }}</span>
          <input v-if="item.isEditing" v-model="item.title" class="input" @blur="finishEditing" 
          @keyup.enter="finishEditing"/>

          <a @click.stop="remove(item)">&#10006;</a>
        </li>
      </ul>
</template>

<script>
export default {
    props: {
        editItems: {
            type: Function,
            required: true
        },
        redacte: {
            type: Function,
            required: true
        },
        finishEditing: {
            type: Function,
            required: true
        },
        remove: {
            type: Function,
            required: true
        },
        visible1: {
          type: Boolean,
          required: true
        },
        visible2: {
          type: Boolean,
          required: true
        },
        items: {
            type: Array,
            required: true
        },
        decury: {
          type: Array,
          required: true
        }
    }
}
</script>

<style scoped>
.input {
  width: 200px;
  height: 20px;
  font-size: 17px;
  font-family: Arial, Helvetica, sans-serif;
}
ul {
  width: 220px;
  margin-top: 15px;
  font-family: Arial, Helvetica, sans-serif;
  list-style-type: none;
  font-size: 17px;
  padding: 0;
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