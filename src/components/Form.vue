<template>
  <div class="input-container">
    <div><input type="datetime-local" v-model="date"></div>
    <div><input type="text" placeholder="Название фирмы" v-model="firmName"></div>
    <div><input type="text" placeholder="ФИО" v-model="name"></div>
    <div><input type="text" placeholder="Номер телефона" v-model="phone"></div>
    <div><input type="text" placeholder="Комментарий" v-model="comment"></div>
    <div><input type="text" placeholder="ATI номер" v-model="ATI"></div>
    <div><button @click="addOrder" @keyup.enter="addOrder">Добавить заказ</button></div>
  </div>
</template>

<script>
export default {
    name: 'transportationForm',
    data(){
        return{
            date: '',
            name: '',
            firmName: '',
            phone: '',
            comment: '',
            ATI: '',
            orders: eval(localStorage.getItem('orders'))
        }
    },
    methods:{
        addOrder(){
            if(!(this.date == '' || this.name == '' || this.firmName == '' || this.phone == '' || this.comment == '' || this.ATI == '')){
                this.orders.push({
                    id: this.orders[this.orders.length - 1].id + 1,
                    date: this.date,
                    firmName: this.firmName,
                    clientName: this.name,
                    phoneNumber: this.phone,
                    comment: this.comment,
                    ATI: this.ATI
                })
                localStorage.orders = JSON.stringify(this.orders)
                this.$emit('update')                    //activating an 'update' event
                this.date = ''
                this.name = ''
                this.firmName = ''
                this.phone = ''
                this.comment = ''
                this.ATI = ''
            } else {
                alert('Заполните все поля')
            }
        },

        getOrders(){
            this.orders = eval(localStorage.getItem('orders'))
        }
    },

    created(){
        this.$parent.$on('update1', this.getOrders)
    },

    watch:{
        ATI: function(newCode, oldCode){                //validating ATI
            if(newCode.length > 5 || isNaN(newCode)){
                this.ATI = oldCode
            }
        }
    }
}
</script>

<style scoped>
    .input-container{
        display: flex;
        justify-content: space-between;
        margin: 30px;
    }

    .input-container > div{
        margin: 10px;
    }
</style>