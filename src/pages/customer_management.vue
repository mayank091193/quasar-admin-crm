<template>
  <q-page class="q-pa-sm">
    <q-table
      title="Customer Management"
      :data="data"
      :hide-header="mode === 'grid'"
      :columns="columns"
      row-key="name"
      :grid="mode=='grid'"
      :filter="filter"
      :pagination.sync="pagination"
    >
      <template v-slot:top-right="props">
        <q-btn @click="new_customer=true" outline color="primary" label="Add New" class="q-mr-xs" />

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
    </q-table>
    <q-dialog v-model="new_customer">
      <q-card style="width: 600px; max-width: 60vw;">
        <q-card-section>
          <div class="text-h6">
            Add new customer
            <q-btn round flat dense icon="close" class="float-right" color="grey-8" v-close-popup></q-btn>
          </div>
        </q-card-section>
        <q-separator inset></q-separator>
        <q-card-section class="q-pt-none">
          <q-form class="q-gutter-md">
            <q-list>
              <q-item>
                <q-item-section>
                  <q-item-label class="q-pb-xs">Customer Name</q-item-label>
                  <q-input dense outlined v-model="customer.name" label="Customer Name" />
                </q-item-section>
              </q-item>
              <q-item>
                <q-item-section>
                  <q-item-label class="q-pb-xs">City</q-item-label>
                  <q-input dense outlined v-model="customer.city" label="City" />
                </q-item-section>
              </q-item>
              <q-item>
                <q-item-section>
                  <q-item-label class="q-pb-xs">State</q-item-label>
                  <q-input dense outlined v-model="customer.state" label="State" />
                </q-item-section>
              </q-item>
              <q-item>
                <q-item-section>
                  <q-item-label class="q-pb-xs">Last Call</q-item-label>
                  <q-input
                    dense
                    outlined
                    v-model="customer.last_call"
                    mask="date"
                    label="Last Call"
                  >
                    <template v-slot:append>
                      <q-icon name="event" class="cursor-pointer">
                        <q-popup-proxy
                          ref="lastCallProxy"
                          transition-show="scale"
                          transition-hide="scale"
                        >
                          <q-date
                            v-model="customer.last_call"
                            @input="() => $refs.lastCallProxy.hide()"
                          />
                        </q-popup-proxy>
                      </q-icon>
                    </template>
                  </q-input>
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
      customer: {},
      new_customer: false,
      mode: "list",
      columns: [
        {
          name: "name",
          required: true,
          label: "Customer Name",
          align: "left",
          field: "name",
          sortable: true
        },
        {
          name: "city",
          align: "left",
          label: "City",
          field: "city",
          sortable: true
        },
        {
          name: "state",
          align: "left",
          label: "State",
          field: "state",
          sortable: true
        },
        {
          name: "last_call",
          align: "left",
          label: "Last Call",
          field: "last_call",
          sortable: true
        }
      ],
      data: [
        {
          name: "Dr. Jada Conolly",
          city: "GILBERT",
          state: "AZ",
          last_call: "12-09-2019"
        },
        {
          name: "Dr. Kiley Ibbotson",
          city: "LA MESA",
          state: "CA",
          last_call: "09-02-2019"
        },
        {
          name: "Dr. Leslie Tecklenburg",
          city: "SAN DIEGO",
          state: "CA",
          last_call: "03-25-2019"
        },
        {
          name: "Dr. Lia Whitledge",
          city: "PHOENIX",
          state: "AZ",
          last_call: "03-18-2019"
        },
        {
          name: "Dr. Sam Wileman",
          city: "MESA",
          state: "AZ",
          last_call: "04-09-2019"
        },
        {
          name: "Dr. Edgar Colmer",
          city: "PHOENIX",
          state: "AZ",
          last_call: "09-03-2019"
        },
        {
          name: "Dr. Kaiden Rozelle",
          city: "LAKEWOOD",
          state: "CA",
          last_call: "01-12-2019"
        },
        {
          name: "Dr. Leslie Stopher",
          city: "YUMA",
          state: "AZ",
          last_call: "04-15-2019"
        },
        {
          name: "Dr. Miguel Subasic",
          city: "TEMPE",
          state: "AZ",
          last_call: "11-09-2019"
        },
        {
          name: "Dr. Reese Vandygriff",
          city: "LAKEWOOD",
          state: "CA",
          last_call: "01-01-2019"
        },
        {
          name: "Dr. Griffin Troglen",
          city: "YUMA",
          state: "AZ",
          last_call: "04-12-2019"
        },
        {
          name: "Dr. Zachary Wehrley",
          city: "TEMPE",
          state: "AZ",
          last_call: "10-09-2019"
        },
        {
          name: "Dr. Kyle Wahlert",
          city: "LAKEWOOD",
          state: "CA",
          last_call: "01-02-2019"
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

      const status = exportFile("customer-management.csv", content, "text/csv");

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
