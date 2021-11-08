<template>
    <form class="formulario" v-on:submit.prevent="processLogInUser">
        <h1>Login</h1>
        <div class="contenedor">    
            <p>Please enter you Login and your Password.</p>

            <div class="input-contenedor">
                <i class="fas fa-envelope icon"></i>
                <input type="text" v-model="user.username" placeholder="Username or Email">
            </div>

            <div class="input-contenedor">
                <i class="fas fa-key icon"></i>
                <input type="password" v-model="user.password" placeholder="Password"> 
            </div>

            <input type="submit" value="Login" class="button_login">
            <p>Forgot Password?</p>
        </div>

        <div class="button">
            <i class="fab fa-google"></i>
            <a class="link-sign" href="#">Sign-in with google</a>
        </div>
        <div>
        <p class="member">Not a member yet? <a class="link" href="registervista.html">Register!</a></p>
        </div>
    </form>
</template>


<script>
import axios from 'axios';

export default {
    name: "LogIn",
    
    data: function(){
        return {
            user: {
                username:"",
                password:""
            }
        }
    },

    methods: {
        processLogInUser: function(){
            axios.post(
                "https://mision-tic-bank-be.herokuapp.com/login/",
                this.user,
                {headers: {}}
                )
                .then((result) => {
                    let dataLogIn = {
                        username: this.user.username,
                        token_access: result.data.access,
                        token_refresh: result.data.refresh,
                    }

                    this.$emit('completedLogIn', dataLogIn)

                })
                .catch((error) => {
                    if (error.response.status == "401")
                        alert("ERROR 401: Credenciales Incorrectas.");
                });
        }
    }
}
</script>


<style>
* {
    box-sizing: border-box;
}
.contenedor{
    width: 100%;
    padding: 15px;
}
.formulario{
    background: none;
    margin-top: 150px;
    padding: 3px;
}
h1{
    text-align: center;
    color: #ffffff;
    font-size: 40px;
}
input[type="text"],
input[type="password"]{
    font-size: 20px;
    width: 82%;
    padding: 10px;
    border: none;
    background: none;
}
.input-contenedor{
    margin-bottom: 17px;
    border: 1px solid #ffffff;
    outline: none;
    border-radius: 10px;
}
.input-contenedor :focus{
    outline: none;
}
.icon{
    min-width: 50px;
    text-align: center;
    color: #3d3d3d;
}
.button{
    border: 1px solid #1A1B22;
    width: 100%;
    height: 60px;
    color: #ffffff;
    font-size: 20px;
    background-color:#1A1B22;
    padding: 15px 20px;
    border-radius: 15px;
    cursor: pointer;
}
.button_login {
    border: 1px solid #FF4F4F;
    width: 100%;
    color: #FF4F4F;
    font-size: 20px;
    background:  none;
    padding: 15px 20px;
    border-radius: 5px;
    cursor: pointer;
}
.button_login:hover{
    background: #FF4F4F;
    color: #ffffff;
}
.button:hover{
    background: rgb(82, 82, 82);
}

p{
    text-align: center;
    font-size:  14px;
    color: #ffffff;
}


.link-sign{
    margin-left: 100px;
    text-decoration: none;
    color: #ffffff;
    font-weight: 600;
    
}

.link{
    margin-left: 100px;
    text-decoration: none;
    color: #ffffff;
    font-weight: 600;
    
}
.link:hover{
    color: #FF4F4F;
}

@media(min-width:768px)
{
    .formulario{
        margin: auto;
        width: 500px;
        margin-top: 150px;
        border-radius: 2%;
    }
}
</style>