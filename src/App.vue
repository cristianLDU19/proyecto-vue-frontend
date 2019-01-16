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
    import detalleRecetas from "./componentes/detalle-receta";
    import  pasos from "./componentes/Pasos";
    import  carreraChef from "./componentes/CarreraChef";
    import  retoUno from "./componentes/retoUno";
    import  retoDos from "./componentes/retoDos";
    import seleccion from "./componentes/seleccionar-Carrera";

    const AppNavigation = StackNavigator(
        {
            Recetas: Prueba,
            Juegos: Modalidades,
            ResultadoCorrecto: resultadoCorrectoPregunta,
            ResultadoIncorrecto: resultadoIncorrectoPregunta,
            DetalleRecetas: detalleRecetas,
            Pasos: pasos,
            CarreraChef:carreraChef,
            RetoUno: retoUno,
            RetoDos: retoDos,
            seleccionReceta: seleccion


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



