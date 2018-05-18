<template>
  <div>
      <h1 class="text-center"><a href="https://github.com/metinseylan/vue-socket.io">Vue-Socket.io</a></h1>

      <div class="container">
          <div v-if="!join" id="join" class="text-center">
              <form @submit.prevent="joinChat(name)">
                  <div class="form-group">
                      <input type="text" max="12" class="form-control input-lg text-center" v-model="name" placeholder="Name">
                  </div>
                  <button class="btn btn-primary btn-lg" type="submit">Join Chat</button>
              </form>

          </div>

          <div v-if="join" id="group">
              <div class="users">
                  <h4>Users</h4>
                  <ul class="list-unstyled">
                      <li v-for="user in users" :key='user.id'>{{user}}</li>
                  </ul>
              </div>
              <div class="chat">

                  <div class="messages">
                      <ul class="list-unstyled">
                          <li v-for="m in messages" :class="{me: (m.name === name)}" :key="m.id">
                              <div class="name">{{m.name}}</div>
                              <span>{{m.message}}</span>
                          </li>
                      </ul>
                  </div>


                  <div class="input">
                      <form @submit.prevent="send(message)">
                          <div class="input-group">
                              <input type="text" class="form-control" v-model="message" placeholder="uyudun mu?...">
                                <span class="input-group-btn">
                                  <button class="btn btn-default" type="submit">-></button>
                                </span>
                          </div>
                      </form>
                  </div>
              </div>
          

          </div>

      </div>
  </div>
</template>
<script>
import $ from 'jquery'
export default {
  data () {
    return {
      join: false,
        name: null,
        users: null,
        message: null,
        messages: {}
    }
  },
  methods: {
        joinChat: function (name) {
            if (name) {
                this.$socket.emit('join', name);
            }
        },
        send: function (message) {
            if (message) {
                this.$socket.emit('send', message);
                this.$set('message', null);
            }
        }
    },
    watch: {
        messages: function () {
            setTimeout(function () {
                $('.messages ul').scrollTop(999999999);
            }, 100)
        }
    },
    sockets: {
            users: function (users) {
                this.users=users;
            },
            joined: function () {
                this.join =true
            },
            messages: function (data) {
                this.messages=data
            },
            onmessage: function (data) {
                this.messages.push(data);
            },
            adduser: function (user) {
                this.users.push(user);
            }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body {
    color: #373a3c;
    background-color: #f9f9f9;
}

* {
    border-radius: 0 !important;
    list-style:none;
}
a {
    color: #337ab7;
    text-decoration: none;
}
.container {
    max-width: 700px;
    box-shadow: 0 2px 3px rgba(0, 0, 0, .05);
    background-color: #fff;
    border: 1px solid #eee;
    padding: 15px;
    margin: 0 auto;
}
.form-control {
    display: block;
    width: 96%;
    color: #555;
    background-color: #fff;
    background-image: none;
    border: 1px solid #ccc;
    box-shadow: inset 0 1px 1px rgba(0,0,0,.075);
    transition: border-color ease-in-out .15s,box-shadow ease-in-out .15s;
}
.input-lg {
    height: 46px;
    padding: 10px 12px;
    font-size: 18px;
    line-height: 1.3333333;
}
.text-center {
    text-align: center;
}
.users {
    border-right: 1px solid #eee;
    float: left;
    width: 140px;
}
.btn-lg {
    padding: 10px 16px;
    font-size: 18px;
    line-height: 1.3333333;
}
.btn-primary {
    color: #fff;
    background-color: #337ab7;
    border-color: #2e6da4;
}
.btn {
    display: inline-block;
    margin-bottom: 0;
    font-weight: 400;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    touch-action: manipulation;
    cursor: pointer;
    user-select: none;
    background-image: none;
    border: 1px solid transparent;
    margin-top:20px;
}
.users ul {
    height: 250px;
    overflow-y: scroll;
    margin: 0;
}

.users h4 {
    margin: 0;
    border-bottom: 1px solid #eee;
    padding-bottom: 3px;
    margin-bottom: 5px;
}

::-webkit-scrollbar {
    width: 5px;
}

::-webkit-scrollbar-thumb {
    background: #666;
}

.chat {
    width: 528px;
    float: left;
    padding-left: 15px;
    position: relative;
    height: 278px;
}

.chat .messages ul {
    height: 240px;
    margin: 0;
    overflow-y: scroll;
    padding-top: 5px;
}

.chat .messages ul li .name {
    font-size: 12px;
    font-weight: bold;
    margin-bottom: 5px;
}

.chat .messages ul li {
    margin-bottom: 10px;
}

.chat .messages ul li span {
    border-radius: 5px !important;
    background: #E0EDFF;
    padding: 5px 12px;
    font-size: 15px;
}

.chat .messages ul li.me {
    text-align: right;
    margin-right: 10px;
}

.chat .input {
    position: absolute;
    bottom: 0;
}


@media screen and (max-width: 768px) {
    body, html{
        position: relative;
        width: 100%;
        height:100%;
    }
    
    #join .btn{
        display: block;
        width: 100%;
    }

    .container {
        position: absolute;
        width: 100%;
        height: 89%;
    }

    h1{
        margin: 4% 0;
    }

    .users{
        display: none;
    }

    .chat{
        float: none;
        padding: 0;
        height: 100%;
        width: 100%;
        position: absolute;
    }

    .chat .messages{
        height: 90%;
    }

    .chat .messages ul{
        height:100%;
    }

    #group{
        height: 100%;
        position: relative;
    }
}
</style>
