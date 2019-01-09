<template>
    <nb-container :style="{ backgroundColor: '#fff' }">
    <nb-content padder>
        <nb-card class="mb-15">
            <nb-card-item header class="cabeza">
                <nb-text>CARRERAS DEL CHEF</nb-text>
            </nb-card-item>

            <nb-card-item cardBody button :onPress="handleBodyClick">
                <image :source="{uri: 'https://firebasestorage.googleapis.com/v0/b/sistemacalificaciones-78da3.appspot.com/o/carreraChefs.png?alt=media&token=8e54e8a8-82b1-450a-872e-618e2c179f17'}" class="card-item-image" />
            </nb-card-item>
        </nb-card>
        <nb-card class="mb-15">
            <nb-card-item header class="cabeza">
                <nb-text>TRIVIA DE COMMIDA</nb-text>
            </nb-card-item>

            <nb-card-item cardBody button :onPress="() => this.props.navigation.navigate('Recetas')">
                <image :source="{uri: 'https://firebasestorage.googleapis.com/v0/b/sistemacalificaciones-78da3.appspot.com/o/trivia.png?alt=media&token=f22c70e1-2125-48a5-bb90-4a71f2579124'}" class="card-item-image" />
            </nb-card-item>
        </nb-card>
    </nb-content>
    </nb-container>
</template>

<script>
    export default {
        props: {
            navigation: {
                type: Object
            }
        },
        data: function() {
            return {
                seg: 1
            };
        },

        created: function() {
            // this.getRecetas();
        },

        methods:
            {
                /*getRecetas: function() {
                    var urlRecetas = 'http://127.0.0.1:8000/recetas';
                    axios.get(urlRecetas).then(response => {this.rece = response.data});
                },*/
                handleBodyClick: function() {
                    this.navigation.navigate("Recetas");
                },
            }
    };
</script>
<style>
    .card-item-image {
        flex: 1;
        height: 220;
    }
    .cabeza{
        padding: 10px;
        background-color: goldenrod;
        border-color: black;
    }
    .mb-15 {
        height: 50%;
        margin-left: 5;
        margin-bottom: 5;
        margin-top: 5;
        margin-right: 5;
    }

</style>