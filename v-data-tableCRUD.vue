
// App.vue
<template>
    <production-adding-reserve-table1-component/>
<template>


//ProductionAddingReserveTable1.vue
<template> // name -> production-adding-reserve-table1-component
    <v-app id="inspire">
        <v-data-table
            :headers="headersReserveList"
            :items="reserve_list"
            :single-select="true"
            :loading="loadingVar"
            item-key="id"
            show-select
            hide-default-footer
            no-results-text="Подходящих записей не найдено."
            loading-text="Загрузка.......Пожалуйста подождите"
            class="elevation-1"
        >
            <template v-slot:top>
                <v-toolbar flat color="white">
                    <v-toolbar-title>Накладные резерва</v-toolbar-title>
                    <v-divider
                        class="mx-4"
                        inset
                        vertical
                    />
                    <v-spacer/>
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
                                            <v-text-field v-model="editedItem.title" label="Наименование документа"/>
                                        </v-col>
                                    </v-row>
                                </v-container>
                            </v-card-text>

                            <v-card-actions>
                                <v-spacer/>
                                <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                                <v-btn color="blue darken-1" text @click="save">Save</v-btn>
                            </v-card-actions>
                        </v-card>
                    </v-dialog>
                </v-toolbar>
            </template>
            <template v-slot:item.reserve_status="{ item }">
                <v-chip :color="getStatus(item.reserve_status)">
                    <strong v-if="item.reserve_status === 1">Черновик</strong>
                    <strong v-else-if="item.reserve_status === 2">Документ</strong>
                    <strong v-else-if="item.reserve_status === 3">Проведен</strong>
                </v-chip>
            </template>
            <template v-slot:item.action="{ item }">
                <v-icon
                    small
                    class="mr-2"
                    @click="editItem(item)"
                >
                    edit
                </v-icon>
                <v-icon
                    small
                    @click="deleteItem(item)"
                >
                    delete
                </v-icon>
            </template>
            <template v-slot:no-data>
                <v-btn color="primary" @click="initialize">Reset</v-btn>
            </template>
        </v-data-table>
    </v-app>
</template>
<script>
    export default {
        name: "ProductionAddingReserveTable",
        data: () => ({
            dialog: false,
            headersReserveList: [
                { text: '#', align: 'left', value: 'id', width: '5%'},
                { text: 'Наименование', align: 'center', value: 'title', width: '50%'},
                { text: 'Статус', align: 'center',value: 'reserve_status',  width: '5%'  },
                { text: 'Дата изменения', align: 'center',value: 'created_at',  width: '45%'},
                { text: 'Actions', value: 'action', sortable: false },
            ],
            reserve_list: [],
            loadingVar: true,
            editedIndex: -1,
            editedItem: {
                title: '',
            },
            defaultItem: {
                title: '',
            },
        }),

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

        created () {
            this.update()
        },
        mounted() {
            this.update();
        },
        methods: {
            update() {
                this.loadingVar = true;
                axios
                    .get('/app/get-json-reserve-list').then((response) => {  //reserve_list
                    this.loadingVar = false;
                    this.reserve_list = response.data;
                });
            },
            async getData() {
                this.loadingVar = true;
                this.data = await update ();
                this.loadingVar = false
            },

            getStatus (status_var) {
                if (status_var === 1) return 'gray';
                else if (status_var === 2) return 'indigo lighten-3';
                else if (status_var === 3) return 'green';
                else return 'green'
            },

            editItem (item) {
                this.editedIndex = this.desserts.indexOf(item);
                this.editedItem = Object.assign({}, item);
                this.dialog = true
            },

            deleteItem (item) {
                const index = this.desserts.indexOf(item);
                confirm('Вы уверены, что хотите удалить этот элемент?') && this.desserts.splice(index, 1)
            },

            close () {
                this.dialog = false;
                setTimeout(() => {
                    this.editedItem = Object.assign({}, this.defaultItem);
                    this.editedIndex = -1
                }, 300)
            },

            save () {
                if (this.editedIndex > -1) {
                    Object.assign(this.desserts[this.editedIndex], this.editedItem)
                } else {
                    this.desserts.push(this.editedItem)
                }
                this.close()
            },
        },
    }

</script>

<style scoped>

</style>
