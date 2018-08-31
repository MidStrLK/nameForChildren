<template>
    <div class="center-table-div">
        <table class="center-table">
            <tr v-for="name in names" v-bind:key="name">
                <td>
                    <div class="font-awesome-div" @click="onRating(name, true)">
                        <font-awesome-icon class="font-awesome-icon" icon="thumbs-up"/>
                    </div>
                </td>
                <td class="rating">
                    {{getNameRating(name)}}
                </td>
                <td>
                    <div class="font-awesome-div" @click="onRating(name, false)">
                        <font-awesome-icon class="font-awesome-icon" icon="thumbs-down"/>
                    </div>
                </td>
                <td>
                    {{filter.lastname}}
                </td>
                <td class="name">
                    {{name}}
                </td>
                <td>
                    {{filter.patronym}}
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
    import * as importNames from "./names.json";

    export default {
        name: "centerTable",
        created: function(){
            this.boyNames = importNames.boys;
            this.girlNames = importNames.girls;
            this.loadData();
            this.loadFilter();
            this.setFilter();
            this.setSort();
        },
        data() {
            return {
                sort: 'alphabet',
                boyNames: ['first', 'second'],
                girlNames: ['qwe', 'asd', 'zxc'],
                namesRating: {
                    'first': 1,
                    'second': 1,
                    'qwe': 1,
                    'asd': 1,
                    'zxc': 1
                },
                names: ['first', 'second'],
                filter: {
                    lastname: '',
                    namebeg: '',
                    nameend: '',
                    patronym: '',
                    rating: '0'
                },
                gender: true
            }
        },
        methods: {
            onRating(name, isUp){
                const rt = this.namesRating[name];
                if((rt === undefined) || (isUp && rt > 9) || (!isUp && rt < 1)) return;

                this.namesRating[name] += isUp ? 1 : -1;

                this.saveData();
            },
            getNameRating(name){
                if(this.namesRating[name] === undefined) this.namesRating[name] = 1;
                return this.namesRating[name];
            },
            setSort(sort){
                if(sort){
                    this.sort = sort;
                }else {
                    sort = this.sort;
                }

                this.names = this.names.sort((A, B) => {
                    let a = '';
                    let b = '';
                    let mod = 1;

                    if(sort === 'alphabet'){
                        a = A.toLowerCase();
                        b = B.toLowerCase();
                        mod = 1;

                    }else if(sort === 'rating'){
                        a = this.getNameRating(A);
                        b = this.getNameRating(B);
                        mod = -1;
                    }else{
                        return 0;
                    }

                    if(a > b) return mod;
                    if(a < b) return -1*mod;
                    return 0;
                });
            },
            setGender(val) {
                this.gender = val;
                this.names = val ? this.boyNames : this.girlNames;
                this.setFilter();
                this.setSort();
            },
            setFilter(val) {
                if(val) {
                    this.filter = val;
                }else{
                    val = this.filter;
                }

                let data = this.gender ? this.boyNames : this.girlNames;

                data = data.filter(item => {
                    let flag = true;
                    const name = item.toLowerCase();
                    const nbeg = val.namebeg.toLowerCase();
                    const nend = val.nameend.toLowerCase();

                    if(nbeg && ((name.indexOf(nbeg) === -1) || (name.indexOf(nbeg) !== 0))) flag = false;
                    if(nend && ((name.indexOf(nend) === -1) || (name.lastIndexOf(nend) !== (name.length - nend.length)))) flag = false;
                    if(this.getNameRating(item) < parseInt(val.rating)) flag = false;

                    return flag;
                });

                this.names = data;

                this.saveData();
                this.saveFilter();
            },
            saveFilter(){
                localStorage.setItem('filter', JSON.stringify(this.filter));
            },
            loadFilter(){
                if(localStorage.getItem('filter')) this.filter = JSON.parse(localStorage.getItem('filter'));
            },
            saveData(){
                localStorage.setItem('names', JSON.stringify(this.getData()));
            },
            loadData(){
                if(localStorage.getItem('names')) this.namesRating = JSON.parse(localStorage.getItem('names'));
            },
            getData(){
                return this.namesRating;
            }
        }
    }
</script>

<style scoped>
    @import '../node_modules/roboto-fontface/css/roboto/roboto-fontface.css';
    table{
        margin-left: 22px;
        font-family: 'Roboto', sans-serif;
    }
    .font-awesome-div{
        width: 22px;
        height: 22px;
        color: #ccc;
    }
    .font-awesome-div:hover{
        color: #555;
    }

    .rating{
        color: #555;
    }

    .name{
        text-align: right;
    }

    td{
        -webkit-user-select: none;
        /* user-select -- это нестандартное свойство */

        -moz-user-select: none;
        /* поэтому нужны префиксы */

        -ms-user-select: none;
    }

</style>