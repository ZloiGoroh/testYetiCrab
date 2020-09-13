<template>
  <div id="app" class="wrapper">
    <transportationForm @update="$emit('update')"/>
    <fullTable @update1="$emit('update1')" /> 
  </div>
</template>

<script>
import fullTable from './components/Table.vue';
import transportationForm from './components/Form';

export default {
  name: 'App',
  components: {
    fullTable,
    transportationForm
  },
  data(){
    return{
      orders: eval(localStorage.getItem('orders'))
    }
  },
  created(){
      if (localStorage.length === 0){
        localStorage.setItem('orders', JSON.stringify([
          {
            id: 0,
            date: '2019-08-18T13:30',
            firmName: 'ООО что-то холдинг',
            clientName: 'Иванов Иван Иванович',
            phoneNumber: '+79090012345',
            comment: 'Просто сделайте всё красиво',
            ATI : '11111'
          },
          {
            id: 1,
            date: '2012-10-07T14:27',
            firmName: 'ООО что-то ещё холдинг',
            clientName: 'Васильев Иван Иванович',
            phoneNumber: '+79090012345',
            comment: 'Просто сделайте всё красиво',
            ATI : '12345'
          },
          {
            id: 2,
            date: '2019-08-18T13:30',
            firmName: 'ООО какие люди',
            clientName: 'Васильев Василий Иванович',
            phoneNumber: '+79090012345',
            comment: 'Просто сделайте всё красиво',
            ATI : '23456'
          },
          {
            id: 3,
            date: '2019-08-18T13:30',
            firmName: 'ООО ААА',
            clientName: 'Васильев Василий Васильевич',
            phoneNumber: '+79090012345',
            comment: 'Просто сделайте всё красиво',
            ATI : '32571'
          },
        ]))
      } 
    }
}
</script>

<style scoped>
  .wrapper{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    flex-direction: column;
  }
</style>
