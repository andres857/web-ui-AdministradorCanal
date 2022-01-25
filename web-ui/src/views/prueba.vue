<template>
    <b-container>
            <input type="text" v-model="nuevaFruta" @keyup.enter="addFruit">
            <button @click="addFruit"> Add </button>
        <b-row>
            <ul>
                <li v-for="fruta in frutas" :key="fruta">
                    <input type="number" v-model.number="fruta.cantidad">
                    <button @click="fruta.cantidad++">+</button>
                    <button @click="fruta.cantidad--">-</button>
                    {{fruta.nombre}} - {{fruta.cantidad}}
                    <span v-if="fruta.cantidad === 0"> No hay stock</span>
                </li>
            </ul>
            </b-row>
        <h1> Total : {{}} </h1>
    </b-container>
</template>


<script>
export default {
    data(){
        return{
            hola:'hello',
            person:{
                name:'andres',
                lastname:'Guerrero'
            },
            items:[
                { id: 1, title: 'Mi viaje con Vue' },
                { id: 2, title: 'Blogging con Vue' },
                { id: 3, title: 'Por qué Vue es tan divertido?' }
            ],
            players:[
                {Emision:"imbanaco", ipv4:"192.365.24.2", MAC:"45:56:36:98"},
                {Emision:"CARACOL", ipv4:"192.365.24.2", MAC:"45:56:36:98"},
                {Emision:"rcn", ipv4:"192.365.24.2", MAC:"45:56:36:98"},
            ],
            frutas:[
                { nombre:'pera', cantidad:8 },
                { nombre:'manzana', cantidad:0 },
                { nombre:'lulo', cantidad:0 },
            ],
            nuevaFruta:'',
            total: 0,
        }
    },
    // FUNCIONES
    methods:{
        addFruit(){
            this.frutas.push({
                nombre: this.nuevaFruta, cantidad: 9
            })
            this.nuevaFruta = ""
        },
        
    },
    // computed:{
    //     sumarfruta(){
    //         this.total = 0;
    //         for(var fruta=0; fruta < this.frutas.length; fruta++ ){
    //             this.total = this.total + fruta.cantidad;
    //         }
    //         return this.total
    //     }
    // }
}
</script>

<style scoped>

</style>