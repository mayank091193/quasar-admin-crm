<template>
  <q-page class="q-pa-sm">
    <q-table
      title="Change Request"
      :data="data"
      :hide-header="mode === 'grid'"
      :columns="columns"
      row-key="name"
      :grid="mode=='grid'"
      :filter="filter"
      :pagination.sync="pagination"
    >
      <template v-slot:top-right="props">
        <q-input borderless dense debounce="300" v-model="filter" placeholder="Search">
          <template v-slot:append>
            <q-icon name="search"/>
          </template>
        </q-input>

        <q-btn
          flat
          round
          dense
          :icon="props.inFullscreen ? 'fullscreen_exit' : 'fullscreen'"
          @click="props.toggleFullscreen"
          v-if="mode === 'list'"
        >
          <q-tooltip
            :disable="$q.platform.is.mobile"
            v-close-popup
          >{{props.inFullscreen ? 'Exit Fullscreen' : 'Toggle Fullscreen'}}
          </q-tooltip>
        </q-btn>

        <q-btn
          flat
          round
          dense
          :icon="mode === 'grid' ? 'list' : 'grid_on'"
          @click="mode = mode === 'grid' ? 'list' : 'grid'; separator = mode === 'grid' ? 'none' : 'horizontal'"
          v-if="!props.inFullscreen"
        >
          <q-tooltip
            :disable="$q.platform.is.mobile"
            v-close-popup
          >{{mode==='grid' ? 'List' : 'Grid'}}
          </q-tooltip>
        </q-btn>

        <q-btn
          color="primary"
          icon-right="archive"
          label="Export to csv"
          no-caps
          @click="exportTable"
        />

      </template>
      <template v-slot:body-cell-status="props">
        <q-td :props="props">
          <q-chip :color="(props.row.status == 'Approved')?'green':(props.row.status == 'Rejected'?'red':'grey')"
                  text-color="white" dense class="text-weight-bolder" square style="width: 85px">{{props.row.status}}
          </q-chip>
        </q-td>
      </template>
    </q-table>
  </q-page>
</template>

<script>

    import {exportFile} from 'quasar'

    function wrapCsvValue(val, formatFn) {
        let formatted = formatFn !== void 0
            ? formatFn(val)
            : val

        formatted = formatted === void 0 || formatted === null
            ? ''
            : String(formatted)

        formatted = formatted.split('"').join('""')

        return `"${formatted}"`
    }

    export default {
        data() {
            return {
                filter: '',
                mode: 'list',
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
                    {
                        name: 'creation_date',
                        align: 'left',
                        label: 'Creation Date',
                        field: 'creation_date',
                        sortable: true
                    }
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
        },
        methods: {
            exportTable() {
                // naive encoding to csv format
                const content = [this.columns.map(col => wrapCsvValue(col.label))].concat(
                    this.data.map(row => this.columns.map(col => wrapCsvValue(
                        typeof col.field === 'function'
                            ? col.field(row)
                            : row[col.field === void 0 ? col.name : col.field],
                        col.format
                    )).join(','))
                ).join('\r\n')

                const status = exportFile(
                    'change-request.csv',
                    content,
                    'text/csv'
                )

                if (status !== true) {
                    this.$q.notify({
                        message: 'Browser denied file download...',
                        color: 'negative',
                        icon: 'warning'
                    })
                }
            }
        }
    }
</script>
<style>
  .q-chip__content {
    display: block;
    text-align: center;
  }
</style>
