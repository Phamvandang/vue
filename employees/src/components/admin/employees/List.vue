<template>
  <div class="employees">
      <h1>Employess List</h1>
      <div class="wrap">
        <v-data-table :headers="headers" :items="data" :loading="loadingTable" :search="search">
            <template v-slot:top>
                <v-toolbar flat color="white">
                    <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
                    <v-spacer></v-spacer>
                    <v-dialog v-model="dialog" max-width="500px">
                        <template v-slot:activator="{ on }">
                            <v-btn color="primary" dark class="mb-2" v-on="on">New Item</v-btn>
                        </template>
                        <v-card>
                            <v-card-title>
                                <span class="headline">{{ formTitle }}</span>
                            </v-card-title>

                            <v-card-text>
                                <v-container>
                                    <v-row>
                                    <v-col cols="12" sm="6" md="4">
                                        <v-text-field v-model="editedItem.name" label="Dessert name"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="6" md="4">
                                        <v-text-field v-model="editedItem.calories" label="Calories"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="6" md="4">
                                        <v-text-field v-model="editedItem.fat" label="Fat (g)"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="6" md="4">
                                        <v-text-field v-model="editedItem.carbs" label="Carbs (g)"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="6" md="4">
                                        <v-text-field v-model="editedItem.protein" label="Protein (g)"></v-text-field>
                                    </v-col>
                                    </v-row>
                                </v-container>
                            </v-card-text>

                            <v-card-actions>
                                <v-spacer></v-spacer>
                                <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                                <v-btn color="blue darken-1" text @click="save">Save</v-btn>
                            </v-card-actions>
                        </v-card>
                    </v-dialog>
                </v-toolbar>
            </template>
            <template v-slot:item.actions="{ item }">
            <v-icon small class="mr-4" @click="editItem(item)">
                mdi-pencil
            </v-icon>
            <v-icon small @click="deleteItem(item)">
                mdi-delete
            </v-icon>
            </template>
            <!-- <template v-slot:no-data>
            <v-btn color="primary" @click="initialize">Reset</v-btn>
            </template> -->
        </v-data-table>
      </div>
  </div>
</template>

<script>
export default {
    name: 'Employees',
    data: () => ({
        dialog: false,
        search: '',
        loadingTable: false,
        headers: [
        {
            text: 'ID',
            align: 'start',
            sortable: true,
            value: 'id',
        },
        { text: 'Employee Name', value: 'employee_name' },
        { text: 'Employee Salary', value: 'employee_salary' },
        { text: 'Employee Age', value: 'employee_age' },
        { text: 'Actions', value: 'actions', sortable: false },
        ],
        data: [],
        editedIndex: -1,
        editedItem: {
            employee_name: '',
            employee_salary: 0,
            employee_age: 0
        }
    }),
    mounted() {
        this.loadData()
    },
    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },
    watch: {
      dialog (val) {
        val || this.close()
      },
    },
    methods: {
        loadData() {
            this.loadingTable = true
            this.$store.dispatch('getEmployees').then(res => {
                this.data = res.data
                this.loadingTable = false
            }).catch(() => {
                this.loadingTable = true
            }) 
        },

        editItem () {
            // this.editedIndex = this.desserts.indexOf(item)
            // this.editedItem = Object.assign({}, item)
            this.dialog = true
        },

        deleteItem () {
            // const index = this.desserts.indexOf(item)
            // confirm('Are you sure you want to delete this item?') && this.desserts.splice(index, 1)
        },

        close () {
            this.dialog = false
            // this.$nextTick(() => {
            //     this.editedItem = Object.assign({}, this.defaultItem)
            //     this.editedIndex = -1
            // })
        },

        save () {
            if (this.editedIndex > -1) {
                // Object.assign(this.desserts[this.editedIndex], this.editedItem)
            } else {
                this.data.push(this.editedItem)
            }
            this.close()
        },
    }
}
</script>
<style lang="scss" scoped>
    .wrap {
        width: 80%;
        margin: 50px auto;
        max-width: 1000px;
    }
</style>
