<template>
    <div class="top-menu-div">
            <select id="gender" @change="onSelectGender" v-model="gender">
                <option value="1">мужской</option>
                <option value="0">женский</option>
            </select>

            <select id="rating" @change="onChangeFilter" v-model="filter.rating">
                <option value="0">0</option>
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
                <option value="5">5</option>
                <option value="6">6</option>
                <option value="7">7</option>
                <option value="8">8</option>
                <option value="9">9</option>
                <option value="10">10</option>
            </select>

        <select id="sort" @change="onChangeSort" v-model="sort">
            <option value="alphabet">По алфавиту</option>
            <option value="rating">По рейтингу</option>
        </select>

        <br>


            <input id="lastname" placeholder="фамилия" type="text" @change="onChangeFilter" v-model="filter.lastname">

            <input id="namebeg" placeholder="начало имени" type="text" @change="onChangeFilter" v-model="filter.namebeg">

            <input id="nameend" placeholder="окончание имени" type="text" @change="onChangeFilter" v-model="filter.nameend">

            <input id="patronym" placeholder="отчество" type="text" @change="onChangeFilter" v-model="filter.patronym">



    </div>
</template>

<script>
    export default {
        name: "topMenu",created: function(){
            this.loadFilter();
        },
        data() {
            return {
                gender: '1',
                sort: '',
                filter: {
                    lastname: '',
                    namebeg: '',
                    nameend: '',
                    patronym: '',
                    rating: '0'
                }
            }
        },
        methods: {
            onChangeSort(){
                this.$emit('sort', this.sort);
            },
            onChangeFilter(){
                this.$emit('filter', this.filter);
            },
            onSelectGender() {
                this.$emit('gender', this.gender);
            },
            loadFilter(){
                if(localStorage.getItem('filter')) this.filter = JSON.parse(localStorage.getItem('filter'));
            }
        }
    }
</script>

<style scoped>
    .top-menu-div{
        height: 50px;
        font: 95% Arial, Helvetica, sans-serif;
        max-width: 200px;
        margin: 10px;
        padding: 16px;
        background: #F7F7F7;
        white-space:nowrap
    }
    .top-menu-div h1{
        background: #43D1AF;
        padding: 20px 0;
        font-size: 140%;
        font-weight: 300;
        text-align: center;
        color: #fff;
        margin: -16px -16px 16px -16px;
    }
    .top-menu-div input[type="text"],
    .top-menu-div input[type="date"],
    .top-menu-div input[type="datetime"],
    .top-menu-div input[type="email"],
    .top-menu-div input[type="number"],
    .top-menu-div input[type="search"],
    .top-menu-div input[type="time"],
    .top-menu-div input[type="url"],
    .top-menu-div textarea,
    .top-menu-div select
    {
        margin-right: 10px;
        display: inline-block;
        -webkit-transition: all 0.30s ease-in-out;
        -moz-transition: all 0.30s ease-in-out;
        -ms-transition: all 0.30s ease-in-out;
        -o-transition: all 0.30s ease-in-out;
        outline: none;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
        -moz-box-sizing: border-box;
        width: 100%;
        background: #fff;
        margin-bottom: 4%;
        border: 1px solid #ccc;
        padding: 3%;
        color: #555;
        font: 95% Arial, Helvetica, sans-serif;
    }
    .top-menu-div input[type="text"]:focus,
    .top-menu-div input[type="date"]:focus,
    .top-menu-div input[type="datetime"]:focus,
    .top-menu-div input[type="email"]:focus,
    .top-menu-div input[type="number"]:focus,
    .top-menu-div input[type="search"]:focus,
    .top-menu-div input[type="time"]:focus,
    .top-menu-div input[type="url"]:focus,
    .top-menu-div textarea:focus,
    .top-menu-div select:focus
    {
        box-shadow: 0 0 5px #43D1AF;
        padding: 3%;
        border: 1px solid #43D1AF;
    }

    #gender{
        width: 100px;
    }

    #rating{
        width: 80px;
    }

    #sort{
        width: 150px;
    }

    #namebeg{
        width: 110px;
    }

    #nameend{
        width: 132px;
    }
</style>