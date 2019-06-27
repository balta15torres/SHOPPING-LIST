<template>
  <div class="TodoList">
    <h1>SHOPPING LIST</h1>

    <div class="TodoList__input">
      <input
        type="text"
        placeholder=" add name of item"
        key="newItem-input"
        v-model="newItem"
        @keyup.enter="handleAdd"
      >
      <button @click="handleAdd()">
        <img src="../../public/icons/add.svg" alt>
      </button>
    </div>
    <hr>
    <div class="TodoList__list" v-for="item in itemsFiltered" :key="item.id">
      <li>
        <input type="checkbox" v-model="item.complete">
        <p :class="{ complete : item.complete == true}">{{item.name}}</p>
      </li>
      <button class="TodoList__list--btnEdit" v-if="!item.editing" @click="editItem(item)">
        <img src="../../public/icons/pencil-edit-button.svg" alt>
      </button>
      <input
        v-else
        type="text"
        v-model="item.name"
        @keyup.enter="doneEditItem(item)"
        @blur="doneEditItem(item)"
        @keyup.esc="beforeEditItem(item)"
      >
      <div class="TodoList__btnsList">
        <button @click="handleRemove(item)">
          <img src="../../public/icons/delete.svg" alt>
        </button>
        <!-- <ModalComponent
          v-if="showModal"
          @acceptModal="removeItem(item)"
          @closeModal="closeModal"
          @handleAdd="handleAdd()"
          :text="modalText"
        />-->
        <ModalComponent
          v-if="showModal"
          @acceptModal="removeItem(item)"
          @closeModal="closeModal"
          @handleAdd="handleAdd()"
          :text="modalText"
        >
          <template #actions>
            <button @click="closeModal()" v-if="isAdding">
              <img src="../../public/icons/check.svg" alt>
            </button>
            <div v-else>
              <button @click="removeItem(item)">
                <img src="../../public/icons/delete.svg" alt>
              </button>
              <button @click="closeModal()">
                <img src="../../public/icons/close.svg" alt>
              </button>
            </div>
          </template>
        </ModalComponent>
      </div>
    </div>
    <div v-if="shoppinglist.length >0 ">
      <hr>
      <div class="TodoList__count">
        <label>
          <input type="checkbox" :checked="!anyRemainingItem" @change="checkAllItems"> check All
        </label>
        <br>
        <p>{{totalItem}} total items</p>
        <p>{{remainingItem}} items left</p>
      </div>
      <hr>

      <div class="TodoList__btns">
        <button :class="{ active:filter == 'all'}" @click="filter = 'all'">All</button>
        <button :class="{ active:filter == 'active'}" @click="filter = 'active'">Active</button>
        <button :class="{ active:filter == 'complete'}" @click="filter = 'complete'">Completed</button>
      </div>
    </div>
    <PoputComponent v-else text=" add item in input !!"/>
    <hr>
  </div>
</template>
<script>
import ModalComponent from "./ModalComponent";
import PoputComponent from "./PoputComponent";

const actionsTextMapping = {
  delete: "Are you sure you want to delete it?",
  add: "add items to your list!"
};

export default {
  name: "TodoList",

  components: {
    ModalComponent,
    PoputComponent
  },
  data() {
    return {
      idItem: 0,
      newItem: "",
      beforeEditItemCache: "",
      filter: "all",
      showModal: false,
      currentItem: null,
      shoppinglist: [],
      currentAction: null
    };
  },
  computed: {
    isAdding() {
      return this.currentAction === "add";
    },

    totalItem() {
      return this.shoppinglist.length;
    },
    remainingItem() {
      return this.shoppinglist.filter(item => !item.complete).length;
    },
    anyRemainingItem() {
      return this.remainingItem != 0;
    },
    itemsFiltered() {
      if (this.filter == "all") {
        return this.shoppinglist;
      } else if (this.filter == "active") {
        return this.shoppinglist.filter(item => !item.complete);
      } else if (this.filter == "complete") {
        return this.shoppinglist.filter(item => item.complete);
      }
      return this.shoppinglist;
    },
    modalText() {
      return actionsTextMapping[this.currentAction];
    }
  },
  methods: {
    addItem() {
      // if (this.newItem.trim().length == 0) this.handleAdd();
      this.shoppinglist.push({
        id: Math.random(),
        name: this.newItem,
        editing: false,
        complete: false
      });
      this.cleanInput();
    },
    removeItem() {
      this.shoppinglist = this.shoppinglist.filter(
        it => it.id !== this.currentItem
      );

      this.closeModal();
    },
    editItem(item) {
      this.beforeEditItemCache = item.name;
      item.editing = true;
    },
    doneEditItem(item) {
      if (item.name.trim() == "") item.name = this.beforeEditItemCache;
      item.editing = false;
    },
    beforeEditItem(item) {
      item.name = this.beforeEditItemCache;
      item.editing = false;
    },
    checkAllItems() {
      this.shoppinglist.forEach(item => (item.complete = event.target.checked));
    },
    openModal() {
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.currentItem = null;
    },
    handleRemove(item) {
      this.currentItem = item.id;
      this.currentAction = "delete";
      this.openModal();
    },
    cleanInput() {
      this.newItem = "";
    },
    handleAdd() {
      this.currentAction = "add";
      if (this.newItem.trim().length == 0) this.openModal();
      else this.addItem();
    }
  }
};
</script>
<style lang="scss">
.TodoList {
  h1 {
    display: flex;
    justify-content: center;
    align-content: center;
  }
  width: 500px;
  .TodoList__input {
    margin: 20px;
    display: flex;
    justify-content: center;
    height: 30px;
    button {
      margin-left: 10px;
      background-color: #42b983;
      border-radius: 5px;
      
      img {
        width: 15px;
        margin-top: 3px;
      }
    }
  }
  .TodoList__list {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 50px;
    li {
      display: flex;
      align-items: center;
      width: 400px;
      font-size: 20px;
      input {
        width: 10px;
        margin-right: 15px;
        margin-bottom: 10px;
        background: #42b983;
      }
    }
    .TodoList__list--btnEdit {
      position: absolute;
      margin-left: 463px;
      border-radius: 5px;
      background-color: #42b983;
      width: 35px;
      height: 35px;

      img {
        width: 15px;
        margin-top: 3px;
      }
    }
  }
  .TodoList__btnsList {
    width: 20%;
    button {
      background-color: #35495e;
      border-radius: 5px;
      width: 35px;
      height: 35px;
      margin:15px;
      img {
        width: 15px;
        margin-top: 3px;
      }
    }
  }
}
.TodoList__count {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #35495e;
  input {
    width: 10px;
    margin-right: 15px;
    margin-bottom: 10px;
  }
}
.TodoList__btns {
  margin: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  .active {
    background-color: #42b983;
    color: black;
  }
  button {
    border-radius: 5px;
    background-color: #35495e;
    color: #42b983;
    margin-right: 15px;
    height: 35px;
    
  }
}
.complete {
  text-decoration: line-through;
  color: rgb(139, 1, 1);
}
</style>



