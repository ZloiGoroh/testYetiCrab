<template>
  <div class="table-container">
    <div class="row-container">
      <div class="id"> ID</div>
      <div class="date">Время получения завки</div>
      <div class="firm">Название фирмы</div>
      <div class="client">ФИО клиента</div>
      <div class="number">Номер телефона</div>
      <div class="comment">Комментарий</div>
      <div class="ati">ATI код</div>
      <div class="buttons"></div>
    </div>

    <div class="row-container" v-for="order in orders" :key="order.id">
      <div class="id">
        {{order.id + 1}}
      </div>
      <div class="date">
        <span v-if="edit[orders.indexOf(order)]">
          <input type="datetime-local" v-model="changedOrders[orders.indexOf(order)].date" />
        </span>
        <span v-else>
          {{validateTime(order.date)}}
        </span>
      </div>
      <div class="firm">
        <span v-if="edit[orders.indexOf(order)]">
          <input type="text" placeholder="Название фирмы" v-model="changedOrders[orders.indexOf(order)].firmName">
        </span>
        <span v-else>
          {{order.firmName}}
        </span>
      </div>
      <div class="client">
        <span v-if="edit[orders.indexOf(order)]">
          <input type="text" placeholder="ФИО" v-model="changedOrders[orders.indexOf(order)].clientName">
        </span>
        <span v-else>
          {{order.clientName}}
        </span>
      </div>
      <div class="number">
        <span v-if="edit[orders.indexOf(order)]">
          <input type="text" placeholder="Номер телефона" v-model="changedOrders[orders.indexOf(order)].phoneNumber">
        </span>
        <span v-else>
          {{order.phoneNumber}}
        </span>
      </div>
      <div class="comment">
        <span v-if="edit[orders.indexOf(order)]">
          <input type="text" placeholder="Комментарий" v-model="changedOrders[orders.indexOf(order)].comment">
        </span>
        <span v-else>
          {{order.comment}}
        </span>
      </div>
      <div class="ati"> 
        <span v-if="edit[orders.indexOf(order)]">
          <input type="text" placeholder="ATI номер" v-model="changedOrders[orders.indexOf(order)].ATI">
        </span>
        <span v-else>
          <a :href="createLink(order.ATI)">{{order.ATI}}</a>
        </span>
      </div>
      <div class="buttons">
        <button @click="deleteOrder(orders.indexOf(order))">Удалить</button> 
        <button v-if="!edit[orders.indexOf(order)]" @click="editOrder(orders.indexOf(order))">Изменить</button>
        <button v-else @click="saveChanges">Сохранить</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'fullTable',
    data(){
      return{
        orders: [],
        edit: [],
        changedOrders: []
      }
    },
    methods:{
      createLink(num){
        return 'https://ati.su/firms/' + num + '/info/';
      },

      validateTime(date){
        return date.slice(0, 10) + ' В ' + date.slice(11, 16)
      },

      getOrders(){
        this.orders = eval(localStorage.getItem('orders'))
        this.changedOrders = this.orders
        this.nullifyEdits()
      },

      nullifyEdits(){
        this.edit = []
        for (let i in this.orders){
          this.edit[i] = false
        }
      },

      editOrder(orderNumber){
        this.nullifyEdits()
        this.changedOrders = eval(localStorage.getItem('orders'))
        this.edit[orderNumber] = true
      },

      deleteOrder(id){
        this.orders.splice(id, 1)
        localStorage.setItem('orders', JSON.stringify(this.orders))
        this.nullifyEdits()
      },

      saveChanges(){
        localStorage.setItem('orders', JSON.stringify(this.changedOrders)) 
        this.getOrders()
        this.nullifyEdits()
      }
    }, 


    created(){
      this.getOrders()
      this.$parent.$on('update', this.getOrders)
    },
    beforeUpdate(){
      this.$emit('update1')
    }
  }
</script>

<style scoped>
  .table-container{
    width: 1200px;
  }

  .row-container{
    width: 100%;
    display: flex;
    justify-content: center;
    /*margin-bottom: 15px;*/
  }

  .row-container:first-child{
    margin-bottom: 20px;
  } 

  .row-container > div{
    border: 1px solid black;
    box-sizing: content-box;
    padding: 2px;
  }

  .id{
    width: 2%;
    text-align: center;
  }

  .date{
    width: 15%;
  }

  .firm{
    width: 15%
  }

  .client{
    width: 20%;
  }

  .number{
    width: 8%;
  }

  .comment{
    width: 20%;
  }

  .ati{
    width: 5%;
    text-align: center;
  }

  .buttons{
    width: 100px;
    padding: 0 !important;
    border: none !important;
  }

  .buttons > button{
    width: 100px;
    margin-bottom: 10px;
  }

  input{
    width: 100%;
  }
</style>
