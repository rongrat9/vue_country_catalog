<template>
    <div class="container">
        <div class="text-center my-5">
            <h1>Countries Catalog</h1>
            <hr />
        </div>

        <div class="input-group mb-3">
            <span class="input-group-text" id="basic-addon1">Search</span>
            <input type="text" class="form-control" placeholder="Country Name" v-model="search" aria-label="search"
                aria-describedby="basic-addon1">
        </div>

        <div class="row mb-3">
            <div class="col-2 text-start">
                <label for="" class="">Sorting</label>
            </div>
            <div class="col-12 text-start">
                <button type="button" class="btn btn-outline-primary btn-sm" @click="sortCountry('asc')">ASC</button> &nbsp;
                <button type="button" class="btn btn-outline-primary btn-sm" @click="sortCountry('desc')">DESC</button>
            </div>

        </div>


        <div class="table-responsive">
            <table class="table table-striped">
                <thead class="table-dark">
                    <tr>
                        <th>Flag</th>
                        <th>Country Name</th>
                        <th>CCA2</th>
                        <th>CCA3</th>
                        <!-- <th>Native Country Name</th> -->
                        <th> Alternative Country Name</th>
                        <th>Country Calling Codes</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(country,index) in countries" :key="index">

                        <td><img :src="country.flags[1]" width="140" height="90" /></td>
                        <td id="show-modal" data-bs-toggle="modal" data-bs-target="#exampleModal" @click="modalCountry(index)">{{ country.name.official }}  </td>
                        <td>{{ country.cca2 }}</td>
                        <td>{{ country.cca3 }}</td>
                        <!-- <td>{{ country.name.nativeName }}</td> -->
                        <td>{{ country.altSpellings.join(', ') }}</td>
                        <td>{{ country.idd.root }}</td>
                    </tr>
                </tbody>

            </table>

            <div class="row">
                <nav aria-label="Page navigation example">
                    <ul class="pagination">
                        <li class="page-item"><a class="page-link" href="#" v-if="page != 1" @click="page--">Previous</a>
                        </li>
                        <li class="page-item" v-for="pageNumber in pages.slice(page - 1, page + 5)" :key="pageNumber"
                            @click="page = pageNumber"><a class="page-link" href="#">{{ pageNumber }}</a></li>
                        <li class="page-item"><a class="page-link" href="#" @click="page++"
                                v-if="page < pages.length">Next</a></li>
                    </ul>
                </nav>
            </div>
        </div>
    </div>

    
    <CountryModal :country="country_data" />

</template>
<script>
import axios from 'axios';
import CountryModal from './CountryModal.vue';

export default {
    components: { 
        CountryModal 
    },
    data() {
        return {
            countries: [],
            page: 1,
            perPage: 25,
            pages: [],
            search: "",
            oldData: [],
            openModal: false,
            country_data: []
        };
    },
    created() {
        this.fetchCountries();
    },
    methods: {
        fetchCountries() {
            axios.get("https://restcountries.com/v3/all")
                .then(response => {
                this.oldData = response.data;
                this.countries = response.data;
                // console.log(this.countries) //250
            }).then(() => {
                this.displayedcountries();
            })
                .catch(error => {
                console.error(error);
            });
        },
        setPages() {
            let numberOfPages = Math.ceil(this.countries.length / this.perPage);
            for (let index = 1; index <= numberOfPages; index++) {
                this.pages.push(index);
            }
        },
        paginate(countries) {
            let page = this.page;
            let perPage = this.perPage;
            let from = (page * perPage) - perPage;
            let to = (page * perPage);
            return countries.slice(from, to);
        },
        sortCountry(order) {
            if (order == "asc") {
                this.countries = this.countries.sort((a, b) => {
                    if (a.name.official < b.name.official) {
                        return -1;
                    }
                });
                return;
            }
            this.countries = this.countries.sort((a, b) => {
                if (a.name.official > b.name.official) {
                    return -1;
                }
            });
        },
        displayedcountries() {
            this.countries = this.paginate(this.oldData.filter(country => country.name.official.toLowerCase().includes(this.search.toLowerCase())));
        },
        modalCountry(index){
            this.country_data = this.countries.find((_, i) => i == index);
            console.log(this.country_data.name.official);
        }
    },
    computed: {},
    watch: {
        openModal() {
            console.log(this.openModal);
        },
        countries() {
            this.setPages();
        },
        search() {
            this.displayedcountries();
        }
    },
};

</script>


