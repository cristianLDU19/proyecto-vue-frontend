<template>
    <nb-container :style="{ backgroundColor: '#FFFFFF'}">
        <nb-header>
            <nb-left>
                <nb-button transparent :onPress="() => this.props.navigation.navigate('Recetas')">
                    <nb-icon name="arrow-back" :style="{ color: 'orange'}" />
                </nb-button>
            </nb-left>
            <nb-body>
                <nb-title :style="{ color: 'orange'}">{{recetaInfo.nombre}}</nb-title>
            </nb-body>
            <nb-right >
                <nb-button class="cabezera-derecha">
                    <nb-thumbnail small :source="logotipo" />
                </nb-button>
            </nb-right>
        </nb-header>
        <nb-content padder>
            <text class="texto">{{recetaInfo.nombre}}</text>
            <image :source="{uri: recetaInfo.foto}" class="card-item-image" />
            <text class="titulo">INGREDIENTES:</text>
            <text class="texto">-------------</text>
            <nb-list>
                <nb-list-item v-for="ingrediente in ingredientes">
                    <nb-text>{{ingrediente.cantidad}} {{ingrediente.nombre}}</nb-text>
                </nb-list-item>
            </nb-list>


            <nb-button rounded class="boton" :onPress="() => {
                            this.props.navigation.navigate('Pasos', {
                              nombreReceta: recetaInfo.slug,
                            });
                            }">
                <nb-text>COMENZAR</nb-text>
            </nb-button>
            </nb-content>
    </nb-container>
</template>

<script>
    import axios from 'axios';
    import logo from "../../assets/usuario.png"

    export default {
        props: {
            navigation: {
                type: Object
            }
        },


        name: "detalle-receta",

        data(){
            return {
                ingredientes: [],
                recetaInfo: {},
                logotipo: logo,
                RecetaSlug: this.navigation.getParam('nombreReceta', 'SIN RECETA'),
            }
        },
        mounted() {
           // this.getNombreRecetas();
            this.getIngredientes();
            this.getRecetas();
            console.log(this.RecetaSlug);

        },
        methods: {
            getIngredientes: function() {
                var url = 'https://proyectobackend.herokuapp.com/api/buscar/'+this.RecetaSlug;
                var th = this;

                axios.get(url)
                    .then(function (response) {
                        th.ingredientes =response.data;
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
                            // console.log(response.data);
                            //console.log("Rceta")
                            //console.log(this.recetaInfo );
                        })
                        .catch(function (error) {
                            console.log(error);
                        });
                },

            }
            /*getNombreRecetas: function() {
                var cadena = document.getElementById("titulo_pag").textContent;
                cadena= cadena.toLowerCase();
                this.receta.nombre =cadena;
            }*/

        };

</script>

<style>
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
    .titulo
    {
        color: orangered;
        fontWeight: bold;
        fontSize: 24;
        margin-top: 5;
        margin-bottom: 0;
        margin-left: 10;
        textAlign: left;

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
    .boton
    {
        background-color: orangered;
        align-self: center;
        margin-top: 20;
    }

</style>