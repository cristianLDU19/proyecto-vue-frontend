<template>
    <nb-container :style="{ backgroundColor: '#FFFFFF'}">
        <nb-header>
            <nb-left>
                <nb-button transparent :onPress="() => this.props.navigation.navigate('Recetas')">
                    <nb-icon name="arrow-back" :style="{ color: 'orange'}" />
                </nb-button>
            </nb-left>
            <nb-body>
                <nb-title :style="{ color: 'orange'}">CARRERA DEL CHEF</nb-title>
            </nb-body>
            <nb-right >
                <nb-button class="cabezera-derecha">
                    <nb-thumbnail small :source="logotipo" />
                </nb-button>
            </nb-right>
        </nb-header>
        <nb-content padder>

            <text class="texto"> SELECCIONE UNA RECETA </text>

            <scroll-view>
                <nb-list>
                    <nb-list-item v-for="receta in recetas" :onPress="() => {
                            this.props.navigation.navigate('CarreraChef', {
                              nombreReceta: receta.slug ,
                            });
                            }">
                        <nb-thumbnail small :source="{uri: receta.foto}" />
                        <nb-text>  {{receta.nombre}}</nb-text>
                    </nb-list-item>
                </nb-list>
            </scroll-view>

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
        data(){
            return {
                recetas: {},
                logotipo: logo,
            }
        },
        mounted() {
            console.log(this.recetas);
            this.getRecetas();
            console.log(this.recetas);
        },
        methods: {
            getRecetas: function() {
                var url = 'https://proyectobackend.herokuapp.com/api/receta/';
                var th = this;
                axios.get(url)
                    .then(function (response) {
                        th.recetas =response.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
            }


        };

</script>

<style>
    .cabezera-derecha
    {
        background-color: orange;
        border-bottom-left-radius: 25;
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

</style>