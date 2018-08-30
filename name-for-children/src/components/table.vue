<template>
    <div class="center-table-div">
        <table class="center-table">
            <tr v-for="name in names" v-bind:key="name">
                <td>
                    {{getNameRating(name)}}
                </td>
                <td>
                    {{filter.lastname}}
                </td>
                <td>
                    {{name}}
                </td>
                <td>
                    {{filter.patronym}}
                </td>
                <td>
                    <button @click="onRating(name, true)">UP</button>
                </td>
                <td>
                   <button @click="onRating(name, false)">DOWN</button>
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
    export default {
        name: "centerTable",
        created: function(){
            this.loadData();
            this.loadFilter();
            this.setFilter();
            this.setSort();
        },
        data() {
            return {
                sort: 'rating',
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

                if(val.namebeg){
                    data = data.filter(item => (item.indexOf(val.namebeg) === 0))
                }

                if(val.nameend){
                    data = data.filter(item => (item.lastIndexOf(val.nameend) === (item.length - val.nameend.length)))
                }

                data = data.filter(item => {
                    let flag = true;

                    if(val.namebeg && (item.indexOf(val.namebeg) !== 0)) flag = false;
                    if(val.nameend && (item.lastIndexOf(val.nameend) !== (item.length - val.nameend.length))) flag = false;
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

</style>