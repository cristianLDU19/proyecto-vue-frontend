<template>
    <nb-container :style="{ backgroundColor: '#FFA500'}">
    <view :style="{ flex:1,justifyContent: 'center',alignItems: 'center'}">
        <text class="titulo">SE ACABÓ</text>
        <text class="titulo">EL</text>
        <text class="titulo">TIEMPO!!!</text>

        <image :source="{uri: 'https://firebasestorage.googleapis.com/v0/b/sistemacalificaciones-78da3.appspot.com/o/retoUno.PNG?alt=media&token=1503da00-70cd-4f5b-9acd-e5d57d542fe8'}" class="card-item-image" />

        <text class="mensaje1">EL QUE TERMINÓ AL FINAL, REALIZA LA SIGUIENTE PENITENCIA:</text>

        <nb-button rounded class="boton" :onPress="irRetoDos">
            <nb-text>OK</nb-text>
        </nb-button>
    </view>
    </nb-container>
</template>

<script>
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
            }
        },
        name: "resultado-preguntas",
        methods:
        {
            irRetoDos: function()
            {
                this.navigation.navigate("RetoDos",{orden: this.orden, nombreReceta: this.RecetaSlug});
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
    color: white;
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