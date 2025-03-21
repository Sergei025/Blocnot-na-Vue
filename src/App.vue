<template>
  <div class="parent">
    <div class="menu">
      <h2>Меню записей</h2>
      <input v-if="pokaz" v-model="searchQuery" @input="filterItems" placeholder="Поиск...">
      <ul v-if="visible1">
        <li v-for="(item, index) in decury" :key="index" @click="redacte(item, index)">
          {{ item }}
          <a @click.stop="remove(index)">&#10006;</a>
        </li>
      </ul>
      <ul v-if="visible2">
        <li v-for="(item, index) in items" :key="index" @click="redacte(item, index)">
          {{ item }}
          <a @click.stop="remove(index)">&#10006;</a>
        </li>
      </ul>
      <p v-if="items.length > 0">Нажмите на пункт списка чтобы его отредактировать</p>
    </div>
    <div class="textarea">
        <textarea v-model="newItem" @keyup.enter="addItem" placeholder="Введите текст, чтобы создать запись..."></textarea>
    </div>
  </div>
</template>

<script>


export default {
  data() {
    return {
        items: [],
        newItem: '',
        searchQuery: '',
        decury: [],
        visible1: false,
        visible2: true
    }
  },
  computed: {
     pokaz() {
      return this.items.length > 1;
     }
  },
  methods: {
    addItem() {
        if (this.newItem.trim() !== '') {
         this.items.push(this.newItem);  // Добавляем новый элемент в список
         this.newItem = '';  // Очищаем поле ввода после добавления
         this.decury = this.items
        } else {
            this.newItem = '';
        }
    },
    redacte(item, index) {
      this.newItem = item;
      this.items.splice(index, 1);
    },
    remove(index) {
      this.items.splice(index, 1);
    },
    filterItems() {
      if (this.searchQuery !== '') {
        console.log("Фильтрация включена, поисковый запрос: ", this.searchQuery);
        const filtered = this.items.filter(item =>
          item.toLowerCase().startsWith(this.searchQuery.toLowerCase())
        );
        this.decury = filtered
        this.visible1 = true;  // Показываем фильтрованный список
        this.visible2 = false; // Скрываем полный список
        return filtered;
      } else {
        this.visible1 = false; // Скрываем фильтрованный список
        this.visible2 = true;  // Показываем полный список
        return this.items;
      }
    }
  }
}

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
input {
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
li {
  width: 100%;
  padding-left: 5px;
  cursor: pointer;
  border-bottom: 1px solid #ccc;
  white-space: normal; 
  word-wrap: break-word;
  overflow-wrap: break-word; 
}
li:hover {
  background-color: #ddd;
}
li a {
  font-size: 16px;
}
p {
  margin-top: 10px;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 18px;
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
