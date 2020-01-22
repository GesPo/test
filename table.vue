<template>
    <v-app id="inspire">
        <v-card class="mt-5 p-3">
            <v-card-title>
                Данны по лестницам
                <v-spacer/>
                <v-text-field
                    v-model="search"
                    append-icon="search"
                    label="Поиск"
                    single-line
                    hide-details
                />
            </v-card-title>
            <v-fade-transition mode="out-in">
                <v-data-table
                    v-model="selected"
                    :headers="headers"
                    :items="reserve_list"
                    :single-select="true"
                    :page.sync="page"
                    :items-per-page="itemsPerPage"
                    :search="search"
                    :loading="loadingVar"
                    item-key="name"
                    show-select
                    hide-default-footer
                    no-results-text="Подходящих записей не найдено."
                    loading-text="Загрузка.......Пожалуйста подождите"
                    @page-count="pageCount = $event"
                    class="elevation-1"
                >
                    <template v-slot:item.reserve_status="{ item }">
                        <v-chip :color="getStatus(item.reserve_status)"><strong>{{ item.reserve_status}}</strong></v-chip>
                    </template>
                </v-data-table>
            </v-fade-transition>
            <div class="d-flex flex-nowrap pt-2">
                <v-pagination
                    v-model="page"
                    :length="pageCount"
                    :total-visible="7"
                    dense
                />
                <v-text-field
                    :value="itemsPerPage"
                    label="Строк"
                    type="number"
                    min="-1"
                    max="17"
                    @input="itemsPerPage = parseInt($event, 10)"
                />
            </div>
        </v-card>
    </v-app>

</template>

<script>
    export default {
        name: "ProductionAddingReserve",
        data () {
            return {
                selected: [],
                page: 1,
                pageCount: 0,
                itemsPerPage: 17,
                search: '',
                headers: [
                    { text: '#', align: 'left', value: 'id'},
                    { text: 'Наименование', value: 'title'},
                    { text: 'Статус', value: 'reserve_status' },
                    { text: 'Дата изменения', value: 'created_at'},
                ],
                reserve_list: [
                    {"id":1,"title":"\u0420\u0435\u0437\u0435\u0440\u0432 \u21161 \u043e\u0442 19.12.2019","reserve_status":3,"created_at":"2019-12-19 11:19:15"},
                    {"id":2,"title":"\u0420\u0435\u0437\u0435\u0440\u0432 \u21162 \u043e\u0442 19.12.2019","reserve_status":2,"created_at":"2019-12-19 13:39:21"},
                    {"id":3,"title":"\u0420\u0435\u0437\u0435\u0440\u0432 \u21163 \u043e\u0442 22.12.2019","reserve_status":1,"created_at":"2019-12-22 13:16:31"},
                    {"id":4,"title":"\u0420\u0435\u0437\u0435\u0440\u0432 \u21164 \u043e\u0442 23.12.2019","reserve_status":1,"created_at":"2019-12-23 15:11:12"}],
                loadingVar: true
            }
        },
        mounted() {
            // this.update();
        },
        methods: {
            // update () {
            //     this.loadingVar = true;
            //     axios
            //         .get('/app/get-json-reserve-list').then((response) => {  //reserve_list
            //         this.loadingVar = false;
            //         this.reserve_list = response.data;
            //     });
            // },
            // async getData() {
            //     this.loadingVar = true;
            //     this.data = await update ();
            //     this.loadingVar = false
            // },
            getStatus (status_var) {
                if (status_var === 1) return 'gray';
                else if (status_var === 2) return 'indigo lighten-1';
                else if (status_var === 3) return 'green';
                else return 'green'
            },
        }
    }
</script>
<style lang="scss">

    .v-pagination__item,
    .v-pagination__navigation {
        font-size: .75rem !important;
        height: 24px !important;
        min-width: 24px !important;
    }

    button.v-pagination__item,
    button.v-pagination__navigation{
        box-shadow: 0 2px 1px -2px rgba(0, 0, 0, .2), 0 1px 2px 0 rgba(0, 0, 0, .14), 0 1px 1px 0 rgba(0, 0, 0, .12) !important;
    }

    button.v-pagination__item:active,
    button.v-pagination__item:hover,
    button.v-pagination__item:focus,
    button.v-pagination__navigation:active,
    button.v-pagination__navigation:hover,
    button.v-pagination__navigation:focus {
        outline: 0;
        outline-offset: 0;
    }


    .v-data-table td {
        height: 35px;
    }

    .v-data-table th {
        font-size: 1rem;
        font-style: normal;
        height: 35px;
    }

    .v-chip.v-size--default {
        border-radius: 4px;
        font-size: 14px;
        height: 24px;
    }


    /*tr:nth-child(even) {background-color: #f2f2f2;}*/

    /*td:nth-last-of-type(3),*/
    /*th:nth-last-of-type(3){*/
    /*    background: rgba(68, 133, 240, 0.11); !* Цвет фона *!*/
    /*}*/

    /*td:nth-last-of-type(2),*/
    /*th:nth-last-of-type(2){*/
    /*    background: rgba(235, 240, 109, 0.11); !* Цвет фона *!*/
    /*}*/

    /*td:nth-last-of-type(1),*/
    /*th:nth-last-of-type(1){*/
    /*    background: rgba(63, 240, 104, 0.11); !* Цвет фона *!*/
    /*}*/

</style>
