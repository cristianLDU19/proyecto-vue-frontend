<template>
    <nb-container :style="{ backgroundColor: '#fff'}">
        <nb-header>
            <nb-left>
                <nb-button transparent :onPress="() => this.props.navigation.navigate('Recetas')">
                    <nb-icon name="arrow-back" :style="{ color: 'orange'}" />
                </nb-button>

            </nb-left>
            <nb-body>
                <nb-title>TRIVIA DE COMIDA</nb-title>
            </nb-body>

        </nb-header>
        <nb-content>
            <text class="titulo-pregunta">{{pregunta.descripcion}}</text>
            <image :source="{uri: pregunta.foto}" class="card-item-image" />


                <nb-list-item
                        :selected="radio1"
                        :onPress="toggleRadio1"
                >
                    <nb-left>
                        <nb-text>{{pregunta.opcion1}}</nb-text>
                    </nb-left>
                    <nb-right>
                        <nb-radio
                                :selected="radio1"
                                :onPress="toggleRadio1"
                        />
                        <!-- <text v-if="radio1" :class="[{'text-primary': radio1==true}]">
                          checked
                        </text> -->
                    </nb-right>
                </nb-list-item>
                <nb-list-item
                        :selected="radio2"
                        :onPress="toggleRadio2"
                >
                    <nb-left>
                        <nb-text>{{pregunta.opcion2}}</nb-text>
                    </nb-left>
                    <nb-right>
                        <nb-radio
                                :selected="radio2"
                                :onPress="toggleRadio2"
                        />
                        <!-- <text v-if="radio2" :class="[{'text-warning': radio2==true}]">
                            checked
                        </text> -->
                    </nb-right>
                </nb-list-item>
                <nb-list-item
                        :selected="radio3"
                        :onPress="toggleRadio3"
                >
                    <nb-left>
                        <nb-text>{{pregunta.opcion3}}</nb-text>
                    </nb-left>
                    <nb-right>
                        <nb-radio
                                :selected="radio3"
                                :onPress="toggleRadio3"
                        />
                        <!-- <text v-if="radio3" :class="[{'text-success': radio3==true}]">
                          checked
                        </text> -->
                    </nb-right>
                </nb-list-item>
                <nb-list-item
                        :selected="radio4"
                        :onPress="toggleRadio4"
                >
                    <nb-left>
                        <nb-text></nb-text>
                    </nb-left>
                    <nb-right>
                        <nb-radio
                                :selected="radio4"
                                :onPress="toggleRadio4"
                        />
                        <!-- <text :class="[{'text-danger': radio4==true}]">
                          Checked
                        </text> -->
                        <!-- <text v-if="radio4" :class="[{'text-danger': radio4==true}, 'bg-primary']">
                          checked
                        </text> -->
                    </nb-right>
                </nb-list-item>
            <nb-button v-if="seleccionado == pregunta.correcta" rounded class="boton" :onPress="irResultadoCorrecto">
                <nb-text>SIGUIENTE</nb-text>
            </nb-button>
            <nb-button v-else rounded class="boton" :onPress="irResultadoIncorrecto">
                <nb-text>SIGUIENTE</nb-text>
            </nb-button>



            </nb-content>
    </nb-container>
</template>

<script>
    import Modal from 'react-native-modalbox';
    import axios from 'axios';
    export default {
        props: {
            navigation: {
                type: Object
            }
        },
        mounted() {
            this.getPregunta();
        },
        data: function() {
            return {
               pregunta : [],
              // pregunta : {"id":1,"descripcion":"¿cuanto es 2 + 2 ?","opcion1":"3","opcion2":"4","opcion3":"5","opcion4":"6","correcta":"4","puntaje":10,"foto":"https://firebasestorage.googleapis.com/v0/b/sistemacalificaciones-78da3.appspot.com/o/hamburguesa.jpg?alt=media&token=c7411209-51b7-4bcb-b7e3-8bf5f4cb8173","slug":"pregunta/corregir/1/"},
                radio1: false,
                radio2: false,
                radio3: false,
                radio4: false,
                seleccionado: '',
                stylesObj: {
                    checkBoxPaddingTop: {
                        paddingTop: 15
                    },
                    checkBoxBgColor: {
                        backgroundColor: "orange",
                        paddingTop: 20
                    }
                }

            };
        },



        methods:
            {
                /*
                https://proyectobackend.herokuapp.com/api/pregunta
                getRecetas: function() {
                    var urlRecetas = 'http://127.0.0.1:8000/recetas';
                    axios.get(urlRecetas).then(response => {this.rece = response.data});
                },*/

                getPregunta: function() {
                    var urlPregunta = 'https://proyectobackend.herokuapp.com/api/pregunta';
                    //axios.get(urlPregunta).then(response => {this.pregunta = response.data});
                    axios.get(urlPregunta)
                        .then(function (response) {
                            this.pregunta =response.data[0];
                           // console.log(response.data);
                            console.log("PREGUNTA")
                            console.log(this.pregunta );
                        })
                        .catch(function (error) {
                            console.log(error);
                        });
                },
                handleBodyClick: function() {
                    console.log(this.seleccionado);
                    this.navigation.navigate("Recetas");
                },
                irResultadoCorrecto: function()
                {
                    if(this.seleccionado == '')
                    {
                        alert("Porfavor seleccione una respuesta");
                    }
                    else
                    {
                        this.navigation.navigate("ResultadoCorrecto");
                    }

                },
                irResultadoIncorrecto: function()
                {
                    if(this.seleccionado == '')
                    {
                        alert("Porfavor seleccione una respuesta");
                    }
                    else
                    {
                        this.navigation.navigate("ResultadoIncorrecto");
                    }
                },
                toggleRadio1: function() {
                    this.radio1 = true;
                    this.radio2 = false;
                    this.radio3 = false;
                    this.radio4 = false;
                    this.seleccionado = this.pregunta.opcion1
                    console.log(this.seleccionado);
                },
                toggleRadio2: function() {
                    this.radio1 = false;
                    this.radio2 = true;
                    this.radio3 = false;
                    this.radio4 = false;
                    this.seleccionado = this.pregunta.opcion2
                    console.log(this.seleccionado);
                },
                toggleRadio3: function() {
                    this.radio1 = false;
                    this.radio2 = false;
                    this.radio3 = true;
                    this.radio4 = false;
                    this.seleccionado = this.pregunta.opcion3
                    console.log(this.seleccionado);
                },
                toggleRadio4: function() {
                    this.radio1 = false;
                    this.radio2 = false;
                    this.radio3 = false;
                    this.radio4 = true;
                    this.seleccionado = this.pregunta.opcion4
                    console.log(this.seleccionado);
                }

            }
    };
</script>
<style>
    .card-item-image {
        height: 200;
        width: 275;
        margin-top: 20;
        margin-bottom: 20;
        borderRadius: 15;
        align-self: center;
    }
    .titulo-pregunta
    {
        color: orange;
        fontWeight: bold;
        fontSize: 30;
        margin-top: 20;
        margin-bottom: 20;
        textAlign: center;
    }
    .boton
    {
        background-color: orange;
        align-self: center;
        margin-top: 20;
    }

</style>
