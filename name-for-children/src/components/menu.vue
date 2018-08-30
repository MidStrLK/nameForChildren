<template>
    <div class="top-menu-div">
        <p>
            <select @change="onSelectGender" v-model="gender">
                <option value="1">мужское</option>
                <option value="0">женское</option>
            </select>

            <select @change="onChangeFilter" v-model="filter.rating">
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

            <input @change="onChangeFilter" v-model="filter.lastname" placeholder="Фамилия">
            <input @change="onChangeFilter" v-model="filter.namebeg" placeholder="Начало имени">
            <input @change="onChangeFilter" v-model="filter.nameend" placeholder="Окончание имени">
            <input @change="onChangeFilter" v-model="filter.patronym" placeholder="Отчество">

            <select @change="onChangeSort" v-model="sort">
                <option value="alphabet">По алфавиту</option>
                <option value="rating">По рейтингу</option>
            </select>
        </p>
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
        background-color: aliceblue;
    }

    .top-menu-div input{
        margin-right: 20px;
    }
</style>