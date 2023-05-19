<script setup>
import { ref } from 'vue'
import  './Hero.css'
let arr = ref([])
let obj = ref({})
let putUserId = ref('')
let isOpen = ref(false)
let posttitle = '';
let postbody = '';
let title = '';
let body = '';
let api = 'https://jsonplaceholder.typicode.com/'
function getAll(){
    fetch(api + 'posts')
    .then(res => res.json())
    .then(data => arr.value = data)
}
function getbyId(id){
    fetch(api + 'posts/' + id)
    .then(res => res.json())
    .then(data => obj.value = data)
}


function byId(e){
    let id = e.target.id 
    getbyId(id)
}

async function deleteUser(id){
    let del = await fetch(api + 'posts/' + id,{
        headers: {'Content-Type' : 'application/json'},
        method: 'DELETE'
    })
    console.log(del);
    let status = del.status
    if(status == 200){
        alert('USER DELETED')
    }
}

function DELETE(e){
    let id = e.target.id
    deleteUser(id)
}

function putId(e) {
    isOpen.value = true
    putUserId.value = e.target.id
    console.log(e.target.id);
}

function PUT() {
    console.log(putUserId.value);
    let put = 'https://jsonplaceholder.typicode.com/posts/' + putUserId.value
    fetch(put ,{
        headers: {'Content-Type' : 'application/json'},
        method: 'PUT',
        body: JSON.stringify({
            title: title,
            body: body
        })
    })
    .then(res => res.json())
    .then(data => console.log(data))
}
function POST(){
    fetch( api + 'posts', {
        headers: {'Content-Type' : 'application/json'},
        method: 'POST',
        body: JSON.stringify({
            title: 'posttitle',
            body: 'postbody'
        })
    })
    .then(res => res.json())
    .then(data => console.log(data))
}
getAll()
</script>
<template>
    <section class="hero">
        <div class="container">
            <div class="hero__wrapper" >
                <div class="post">
                    <div class="post__left-wrapper">
                        <input class="post__letf-inp" type="text" v-model="posttitle">
                        <input class="post__letf-inp" type="text" v-model="postbody">
                        <button class="post__letf-btn" @click="POST">POST</button>
                    </div>
                    <div class="post__right-wrapper">
                        <button class="post__right-btn">Abduvoris</button>
                        <button class="post__right-btn">Logout</button>
                    </div>
                </div>
                <ul class="hero__list">
                    <!-- <li><h2 class="item__title">Posts</h2></li> -->
                    <li class="hero__item" @click="byId" v-for="item in arr" :key="item.id" :id="item.id">
                        <div>
                            <h2 class="item__title">{{ item.title }}</h2>
                            <p class="item__text">{{ item.body }}</p>
                        </div>
                        <div>
                            <button class="item__delete" @click="DELETE" :id="item.id">DELETE</button>
                            <button class="item__edit" @click="putId" :id="item.id">EDIT </button>
                        </div>
                    </li>
                </ul>
                <div v-if="isOpen" class="card">
                    <h1 class="card__title">Edit post</h1>
                    <input class="card__inp" type="text" placeholder="title" v-model="title">
                    <input class="card__inp" type="text" placeholder="body" v-model="body">
                    <!-- <h1>{{ obj.name }}</h1>
                        <h2>{{ obj.username }}</h2>
                        <p>{{ obj.email }}</p> -->
                        <div class="card__btn-wrapper">
                            <button class="card__cencel" @click="isOpen = false">Cencel</button>
                            <button class="card__save" @click="PUT">Save changes</button>
                        </div>  
                    </div>
                </div>
            </div>
        </section>
    </template>