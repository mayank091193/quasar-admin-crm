<template>
  <q-page class="q-pa-sm">
    <q-table
      title="Invoices"
      :data="data"
      :hide-header="mode === 'grid'"
      :columns="columns"
      row-key="name"
      :grid="mode=='grid'"
      :filter="filter"
      :pagination.sync="pagination"
    >
      <template v-slot:top-right="props">
        <q-btn
          @click="invoice_dialog=true"
          outline
          color="primary"
          label="Add New"
          class="q-mr-xs"
        />

        <q-input outlined dense debounce="300" v-model="filter" placeholder="Search">
          <template v-slot:append>
            <q-icon name="search" />
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
          >{{props.inFullscreen ? 'Exit Fullscreen' : 'Toggle Fullscreen'}}</q-tooltip>
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
          >{{mode==='grid' ? 'List' : 'Grid'}}</q-tooltip>
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
          <q-chip
            :color="(props.row.status == 'Active')?'green':(props.row.status == 'Inactive'?'red':'grey')"
            text-color="white"
            dense
            class="text-weight-bolder"
            square
            style="width: 85px"
          >{{props.row.status}}</q-chip>
        </q-td>
      </template>
    </q-table>
    <q-dialog v-model="invoice_dialog">
      <q-card style="width: 600px; max-width: 60vw;">
        <q-card-section>
          <div class="text-h6">
            Add new invoice
            <q-btn round flat dense icon="close" class="float-right" color="grey-8" v-close-popup></q-btn>
          </div>
        </q-card-section>
        <q-separator inset></q-separator>
        <q-card-section class="q-pt-none">
          <q-form class="q-gutter-md">
            <q-list>
              <q-item>
                <q-item-section>
                  <q-item-label class="q-pb-xs">Account</q-item-label>
                  <q-input dense outlined v-model="invoice.account" label="Account" />
                </q-item-section>
              </q-item>
              <q-item>
                <q-item-section>
                  <q-item-label class="q-pb-xs">Amount</q-item-label>
                  <q-input dense outlined v-model="invoice.amount" label="Amount" />
                </q-item-section>
              </q-item>
              <q-item>
                <q-item-section>
                  <q-item-label class="q-pb-xs">Invoice Date</q-item-label>
                  <q-input
                    dense
                    outlined
                    v-model="invoice.invoice_date"
                    mask="date"
                    label="Invoice Date"
                  >
                    <template v-slot:append>
                      <q-icon name="event" class="cursor-pointer">
                        <q-popup-proxy
                          ref="invoiceDateProxy"
                          transition-show="scale"
                          transition-hide="scale"
                        >
                          <q-date
                            v-model="invoice.invoice_date"
                            @input="() => $refs.invoiceDateProxy.hide()"
                          />
                        </q-popup-proxy>
                      </q-icon>
                    </template>
                  </q-input>
                </q-item-section>
              </q-item>
              <q-item>
                <q-item-section>
                  <q-item-label class="q-pb-xs">Due Date</q-item-label>
                  <q-input dense outlined v-model="invoice.due_date" mask="date" label="Due Date">
                    <template v-slot:append>
                      <q-icon name="event" class="cursor-pointer">
                        <q-popup-proxy
                          ref="dueDateProxy"
                          transition-show="scale"
                          transition-hide="scale"
                        >
                          <q-date
                            v-model="invoice.due_date"
                            @input="() => $refs.dueDateProxy.hide()"
                          />
                        </q-popup-proxy>
                      </q-icon>
                    </template>
                  </q-input>
                </q-item-section>
              </q-item>
              <q-item>
                <q-item-section>
                  <q-item-label class="q-pb-xs">Invoice Type</q-item-label>
                  <q-input dense outlined v-model="invoice.invoice_type" label="Invoice Type" />
                </q-item-section>
              </q-item>
              <q-item>
                <q-item-section>
                  <q-item-label class="q-pb-xs">Status</q-item-label>
                  <q-input dense outlined v-model="invoice.status" label="Status" />
                </q-item-section>
              </q-item>
            </q-list>
          </q-form>
        </q-card-section>

        <q-card-actions align="right" class="bg-white text-teal">
          <q-btn label="Save" type="submit" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import { exportFile } from "quasar";

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
      invoice_dialog: false,
      columns: [
        {
          name: "invoice_id",
          align: "left",
          label: "#",
          field: "invoice_id",
          sortable: true
        },
        {
          name: "account",
          required: true,
          label: "Account",
          align: "left",
          field: "account",
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
          name: "invoice_date",
          align: "left",
          label: "Invoice Date",
          field: "invoice_date",
          sortable: true
        },
        {
          name: "due_date",
          align: "left",
          label: "Due Date",
          field: "due_date",
          sortable: true
        },
        {
          name: "invoice_type",
          align: "left",
          label: "Invoice Type",
          field: "invoice_type",
          sortable: true
        },
        {
          name: "status",
          align: "left",
          label: "Status",
          field: "status",
          sortable: true
        }
      ],
      data: [
        {
          invoice_id: "INV 002",
          account: "Kiley Ibbotson",
          amount: "$ 1900",
          invoice_type: "Onetime",
          status: "Active",
          invoice_date: "09-02-2019",
          due_date: "10-02-2019"
        },
        {
          invoice_id: "INV 003",
          account: "Leslie Tecklenburg",
          amount: "$ 1200",
          invoice_type: "Onetime",
          status: "Active",
          invoice_date: "03-25-2019",
          due_date: "04-25-2019"
        },
        {
          invoice_id: "INV 004",
          account: "Lia Whitledge",
          amount: "$ 1550",
          invoice_type: "Onetime",
          status: "Active",
          invoice_date: "03-18-2019",
          due_date: "04-18-2019"
        },
        {
          invoice_id: "INV 005",
          account: "Sam Wileman",
          amount: "$ 1800",
          invoice_type: "Onetime",
          status: "Inactive",
          invoice_date: "04-09-2019",
          due_date: "05-09-2019"
        },
        {
          invoice_id: "INV 006",
          account: "Edgar Colmer",
          amount: "$ 1000",
          invoice_type: "Onetime",
          status: "Active",
          invoice_date: "09-03-2019",
          due_date: "10-03-2019"
        },
        {
          invoice_id: "INV 007",
          account: "Kaiden Rozelle",
          amount: "$ 1200",
          invoice_type: "Onetime",
          status: "Active",
          invoice_date: "01-12-2019",
          due_date: "02-12-2019"
        },
        {
          invoice_id: "INV 008",
          account: "Leslie Stopher",
          amount: "$ 1500",
          invoice_type: "Onetime",
          status: "Active",
          invoice_date: "04-15-2019",
          due_date: "05-15-2019"
        },
        {
          invoice_id: "INV 009",
          account: "Miguel Subasic",
          amount: "$ 2000",
          invoice_type: "Onetime",
          status: "Active",
          invoice_date: "11-09-2019",
          due_date: "12-09-2019"
        },
        {
          invoice_id: "INV 010",
          account: "Reese Vandygriff",
          amount: "$ 1450",
          invoice_type: "Onetime",
          status: "Inactive",
          invoice_date: "01-01-2019",
          due_date: "02-01-2019"
        },
        {
          invoice_id: "INV 011",
          account: "Griffin Troglen",
          amount: "$ 1200",
          invoice_type: "Onetime",
          status: "Active",
          invoice_date: "04-12-2019",
          due_date: "06-12-2019"
        },
        {
          invoice_id: "INV 012",
          account: "Zachary Wehrley",
          amount: "$ 1400",
          invoice_type: "Onetime",
          status: "Active",
          invoice_date: "10-09-2019",
          due_date: "11-09-2019"
        },
        {
          invoice_id: "INV 013",
          account: "Kyle Wahlert",
          amount: "$ 1200",
          invoice_type: "Onetime",
          status: "Active",
          invoice_date: "01-02-2019",
          due_date: "02-02-2019"
        },
        {
          invoice_id: "INV 014",
          account: "John Subasic",
          amount: "$ 1234",
          invoice_type: "Onetime",
          status: "Active",
          invoice_date: "07-06-2019",
          due_date: "08-06-2019"
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

      const status = exportFile("invoices.csv", content, "text/csv");

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
