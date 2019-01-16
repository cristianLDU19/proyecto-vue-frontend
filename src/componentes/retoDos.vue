<template>
    <nb-container :style="{ backgroundColor: '#FFA500'}">
    <view :style="{ flex:1,justifyContent: 'center',alignItems: 'center'}">
        <text class="titulo">{{reto.descripcion}}</text>

        <image :source="{uri: 'https://firebasestorage.googleapis.com/v0/b/sistemacalificaciones-78da3.appspot.com/o/retoDos.PNG?alt=media&token=f4f33941-f807-46ab-a903-c02e18bf2ed8'}" class="card-item-image" />

        <nb-button rounded class="boton" :onPress="irCarrera">
            <nb-text>OK</nb-text>
        </nb-button>
    </view>
    </nb-container>
</template>

<script>
    import axios from 'axios';

    export default {
        props: {
            navigation: {
                type: Object
            }
        },
        data(){
            return {
                orden: this.navigation.getParam('orden', 1),
                RecetaSlug: this.navigation.getParam('nombreReceta', 'SIN RECETA'),
                reto: {}
            }
        },
        mounted()
        {
            this.getReto();
        },
        name: "resultado-preguntas",
        methods:
        {
            getReto: function() {
                var url = 'https://proyectobackend.herokuapp.com/api/reto/aleatorio';
                var th = this;
                axios.get(url)
                    .then(function (response) {
                        th.reto = response.data[0];

                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
            irCarrera: function()
            {
                this.navigation.navigate("CarreraChef",{orden: this.orden, nombreReceta: this.RecetaSlug});
            }
        }

    }
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
    fontSize: 20;
    textAlign: center;
}
.boton
{
    background-color: orangered;
    align-self: center;
    margin-top: 20;
}

</style>