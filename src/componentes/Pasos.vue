<template>
    <nb-container :style="{ backgroundColor: '#FFF'}">
        <nb-header>
            <nb-left>
                <nb-button transparent :onPress="() => this.props.navigation.goBack()">
                    <nb-icon name="arrow-back" :style="{ color: 'orange'}" />
                </nb-button>
            </nb-left>
            <nb-body >
                <nb-title :style="{ color: 'orange'}">{{recetaInfo.nombre}}</nb-title>
            </nb-body>
            <nb-right >
                <nb-button class="cabezera-derecha">
                    <nb-thumbnail small :source="logotipo" />
                </nb-button>
            </nb-right>
        </nb-header>
        <nb-content>
            <view :style="{ margin:20}">

                <image :source="{uri: recetaInfo.foto}" class="card-item-image" />

                <nb-button transparent >
                    <text class="orden" :style="{color:'orangered'}">{{orden}}</text>
                </nb-button>

                <text class="texto_instruccion"> {{pasos.instruccion}} </text>


                <text  class="texto_tiempo">TIEMPO</text>
                <text class="tiempo" v-if="segundos<10" >{{minutos}}:0{{segundos}} </text>
                <text class="tiempo" v-else>{{minutos}}:{{segundos}} </text>




                <nb-button  v-if="jugando==false" rounded class="boton" :onPress="comenzar">
                    <nb-text>COMENZAR</nb-text>
                </nb-button>

                <nb-button v-else rounded class="boton" :onPress="finalizar">
                    <nb-text>Ya Acabe  !!</nb-text>
                </nb-button>

            </view>
        </nb-content>

        <nb-footer>
            <nb-body>
                <nb-row  :style="{ justifyContent: 'center',alignItems: 'center'}" v-if="orden != recetaInfo.numero_pasos">
                    <nb-button :style="{ backgroundColor: 'orange', marginRight:10,padding:1}" :onPress="pasoAnterior">
                        <nb-icon active name="arrow-back"  :style="{ color: 'white'}" />
                    </nb-button>

                    <nb-button  :style="{ backgroundColor: 'orange', marginLeft:10}" :onPress="siguientePaso">
                        <nb-icon active name="arrow-forward"  :style="{ color: 'white'}" />
                    </nb-button>
                </nb-row>
                <nb-row :style="{ justifyContent: 'center',alignItems: 'center'}" v-else>
                    <nb-button :style="{ backgroundColor: 'orange', marginRight:10,padding:1}" :onPress="pasoAnterior">
                        <nb-icon active name="arrow-back"  :style="{ color: 'white'}" />
                    </nb-button>
                    <nb-button  :style="{ backgroundColor: 'orange', marginLeft:10}" :onPress="() => this.props.navigation.navigate('Recetas')">
                        <nb-icon active name="home"  :style="{ color: 'white'}" />
                    </nb-button>
                </nb-row>
            </nb-body>
        </nb-footer>
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
                recetaInfo:{},
                orden: 1,
                minutos: 0,
                segundos: 0,
                numPasos: 1,
                jugando: false,

            }
        },
        mounted() {
            this.getPaso();
            this.getRecetas();
            this.jugando = false;
        },
        methods:
            {
                tick() {
                     if(this.segundos == 0)
                    {
                        if(this.segundos == 0 && this.minutos == 0)
                        {
                            this.parar();
                            this.enviarMensaje();
                            this.minutos = this.pasos.duracion;
                            this.segundos = 0;
                        }
                        else
                        {
                            console.log("seggundos igual a 0");
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
                siguientePaso: function()
                {
                    this.numPasos = this.recetaInfo.numero_pasos;
                    if(this.orden < this.numPasos)
                    {
                        this.orden = this.orden + 1;
                        this.getPaso();
                    }
                },
                pasoAnterior: function()
                {
                   if(this.orden>1)
                   {
                       this.orden = this.orden - 1;
                       this.getPaso();
                   }

                },
                finalizar()
                {
                    this.jugando = false;
                    this.minutos = 0;
                    this.segundos = 1;
                },
                enviarMensaje: function() {
                    var msj = 'El tiempo ha finalizado..¡¡¡ vaya al paso ' + (this.orden +1)+' de la receta '+ this.recetaInfo.nombre;
                    var url = 'https://proyectobackend.herokuapp.com/api/enviar';
                    axios.post(url, {
                        mensaje: msj
                    })
                        .then(function (response) {
                            console.log("enviado");
                        })
                        .catch(function (error) {
                            console.log(error);
                        });
                },
                getPaso: function() {

                    var url = 'https://proyectobackend.herokuapp.com/api/obtenerPasos/'+this.RecetaSlug+'/'+this.orden;
                    var th = this;

                    axios.get(url)
                        .then(function (response) {
                            th.pasos = response.data[0];
                            th.minutos = th.pasos.duracion;
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
    .cabezera-derecha
    {
        background-color: orange;
        border-bottom-left-radius: 25;
    }
    .orden
    {
        border-color: orangered;
        border-width: 1;
        fontWeight: bold;
        fontSize: 16;
        padding: 10;
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
        justify-content: flex-start;
        margin-top: 15;
    }
    .tiempo
    {
        color:gray;
        fontWeight: bold;
        fontSize: 18;
        justify-content: flex-start;
    }
    .texto_instruccion
    {
        color: gray;
        fontSize: 18;
    }

</style>