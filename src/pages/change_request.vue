<template>
  <q-page class="q-pa-sm">
    <q-table
      title="Change Request"
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
      <template v-slot:body-cell-status="props">
            <q-td :props="props">
              <q-chip :color="(props.row.status == 'Approved')?'green':(props.row.status == 'Rejected'?'red':'grey')" text-color="white" dense class="text-weight-bolder" square style="width: 85px">{{props.row.status}}</q-chip>
            </q-td>
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
                    {name: 'change_id', align: 'left', label: 'Change ID', field: 'change_id', sortable: true},
                    {
                        name: 'desc',
                        required: true,
                        label: 'Customer Name',
                        align: 'left',
                        field: row => row.name,
                        sortable: true
                    },
                    {name: 'change_type', align: 'left', label: 'Change Type', field: 'change_type', sortable: true},
                    {name: 'status', align: 'left', label: 'Status', field: 'status', sortable: true},
                    {name: 'creation_date', align: 'left', label: 'Creation Date', field: 'creation_date', sortable: true}
                ],
                data: [
                    {
                        change_id: "C001",
                        name: 'Dr. Jada Conolly',
                        change_type: 'Name Change',
                        status: 'Submitted',
                        creation_date: '12-09-2019'
                    },
                    {
                        change_id: "C002",
                        name: 'Dr. Kiley Ibbotson',
                        change_type: 'Address Change',
                        status: 'Approved',
                        creation_date: '09-02-2019'
                    },
                    {
                        change_id: "C003",
                        name: 'Dr. Leslie Tecklenburg',
                        change_type: 'New Profile',
                        status: 'Approved',
                        creation_date: '03-25-2019'
                    },
                    {
                        change_id: "C004",
                        name: 'Dr. Lia Whitledge',
                        change_type: 'Name Change',
                        status: 'Submitted',
                        creation_date: '03-18-2019'
                    },
                    {
                        change_id: "C005",
                        name: 'Dr. Sam Wileman',
                        change_type: 'Name Change',
                        status: 'Rejected',
                        creation_date: '04-09-2019'
                    },
                    {
                        change_id: "C006",
                        name: 'Dr. Edgar Colmer',
                        change_type: 'Address Change',
                        status: 'Approved',
                        creation_date: '09-03-2019'
                    },
                    {
                        change_id: "C007",
                        name: 'Dr. Kaiden Rozelle',
                        change_type: 'Address Change',
                        status: 'Submitted',
                        creation_date: '01-12-2019'
                    },
                    {
                        change_id: "C008",
                        name: 'Dr. Leslie Stopher',
                        change_type: 'New Profile',
                        status: 'Approved',
                        creation_date: '04-15-2019'
                    },
                    {
                        change_id: "C009",
                        name: 'Dr. Miguel Subasic',
                        change_type: 'New Profile',
                        status: 'Approved',
                        creation_date: '11-09-2019'
                    },
                    {
                        change_id: "C010",
                        name: 'Dr. Reese Vandygriff',
                        change_type: 'New Profile',
                        status: 'Rejected',
                        creation_date: '01-01-2019'
                    },
                    {
                        change_id: "C011",
                        name: 'Dr. Griffin Troglen',
                        change_type: 'New Profile',
                        status: 'Approved',
                        creation_date: '04-12-2019'
                    },
                    {
                        change_id: "C012",
                        name: 'Dr. Zachary Wehrley',
                        change_type: 'Address Change',
                        status: 'Submitted',
                        creation_date: '10-09-2019'
                    },
                    {
                        change_id: "C013",
                        name: 'Dr. Kyle Wahlert',
                        change_type: 'Address Change',
                        status: 'Approved',
                        creation_date: '01-02-2019'
                    },
                    {
                        change_id: "C014",
                        name: 'Dr. John Subasic',
                        change_type: 'Address Change',
                        status: 'Approved',
                        creation_date: '07-06-2019'
                    }
                ],
                pagination: {
                  rowsPerPage: 10
                }
            }
        }
    }
</script>
<style>
  .q-chip__content{
    display: block;
    text-align: center;
  }
</style>
