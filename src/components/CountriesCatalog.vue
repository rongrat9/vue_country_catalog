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
                <button type="button" class="btn btn-outline-primary btn-sm" @click="order = 'asc'">ASC</button> &nbsp;
                <button type="button" class="btn btn-outline-primary btn-sm" @click="order = 'desc'">DESC</button>
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
                        <th> Alternative Name</th>
                        <th>Calling Codes</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(country, index) in countries" :key="index">
                        <td><img :src="country.flags[1]" width="140" height="90" /></td>
                        <td id="show-modal" data-bs-toggle="modal" data-bs-target="#exampleModal"
                            @click="modalCountry(index)"><a href="#" class="text-decoration-none" :title="message">{{ country.name.official }}</a></td>
                        <td>{{ country.cca2 }}</td>
                        <td>{{ country.cca3 }}</td>
                        <td>{{ country.altSpellings.join(', ') }}</td>
                        <td>{{ country.idd.root }}</td>
                    </tr>
                </tbody>
            </table>

            <div class="row">
                <nav aria-label="Page navigation example">
                    <ul class="pagination">
                        <li class="page-item"><a class="page-link" href="#" v-if="page != 1" @click.prevent="page--">Previous</a>
                        </li>
                        <li class="page-item"  v-for="pageNumber in pages.slice(page-1, page+5)" :key="pageNumber" @click.prevent="page = pageNumber"><a class="page-link" href="#">{{ pageNumber }}</a></li>
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
            message: 'Click me to open modal',
            countries: [],
            page: 1,
            perPage: 25,
            pages: [],
            search: "",
            oldData: [],
            openModal: false,
            country_data: [],
            order : 'asc'
        };
    },
    mounted() {
        this.displayedcountries();
    },
    methods: {
        fetchCountries() {
            axios.get("https://restcountries.com/v3/all")
                .then(response => {
                    this.oldData = response.data;
                    // this.countries = response.data;
                    // console.log(this.countries) //250
                }).then(() => {
                    this.oldData = this.oldData.filter(country => country.name.official.toLowerCase().includes(this.search.toLowerCase()));
                    // this.countries = this.paginate(this.oldData);
                    if (this.order == "asc") {
                        this.countries = this.paginate(this.oldData.sort((a, b) => {
                            if (a.name.official < b.name.official) {
                                return -1;
                            }
                        }));
                    }else{
                        this.countries = this.paginate(this.oldData.sort((a, b) => {
                            if (a.name.official > b.name.official) {
                                return -1;
                            }
                        }));
                    }
                })
                .catch(error => {
                    console.error(error);
                });
        },
        setPages() {
            this.pages = [];
            let numberOfPages = Math.ceil(this.oldData.length / this.perPage);
			for (let index = 1; index <= numberOfPages; index++) {
				this.pages.push(index);
			}
            // console.log(this.pages)
            // console.log(this.page)
        },
        paginate(countries) {
            let from = (this.page * this.perPage) - this.perPage;
            let to = (this.page * this.perPage);
            return countries.slice(from, to);
        },
        // sortCountry(order) {
        //     if (order == "asc") {
        //         this.countries = this.paginate(this.oldData.sort((a, b) => {
        //             if (a.name.official < b.name.official) {
        //                 return -1;
        //             }
        //         }));
        //         return;
        //     }
        //     this.countries = this.paginate(this.oldData.sort((a, b) => {
        //         if (a.name.official > b.name.official) {
        //             return -1;
        //         }
        //     }));
        // },
        displayedcountries() {
            this.fetchCountries();
        },
        modalCountry(index) {
            this.country_data = this.countries.find((_, i) => i == index);
            console.log(this.country_data.name.official);
        }
    },
    watch: {
        countries() {
            this.setPages();
        },
        search() {
            if(this.page == 1){
                this.displayedcountries();
            }else{
                this.page = 1;
            }
        },
        page() {
            this.displayedcountries();
        },
        order(){
            this.fetchCountries()
        }
        
    },
};

</script>


