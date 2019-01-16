<template>
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
    <nb-content>
        <view :style="{ margin:20}">
            <text :style="{ color: 'orange'}"> SELECCIONE UNA RECETA </text>
        </view>
    </nb-content>

    <scroll-view>
        <nb-list>
            <nb-list-item v-for="receta in recetas" :onPress="() => {
                            this.props.navigation.navigate('CarreraChef', {
                              nombreReceta: receta.slug ,
                            });
                            }">
                <nb-thumbnail  :source="receta.foto" />
                <nb-text>  {{receta.nombre}}</nb-text>
            </nb-list-item>
        </nb-list>
    </scroll-view>


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
            this.getRecetas();
            console.log("comienzo");
        },
        methods: {
            getRecetas: function() {
                var url = 'https://proyectobackend.herokuapp.com/api/receta/'+this.RecetaSlug;
                var th = this;

                axios.get(url)
                    .then(function (response) {
                        th.recetas =response.data[0];
                        // console.log(response.data);
                        //console.log("Rceta")
                        //console.log(this.recetaInfo );
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            }
        }

    }
</script>

<style>
    .cabezera-derecha
    {
        background-color: orange;
        border-bottom-left-radius: 25;
    }

</style>