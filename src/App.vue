<template>
    <root>
        <app-navigation></app-navigation>
    </root>
</template>

<script>
    import { StackNavigator } from "vue-native-router";
    import { Root } from "native-base";

    import Prueba from "./componentes/inicio";
    import Modalidades from "./componentes/preguntas";
    import resultadoCorrectoPregunta from "./componentes/resultado-correcto-pregunta";
    import resultadoIncorrectoPregunta from "./componentes/resultado-incorrecto-pregunta";

    const AppNavigation = StackNavigator(
        {
            Recetas: Prueba,
            Juegos: Modalidades,
            ResultadoCorrecto: resultadoCorrectoPregunta,
            ResultadoIncorrecto: resultadoIncorrectoPregunta,


        },
        {
            initialRouteName: 'Recetas',
            headerMode: "none"
        }
    )
    export default {
        components: { Root, AppNavigation }
    }
</script>



