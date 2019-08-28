<template>
  <div>
    <!-- <form @submit.prevent="add">
        <input type="hidden" v-model="form.id">
        <input type="text" v-model="form.name">
        <button type="submit" v-show="!updateSubmit">add</button>  
        <button type="button" v-show="updateSubmit" @click="update(form)">Update</button> 
    </form>
    <ul v-for="user in users" :key="user.idd">
      <li>
      <span>{{user.name}}</span> &#160;
      <button @click="edit(user)">Edit</button> ||  <button @click="del(user)">Delete</button>
      </li>
    </ul> -->
    <form @submit.prevent="add">
        <input type="text" v-model="form.count">
        <input type="text" v-model="form.param">
        <button type="submit" v-show="!updateSubmit">add</button>  
    </form>
    <center>
        <h1>Count : {{count}}</h1>
        <h1>Progress : {{total}}</h1>
    </center>
  </div>
</template>

<script>
/* eslint-disable */ 
import axios from 'axios'
import Pusher from 'pusher-js' 

export default {
  data(){
    return{
        form: {
          count: '',
          param: ''
        },
        count: '',
        total: null,
        updateSubmit: false
    }
  },
  mounted() {
    this.subscribe()
  },
  methods: {
    add(){
        axios({
            method: 'post',
            url: 'http://localhost:8000/api/count',
            data: {
                count: this.form.count,
                param: this.form.param
            },
            headers: {
                'Access-Control-Allow-Origin': '*',
                'Content-Type': 'application/json'
            }
        }).then(res => {
            console.log(res.data.data)
            this.count = res.data.data
            // this.form.count = ''
            // this.form.param = ''
        })
    },
    subscribe () {
      let pusher = new Pusher('88b5cf872e5704525dc8', { cluster: 'ap1' })
      var vm = this
      pusher.subscribe('my-channel')
      pusher.bind('my-event', data => {
          console.log(data.data)
          vm.total = data.data
        // this.mockReviews.unshift(data.review)
      })
    }
  }
}
</script>