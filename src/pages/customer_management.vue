<template>
  <q-page class="q-pa-sm">
    <q-table
      title="Customer Management"
      :data="data"
      :columns="columns"
      row-key="name"
      :filter="filter"
      :pagination.sync="pagination"
    >
      <template v-slot:top-right>
        <q-input borderless dense debounce="300" v-model="filter" placeholder="Search">
          <template v-slot:append>
            <q-icon name="search"/>
          </template>
        </q-input>
      </template>
    </q-table>
  </q-page>
</template>

<script>
    export default {
        data() {
            return {
                filter: '',
                columns: [
                    {
                        name: 'desc',
                        required: true,
                        label: 'Customer Name',
                        align: 'left',
                        field: row => row.name,
                        sortable: true
                    },
                    {name: 'city', align: 'left', label: 'City', field: 'city', sortable: true},
                    {name: 'state', align: 'left', label: 'State', field: 'state', sortable: true},
                    {name: 'last_call', align: 'left', label: 'Last Call', field: 'last_call', sortable: true}
                ],
                data: [
                    {
                        name: 'Dr. Jada Conolly',
                        city: 'GILBERT',
                        state: 'AZ',
                        last_call: '12-09-2019'
                    },
                    {
                        name: 'Dr. Kiley Ibbotson',
                        city: 'LA MESA',
                        state: 'CA',
                        last_call: '09-02-2019'
                    },
                    {
                        name: 'Dr. Leslie Tecklenburg',
                        city: 'SAN DIEGO',
                        state: 'CA',
                        last_call: '03-25-2019'
                    },
                    {
                        name: 'Dr. Lia Whitledge',
                        city: 'PHOENIX',
                        state: 'AZ',
                        last_call: '03-18-2019'
                    },
                    {
                        name: 'Dr. Sam Wileman',
                        city: 'MESA',
                        state: 'AZ',
                        last_call: '04-09-2019'
                    },
                    {
                        name: 'Dr. Edgar Colmer',
                        city: 'PHOENIX',
                        state: 'AZ',
                        last_call: '09-03-2019'
                    },
                    {
                        name: 'Dr. Kaiden Rozelle',
                        city: 'LAKEWOOD',
                        state: 'CA',
                        last_call: '01-12-2019'
                    },
                    {
                        name: 'Dr. Leslie Stopher',
                        city: 'YUMA',
                        state: 'AZ',
                        last_call: '04-15-2019'
                    },
                    {
                        name: 'Dr. Miguel Subasic',
                        city: 'TEMPE',
                        state: 'AZ',
                        last_call: '11-09-2019'
                    },
                    {
                        name: 'Dr. Reese Vandygriff',
                        city: 'LAKEWOOD',
                        state: 'CA',
                        last_call: '01-01-2019'
                    },
                    {
                        name: 'Dr. Griffin Troglen',
                        city: 'YUMA',
                        state: 'AZ',
                        last_call: '04-12-2019'
                    },
                    {
                        name: 'Dr. Zachary Wehrley',
                        city: 'TEMPE',
                        state: 'AZ',
                        last_call: '10-09-2019'
                    },
                    {
                        name: 'Dr. Kyle Wahlert',
                        city: 'LAKEWOOD',
                        state: 'CA',
                        last_call: '01-02-2019'
                    }
                ],
                pagination: {
                  rowsPerPage: 10
                }
            }
        }
    }
</script>
