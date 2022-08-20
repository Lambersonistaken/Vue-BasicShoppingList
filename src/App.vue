<template>

<div class="container">

  <h3>Alışveriş Listesi</h3>
  <hr/>

  <div class="my-2">

    <input type="text" placeholder="Ne Alacaksın ?" @keydown.enter="onSave">

  </div>

  <ul v-if="itemsList.length > 0">
    <li v-for="item in itemsList" :key="item.id" class="d-flex justify-content-between align-items-center">
      <span>{{item.title}}</span>
      <button class="sm red" @click="onDelete(item)">Sil</button>
    </li>

  </ul>
  <div v-else class="bg-blue">Herhangi Bir Ürün Yoktur...</div>
<small class="text-blue d-flex text-right justify-content-end align-items-center">{{itemsList.length}} adet alınacak ürün vardır.</small>
</div>

</template>


<script>
import axios from "axios"
export default  {

data() {

  return {
    itemsList : []

  }
},
  mounted() {
    axios.get("http://localhost:3000/items").then(items_response => {
        this.itemsList = items_response.data || []
    })
  },
  methods:  {

  onSave(e){
    const saveObject = {
      title : e.target.value,
      created_at : new Date(),
      completed : false

    }

    axios.post("http://localhost:3000/items",saveObject).then(save_response => {
      console.log(save_response)
      this.itemsList.push(save_response.data)
      e.target.value = ""
      e.target.focus()
    })
  },

    onDelete(item) {
      axios.delete(`http://localhost:3000/items/${item.id}`).then(delete_response => {
      console.log(delete_response)
      this.itemsList = this.itemsList.filter(i => i.id != item.id)
      })
    }



  }

}

</script>

<style>

li{
  max-width: 92%;
}

</style>