<template>
    <div id="app">
        <img alt="Vue logo" src="./assets/logo.png">
        <HelloWorld msg="Welcome to Your Vue.js App" :toggle="toggle" />
        <div>
            <input v-model="item" />
            <button @click="addItem()">
                Add element
            </button>

            <ul v-if="show">
                <li v-for="(msg,index) in messages" :key="index">{{ msg.title.title }}</li>
            </ul>
        </div>
    </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import axios from "axios";

const apiFtUrl = process.env.FT_API;
 
    export default {
        name: 'App',
        components: {
            HelloWorld
        },
        data() {
            return {
                messages: [],
                show: false,
                item: ''
            }
        },
        methods: {  
            async toggle() {
                //const callPromise = () => new Promise(resolve => setTimeout(() => resolve(), 1000));
                //await callPromise();
                //this.show = !this.show;

                const { data } = await axios.post(apiFtUrl, {
                    queryString: "banks",
                    resultContext: {
                       aspects: ['title', 'lifecycle', 'location', 'summary', 'editorial']
                    }
                }, {
                    headers: {
                       'Content-Type': 'application/json',
                       'X-Api-Key': process.env.API_KEY
                    }
                });
                this.messages = data.results[0].results;
            },

            addItem() {
                if (this.item.trim()) {
                    this.messages.push(this.item);
                }
            }

        }
    }
  


</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
  display: block;
}
</style>
