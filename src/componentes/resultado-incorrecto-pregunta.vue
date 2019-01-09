<template>
    <nb-container :style="{ backgroundColor: '#fff'}">
        <nb-content>
            <nb-list>
                <nb-list-item>
                    <nb-text>Simon Mignolet</nb-text>
                </nb-list-item>
            </nb-list>
        </nb-content>
    </nb-container>
   <!-- <nb-container :style="{ backgroundColor: '#FFA500'}">
    <view :style="{ flex:1,justifyContent: 'center',alignItems: 'center'}">
        <text class="titulo">{{preg.descripcion}}</text>
        <text class="mensaje1">LO SIENTO</text>
        <text class="mensaje2">¡PERDISTE!</text>

        <image :source="{uri: 'https://firebasestorage.googleapis.com/v0/b/sistemacalificaciones-78da3.appspot.com/o/puntos.JPG?alt=media&token=c667bbfa-bca0-4e53-8154-d3b9b13d5d23'}" class="card-item-image" />

        <nb-button rounded class="boton" :onPress="irPreguntas">
            <nb-text>SIGUIENTE</nb-text>
        </nb-button>
    </view>
    </nb-container>-->
</template>

<script>

    import axios from 'axios';
    export default {
        props: {
            navigation: {
                type: Object
            }
        },
        mounted()
        {
            this.getPregunta();
        },
        data(){
            return {
                preg: [],

                vue:{
                    exp:null,
                    resp:'INCORRECTO',

                }


            }
        },

        methods: {
            getPregunta: function() {
                //var urlPregunta = 'https://proyectobackend.herokuapp.com/api/pregunta';
                var urlPregunta = 'https://reddit.com/r/aww.json';
                axios.get(urlPregunta)
                    .then(function (response) {
                        this.preg =response.data.data;
                        // console.log(response.data);
                        console.log("PREGUNTA")
                      //  console.log(this.preg );
                    })
                    .catch(function (error) {
                        console.log(error);
                    });


            },

            irPreguntas: function()
            {
                this.navigation.navigate("Juegos");
            }

        }
    }

   /* export default {
        props: {
            navigation: {
                type: Object
            }
        },
        name: "resultado-preguntas",
        methods:
        {
            irPreguntas: function()
            {
                this.navigation.navigate("Juegos");
            }
        }

    }*/
</script>

<style>
    .card-item-image {
        height: 75;
        width: 75;
        margin-top: 20;
        margin-bottom: 20;
        borderRadius: 15;
        align-self: center;
    }
.titulo
{
    color: orangered;
    fontWeight: bold;
    fontSize: 30;
    margin-top: 20;
    margin-bottom: 20;
    textAlign: center;
}
.mensaje1
{
    color: white;
    fontWeight: bold;
    fontSize: 20;
    textAlign: center;
}
.mensaje2
{
    color: white;
    fontWeight: bold;
    fontSize: 30;
    textAlign: center;
}
.boton
{
    background-color: orangered;
    align-self: center;
    margin-top: 20;
}

</style>