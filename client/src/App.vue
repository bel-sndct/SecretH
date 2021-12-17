<template>
<div id="app">

    <div v-show="lobby" class="lobby">

        <div class="container">
            <div class="newRoom">
                <h4>Create a new game</h4>
                <div class="inputNewRoom">
                    <form onsubmit="return false">
                        <input v-model="username" type="text" placeholder="Enter Username" class="inputForm">
                        <button @click="createGame(username)" class="inputForm">Create</button>
                    </form>
                </div>
            </div>

            <div class="joinRoom">
                <h4>Or join an existing game</h4>
                <div class="inputJoinRoom">
                    <form onsubmit="return false">
                        <input v-model="username" type="text" placeholder="Enter Username" class="inputForm">
                        <input v-model="roomID" type="text" placeholder="room ID here" class="inputFormID">
                        <button @click="joinGame(username, roomID)" class="inputForm">Join</button>
                    </form>
                </div>
            </div>

        </div>
    </div>

    <div v-show="game" class="game">
        <div class="columns top-half" style="margin-top: 0px;">
            <div class="column is-three-fifths">
                <Board />
            </div>
            <div id="chat" class="column">
                <Chat :username="username" />
            </div>
        </div>

        <div class="columns">
          <div class="column">
              <Player :user="username" />
          </div>
        </div>
    </div>

</div>
</template>

<script>
import Board from './components/Board.vue';
import Player from './components/Player.vue';
import Chat from './components/Chat.vue';

import socket from '../src/socket';

export default {
    name: 'App',
    components: {
        Board,
        Player,
        Chat
    },
    data() {
        return{
            lobby: true,
            game: false,
            username: null,
            roomID: null,
            admin: false
        }
    },
    created () {  
        socket.on('newGame', (roomID) => {
            this.roomID = roomID;
        });
    },
    methods: {
        createGame (username) {
            if (username != null) {
                socket.emit('createGame',{username:username});
                this.lobby = false
                this.game = true
                this.username = username
                this.admin = true
            }
        },
        joinGame (username, roomID) {
            if (username != null && roomID != null) {
                socket.emit('joinGame',{username:username, roomID:roomID});
                this.lobby = false
                this.game = true
                this.username = username
                this.roomID = roomID
            }
        }
    }
}
</script>

<style scoped>

#app {
    background-image: url("./assets/background_img.png");
    font-weight: 1000;
    color: black;
    font-family: 'Trebuchet MS';
}

.columns {
    height: 50vh;
}

.column {
    border: 2px solid black;
    /*margin-left: 20%;*/
}

#chat {
    /* overflow-y: auto;
    word-break: break-word; */
}

.container {
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    height: 100%;
}

.newRoom {
    position: absolute;
    text-align: center;
    margin: 0 auto;
    left: 0;
    right: 0;
}

.joinRoom {
    margin-top: 20%;
}

.inputForm {
    height: 40px;
    font-size: 15px;
    font-weight: bold;
    background-color: #FFEBCD;
}

.inputFormID {
    height: 40px;
    width: 110px;
    font-size: 15px;
    font-weight: bold;
    background-color: #FFEBCD;
}

.lobby {
    height: 100vh;
    background-color: #f2654b;
    background-image: url("./assets/banner.png");
    background-repeat: no-repeat;
    background-position: top;
    
    font-size: 2.5vw;
    font-family: Monotype Corsiva;
    color: #F5DEB3;
}

</style>