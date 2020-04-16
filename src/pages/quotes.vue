<template>
  <q-page class="q-pa-sm">
    <q-table
      title="Quotes"
      :data="data"
      :hide-header="mode === 'grid'"
      :columns="columns"
      row-key="name"
      :grid="mode=='grid'"
      :filter="filter"
      :pagination.sync="pagination"
    >
      <template v-slot:top-right="props">
        <q-input outlined dense debounce="300" v-model="filter" placeholder="Search">
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
      <template v-slot:body-cell-stage="props">
        <q-td :props="props">
          <q-chip
            :color="(props.row.stage == 'Draft')?'green':(props.row.stage == 'Cheques'?'orange':'secondary')"
            text-color="white"
            dense
            class="text-weight-bolder"
            square
            style="width: 85px"
          >{{props.row.stage}}
          </q-chip>
        </q-td>
      </template>
      <template v-slot:body-cell-action="props">
        <q-td :props="props">
          <div class="q-gutter-sm">
            <q-btn dense color="primary" icon="edit"/>
            <q-btn dense color="red" icon="delete"/>
          </div>
        </q-td>
      </template>
    </q-table>
  </q-page>
</template>

<script>
    import {exportFile} from "quasar";

    function wrapCsvValue(val, formatFn) {
        let formatted = formatFn !== void 0 ? formatFn(val) : val;

        formatted =
            formatted === void 0 || formatted === null ? "" : String(formatted);

        formatted = formatted.split('"').join('""');

        return `"${formatted}"`;
    }

    export default {
        data() {
            return {
                filter: "",
                mode: "list",
                invoice: {},
                employee_dialog: false,
                columns: [
                    {
                        name: "serial_no",
                        align: "left",
                        label: "Serial No.",
                        field: "serial_no",
                        sortable: true
                    },
                    {
                        name: "account",
                        align: "left",
                        label: "Account",
                        field: "account",
                        sortable: true
                    },
                    {
                        name: "subject_name",
                        required: true,
                        label: "Subject Name",
                        align: "left",
                        field: "subject_name",
                        sortable: true
                    },
                    {
                        name: "amount",
                        align: "left",
                        label: "Amount",
                        field: "amount",
                        sortable: true
                    },
                    {
                        name: "entry_date",
                        align: "left",
                        label: "Entry Date",
                        field: "entry_date",
                        sortable: true
                    },
                    {
                        name: "expired_date",
                        align: "left",
                        label: "Expired Date",
                        field: "expired_date",
                        sortable: true
                    },
                    {
                        name: "stage",
                        align: "left",
                        label: "Stage",
                        field: "stage",
                        sortable: true
                    },
                    {
                        name: "action",
                        align: "left",
                        label: "Action",
                        field: "action",
                        sortable: true
                    }
                ],
                data: [
                    {
                        serial_no: "01",
                        subject_name: "Design",
                        account: "Leslie Tecklenburg",
                        amount: "$ 4200",
                        entry_date: "05/01/2020",
                        expired_date: "05/02/2020",
                        stage: "Draft",
                    },
                    {
                        serial_no: "02",
                        subject_name: "Networking",
                        account: "Lia Whitledge",
                        amount: "$ 2550",
                        entry_date: "15/12/2019",
                        expired_date: "15/04/2020",
                        stage: "Cheques",
                    },
                    {
                        serial_no: "03",
                        subject_name: "Networking",
                        account: "Sam Wileman",
                        amount: "$ 3800",
                        entry_date: "12/11/2019",
                        expired_date: "15/04/2020",
                        stage: "Draft",
                    },
                    {
                        serial_no: "06",
                        subject_name: "Technology",
                        account: "John Rozelle",
                        amount: "$ 3200",
                        entry_date: "10/11/2019",
                        expired_date: "12/03/2020",
                        stage: "Transfer",
                    },
                    {
                        serial_no: "04",
                        subject_name: "Technology",
                        account: "Edgar Colmer",
                        amount: "$ 4000",
                        entry_date: "11/09/2019",
                        expired_date: "25/01/2020",
                        stage: "Cheques",
                    },
                    {
                        serial_no: "05",
                        subject_name: "Networking",
                        account: "Kaiden Rozelle",
                        amount: "$ 1200",
                        entry_date: "10/11/2019",
                        expired_date: "15/03/2020",
                        stage: "Transfer",
                    },
                    {
                        serial_no: "07",
                        subject_name: "Technology",
                        account: "Jacob Firtado",
                        amount: "$ 2200",
                        entry_date: "09/10/2019",
                        expired_date: "23/03/2020",
                        stage: "Draft",
                    },
                    {
                        serial_no: "05",
                        subject_name: "Design",
                        account: "John Doe",
                        amount: "$ 900",
                        entry_date: "12/11/2019",
                        expired_date: "06/03/2020",
                        stage: "Transfer",
                    }
                ],
                pagination: {
                    rowsPerPage: 10
                }
            };
        },
        methods: {
            exportTable() {
                // naive encoding to csv format
                const content = [this.columns.map(col => wrapCsvValue(col.label))]
                    .concat(
                        this.data.map(row =>
                            this.columns
                                .map(col =>
                                    wrapCsvValue(
                                        typeof col.field === "function"
                                            ? col.field(row)
                                            : row[col.field === void 0 ? col.name : col.field],
                                        col.format
                                    )
                                )
                                .join(",")
                        )
                    )
                    .join("\r\n");

                const status = exportFile("quotes.csv", content, "text/csv");

                if (status !== true) {
                    this.$q.notify({
                        message: "Browser denied file download...",
                        color: "negative",
                        icon: "warning"
                    });
                }
            }
        }
    };
</script>
<style>
  .q-chip__content {
    display: block;
    text-align: center;
  }
</style>
