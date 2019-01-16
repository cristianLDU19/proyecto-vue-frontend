<template>
    <nb-container :style="{ backgroundColor: '#FFF'}">
        <nb-header>
            <nb-left>
                <nb-button transparent :onPress="() => this.props.navigation.navigate('Recetas')">
                    <nb-icon name="arrow-back" :style="{ color: 'orange'}" />
                </nb-button>
            </nb-left>
            <nb-body >
                <nb-title :style="{ color: 'orange'}">CARRERA DEL CHEF</nb-title>
            </nb-body>
            <nb-right >
                <nb-button class="cabezera-derecha">
                    <nb-thumbnail small :source="logotipo" />
                </nb-button>
            </nb-right>
        </nb-header>
        <nb-content>
            <view :style="{ flex:1,justifyContent: 'center',alignItems: 'center',margin:10}">

                <text class="texto"> {{recetaInfo.nombre}} </text>

                <image :source="{uri: recetaInfo.foto}" class="card-item-image" />

                <nb-button transparent :style="{marginBottom:10}">
                    <nb-text class="orden" :style="{color:'orangered'}">{{orden}}</nb-text>
                    <text :style="{color:'gray'}"> {{pasos.instruccion}} </text>
                </nb-button>

                <view :style="{ backgroundColor: 'lightgray'}">
                    <text  class="texto_tiempo">TIEMPO RESTANTE</text>
                    <text class="tiempo" v-if="segundos<10" >{{minutos}}:0{{segundos}} </text>
                    <text class="tiempo" v-else>{{minutos}}:{{segundos}} </text>
                </view>

                <nb-button v-if="jugando==false" rounded class="boton" :onPress="comenzar">
                    <nb-text>COMENZAR</nb-text>
                </nb-button>

                <nb-button v-else rounded class="boton" :onPress="finalizar">
                    <nb-text>Ya Acabe  !!</nb-text>
                </nb-button>

            </view>
        </nb-content>

    </nb-container>

</template>

<script>
    import axios from 'axios';
    import logo from "../../assets/usuario.png";

    export default {
        props: {
            navigation: {
                type: Object
            }
        },
        data(){
            return {

                RecetaSlug: this.navigation.getParam('nombreReceta', 'SIN RECETA'),
                logotipo: logo,
                pasos: {},
                recetaInfo: {},
                orden: this.navigation.getParam('orden', 1),
                minutos: 0,
                segundos: 0,
                jugando: false,
                numPasos: 0

            }
        },
        mounted() {
            this.getPaso();
            this.getRecetas();
            this.jugando  = false;

        },
        methods:
            {
                irRetoUno: function()
                {
                    this.numPasos = this.recetaInfo.numero_pasos;
                    console.log(this.orden);
                    console.log(this.numPasos);
                    if(this.orden < this.numPasos)
                    {
                        this.orden = this.orden + 1;
                        this.navigation.navigate("RetoUno", {orden: this.orden, nombreReceta: this.recetaInfo.slug});
                    }

                },
                tick() {
                     if(this.segundos == 0)
                    {
                        if(this.segundos == 0 && this.minutos == 0)
                        {
                            this.parar();
                            this.irRetoUno();
                        }
                        else
                        {
                            this.minutos = this.minutos -1;
                            this.segundos = 59;
                        }
                    }
                    else
                     {
                         this.segundos = this.segundos - 1
                     }
                    },
                comenzar()
                    {
                        this.interval = setInterval(() => this.tick(), 1000);
                        this.jugando = true;
                    },
                parar()
                {
                    clearInterval(this.interval);
                },
                finalizar()
                {
                    this.jugando = false;
                    this.minutos = 0;
                    this.segundos = 1;
                },

                getPaso: function() {
                    var url = 'https://proyectobackend.herokuapp.com/api/obtenerPasos/'+this.RecetaSlug+'/'+this.orden;
                    var th = this;

                    console.log(url);
                    axios.get(url)
                        .then(function (response) {
                            th.pasos = response.data[0];
                            th.minutos = th.pasos.duracion;
                            console.log(th.pasos);
                        })
                        .catch(function (error) {
                            console.log(error);
                        });

                },
                getRecetas: function() {
                    var url = 'https://proyectobackend.herokuapp.com/api/receta/'+this.RecetaSlug;
                    var th = this;
                    axios.get(url)
                        .then(function (response) {
                            th.recetaInfo =response.data[0];
                        })
                        .catch(function (error) {
                            console.log(error);
                        });
                },
            }

        };




</script>

<style scoped>
    .card-item-image {
        height: 200;
        width: 200;
        margin-top: 10;
        margin-bottom: 10;
        borderRadius: 15;
        align-self: center;
    }
    .texto
    {
        color: orangered;
        fontWeight: bold;
        fontSize: 24;
        margin-top: 0;
        margin-left: 10;
        margin-bottom: 5;
        textAlign: center;
    }
    .cabezera-derecha
    {
        background-color: orange;
        border-bottom-left-radius: 25;
    }
    .orden
    {
        border-color: orangered;
        border-width: 1;
    }
    .boton
    {
        background-color: orangered;
        align-self: center;
        margin-top: 20;
    }
    .texto_tiempo
    {
        color:orangered;
        fontWeight: bold;
        fontSize: 18;
    }
    .tiempo
    {
        color:gray;
        fontWeight: bold;
        fontSize: 18;


    }

</style>