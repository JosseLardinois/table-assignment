<template>
    <h1>Student School Keuzes</h1>
    <div class="searchBar">
        <!-- Filter Search -->
        <input id="inputSearch" type="text" v-model="search" placeholder="Type een naam, school of ID" aria-label="Recipient's username" aria-describedby="button-addon2" class="form-control">
        <p v-if="noResults">Sorry, no results for {{search}}</p>
    </div>

    <table id="tableComponent" class="table table-bordered table-striped">
        <thead>
            <tr>
                <!-- loop through each value of the fields to get the table header -->
                <th v-for="field in fields" :key='field' @click="sortTable(field)">
                    {{field}} <i class="bi bi-sort-alpha-down" aria-label='Sort Icon'></i>
                </th>
            </tr>
        </thead>
        <tbody>
            <!-- Loop through the list get the each student data -->
            <tr v-for="item in results" :key='item'>
                <td v-for="field in fields" :key='field'>
                    {{item[field]}}
                </td>
                <td>
                    <button @click='getSingleUserInfo(item); TogglePopup("buttonTrigger"); sortTable(field);'>Edit Record</button>
                </td>
            </tr>
        </tbody>
    </table>  

    <div id="fullTable" style="display:none">
            <table id="tableComponent" class="table table-bordered table-striped">
        <thead>
            <tr>
                <!-- loop through each value of the fields to get the table header -->
                <th v-for="field in fields" :key='field' @click="sortTable(field)">
                    {{field}} <i class="bi bi-sort-alpha-down" aria-label='Sort Icon'></i>
                </th>
            </tr>
        </thead>
        <tbody>
            <!-- Loop through the list get the each student data -->
            <tr v-for="item in sortedList" :key='item'>
                <td v-for="field in fields" :key='field'>
                    {{item[field]}}
                </td>
                <td>
                    <button @click='getSingleUserInfo(item); TogglePopup("buttonTrigger"); sortTable(field);'>Edit Record</button>
                </td>
            </tr>
        </tbody>
    </table>
    </div>
    <div>

    </div>


    <!--------------------------------------------------------------------------------------------------------------------->
    <Edit v-if="popupTriggers.buttonTrigger" :TogglePopup="() => TogglePopup('buttonTrigger')" :fields='fields' :studentData="studentData" :currentRow='currentRow'>


    </Edit>
</template>
<script>
    import { computed, ref } from "vue";
    import { sortBy } from 'lodash';
    import Edit from '@/components/Edit.vue';
    import { useVueFuse } from 'vue-fuse'

    import "bootstrap/dist/css/bootstrap.min.css";

    export default {
        name: 'TableComponent',
        methods: {
            getSingleUserInfo(item) {
                this.currentRow = [ item.ID, item.Naam, item.School
                ];
            },
            hideDiv() {
                var x = document.getElementById("fullTable");
                if(x.style.display === "none") {
        x.style.display = "block";
    } else {
        x.style.display = "none";
    }
            }
        },
        components: {
            Edit
        },
        watch: {
            search(input) {
                if (input == "") {
                    this.hideDiv();
                }

            }, 
        }, immediate: true,
        props: {
            studentData: {
                type: Array,
            },
            fields: {
                type: Array,
            },
            defaultAll: {
                type: Boolean
            },
        },
        setup(props) {
            let sort = ref(false);
            let updatedList = ref([])
            let searchQuery = ref("");

            const popupTriggers = ref({
                buttonTrigger: false
            });
            const TogglePopup = (trigger) => {
                popupTriggers.value[trigger] = !popupTriggers.value[trigger]
            };
            const currentRow = 
                [];
            const sortTable = (col) => {
                sort.value = true
                // Use of _.sortBy() method
                updatedList.value = sortBy(props.studentData, col)
            }
            const sortedList = computed(() => {
                if (sort.value) {
                    return updatedList.value
                }
                else {
                    return props.studentData;
                }
            });
            // Filter Search
            const { search, results, noResults } = useVueFuse(sortedList, {
                keys: [
                    { name: 'ID', weight: 1 },
                    { name: 'Naam', weight: 1 },
                    { name: 'School', weight: 1 },
                ],
                
            });
            return {
                sortedList, sortTable, searchQuery, TogglePopup, popupTriggers, currentRow, search, results, noResults
                }
        }

    }
</script>
<style>
    table th:hover {
        background: #f2f2f2;
    }
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    label {
        display: inline-block;
        width: 200px;
        margin-right: 10px;
        text-align: right;
    }

    input {
    }

    fieldset {
        border: none;
        width: 500px;
        margin: 0px auto;
    }

</style>