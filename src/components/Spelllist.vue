<template>
    <div style="color: white;" class="container mt-5">
        <table class="table custom-table table-dark mx-auto">
            <thead>
                <tr>
                    <th scope="col">ID</th>
                    <th scope="col">Poziom</th>
                    <th scope="col">Nazwa</th>
                    <th scope="col">Szkoła czaru</th>
                    <th scope="col">Czas rzucania</th>
                </tr>
            </thead>
            <tbody v-if="this.items.length > 0">
                    <tr v-for="item in VisiblePost" :key="item.id" class="position-relative">
                        <td scope="row">
                            <router-link class="stretched-link" style="text-decoration: none; color:whitesmoke" 
                            :to="{ name: 'BySpellId', params: {  id: item.id }}">
                                {{ item.id }}
                            </router-link>
                        </td> 
                        <td>{{ item.title.slice(0,15) }}</td> 
                        <td>{{ item.body.slice(0,15) }}</td> 
                        <td>{{ item.title.slice(0,15) }}</td>  
                        <td>{{ item.title.slice(0,15) }}</td>
                    </tr>
            </tbody>
            <tbody v-else>
                <tr>
                    <td colspan="5">
                        Ładowanie...
                    </td>
                </tr>
            </tbody>
        </table>
        <nav aria-label="Page navigation">
            <ul class="pagination justify-content-center mt-4">
                <li class="page-item">
                    <a class="page-link"
                    aria-label="Previous"
                    href="#"
                    @click="changePage(currentPage - 1)"
                    :class="{disabled : currentPage === 1}"
                    ><span 
                    :class="{discolor : currentPage === 1}" 
                    aria-hidden="true">&laquo;</span></a>
                </li>
                <li class="page-item" 
                v-for="pageNumber in visiblePageNumbers"
                :key="pageNumber"
                :class="{active : currentPage == pageNumber} "
                >
                    <a class="page-link"
                    href="#" @click="changePage(pageNumber)">
                        {{ pageNumber }}
                    </a>
                </li>
                <li class="page-item">
                    <a class="page-link"
                    href="#"
                    @click="changePage(currentPage + 1)"
                    :class="{disabled : currentPage === totalPages}"
                    aria-label="Next">
                    <span 
                        :class="{discolor : currentPage === totalPages}" 
                        aria-hidden="true"
                        >&raquo;</span></a>
                </li>
            </ul>
        </nav>
    </div>  
</template>

<script>
import axios from 'axios';


export default {
    name: "Spelllist",
    data() {
        return {
            items: [],
            itemPerPage: 25,
            currentPage: 1
        };
    },
    computed: {
        VisiblePost() {
            const startPage = (this.currentPage - 1 ) *  this.itemPerPage;
            const endPage = startPage + this.itemPerPage
            return this.items.slice(startPage,endPage)
        },
        totalPages() {
            return Math.ceil(this.items.length / this.itemPerPage)
        },
        visiblePageNumbers() {
            let pageNumbers = []
            if (this.totalPages <= 7) {
                for(let i = 1; i <= this.totalPages; i++) {
                    pageNumbers.push(i)
                }
            }else {
                if(this.currentPage <= 4){
                    pageNumbers = [1,2,3,4,5,"...",this.totalPages];
                }else if (this.currentPage >= this.totalPages - 3) {
                   pageNumbers 
                   = [
                        1,
                        "...",
                        this.totalPages - 4, 
                        this.totalPages - 3, 
                        this.totalPages - 2, 
                        this.totalPages - 1,
                        this.totalPages
                    ] 
                }else {
                    pageNumbers = [1,"...",this.currentPage - 1, this.currentPage, this.currentPage + 1, "...", this.totalPages]}
            }
            return pageNumbers
        }
    },
    mounted() {
        axios.get('https://localhost:7095/api/Spell')
            .then(response => this.items = response.data);
    },
    methods: {
        changePage(page) {
            if(page >= 1 && page <= this.totalPages){
                this.currentPage = page
            }
        }
    }
};
    </script>

<style scoped>
    .table {
    border-collapse: separate;
    border-spacing:0 5px;
    }
    .custom-table {
    min-width: 1200px; 
    }
    .custom-table thead tr, .custom-table thead th {
        border-top: none;
        border-bottom: none !important;
        color: #fff; 
        
    }
    .custom-table tbody th, .custom-table tbody td {
        color: #bababa;
        font-weight: 400;
        padding-bottom: 20px;
        padding-top: 20px;
        font-weight: 300; 
    }
        .custom-table tbody th small, .custom-table tbody td small {
        color: #b3b3b3;
        font-weight: 300; 
    }
    .custom-table tbody tr th, .custom-table tbody tr td {
        background: #27272d;
        border: none;
        -webkit-transition: .3s all ease;
        -o-transition: .3s all ease;
        transition: .3s all ease; 
    }
     
    .custom-table tbody tr.active th, .custom-table tbody tr.active td, .custom-table tbody tr:hover th, .custom-table tbody tr:hover td {
        color: #fff;
        background: #2e2e36; 
    }
       
    .discolor {
        color: gray !important;    
    }
    
   .stretched-link::after {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        z-index: 1;
        pointer-events: auto;
        content: "";
        background-color: rgba(0, 0, 0, 0);
    }

    .position-relative {
        position: relative;
    }
    .pagination li a
    {
        background-color: #202528;
        color: whitesmoke;
    }

    
    .pagination li a:hover,
    .pagination li span:focus,
    .pagination li span:hover
    {
        color: black;
        background-color: #eee;
        border-color: #ddd;
    }

    .pagination .active a
    {
        color: black;
        background-color:whitesmoke !Important;
        border: solid 1px black !Important;
    }

    .pagination .active a:hover
    {
        background-color: #5A4181 !Important;
        border: solid 1px #5A4181;
    }
</style>
