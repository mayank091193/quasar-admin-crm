<template>
  <q-page>
    <h5 class="q-my-xs q-pt-sm q-ml-md text-black">Department</h5>
    <div class="row q-col-gutter-sm q-ma-xs q-mr-sm">
      <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
        <q-card flat bordered class="">
          <q-card-section class="row">
            <div class="text-h6 col-12">Account
            </div>
          </q-card-section>

          <q-separator inset></q-separator>

          <q-card-section>
            <q-table
              :data="account_data"
              :columns="columns"
              row-key="name"
              :pagination.sync="pagination"
            >
              <template v-slot:body-cell-action="props">
                <q-td :props="props">
                  <div class="q-gutter-sm">
                    <q-btn dense color="primary" icon="edit"/>
                    <q-btn dense color="red" icon="delete"/>
                  </div>
                </q-td>
              </template>
            </q-table>
          </q-card-section>
        </q-card>
      </div>
      <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
        <q-card flat bordered class="">
          <q-card-section class="row">
            <div class="text-h6 col-12">Finance
            </div>
          </q-card-section>

          <q-separator inset></q-separator>

          <q-card-section>
            <q-table
              :data="finance_data"
              :columns="columns"
              row-key="name"
              :pagination.sync="pagination"
            >
              <template v-slot:body-cell-action="props">
                <q-td :props="props">
                  <div class="q-gutter-sm">
                    <q-btn dense color="primary" icon="edit"/>
                    <q-btn dense color="red" icon="delete"/>
                  </div>
                </q-td>
              </template>
            </q-table>
          </q-card-section>
        </q-card>
      </div>
      <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
        <q-card flat bordered class="">
          <q-card-section class="row">
            <div class="text-h6 col-12">Human Resources
            </div>
          </q-card-section>

          <q-separator inset></q-separator>

          <q-card-section>
            <q-table
              :data="hr_data"
              :columns="columns"
              row-key="name"
              :pagination.sync="pagination"
            >
              <template v-slot:body-cell-action="props">
                <q-td :props="props">
                  <div class="q-gutter-sm">
                    <q-btn dense color="primary" icon="edit"/>
                    <q-btn dense color="red" icon="delete"/>
                  </div>
                </q-td>
              </template>
            </q-table>
          </q-card-section>
        </q-card>
      </div>
      <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
        <q-card flat bordered class="">
          <q-card-section class="row">
            <div class="text-h6 col-12">Information Technology
            </div>
          </q-card-section>

          <q-separator inset></q-separator>

          <q-card-section>
            <q-table
              :data="it_data"
              :columns="columns"
              row-key="name"
              :pagination.sync="pagination"
            >
              <template v-slot:body-cell-action="props">
                <q-td :props="props">
                  <div class="q-gutter-sm">
                    <q-btn dense color="primary" icon="edit"/>
                    <q-btn dense color="red" icon="delete"/>
                  </div>
                </q-td>
              </template>
            </q-table>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
    export default {
        data() {
            return {
                columns: [
                    {
                        name: "serial_no",
                        align: "left",
                        label: "Serial No.",
                        field: "serial_no",
                        sortable: true
                    },
                    {
                        name: "designation",
                        align: "left",
                        label: "Designation",
                        field: "designation",
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
                finance_data: [
                    {
                        serial_no: "01",
                        designation: "Admin",
                    },
                    {
                        serial_no: "02",
                        designation: "Staff",
                    },
                    {
                        serial_no: "03",
                        designation: "Admin",
                    }
                ],
                account_data: [
                    {
                        serial_no: "01",
                        designation: "Senior Account",
                    },
                    {
                        serial_no: "02",
                        designation: "Manager Account",
                    },
                    {
                        serial_no: "03",
                        designation: "Manager",
                    }
                ],
                hr_data: [
                    {
                        serial_no: "01",
                        designation: "Manager",
                    },
                    {
                        serial_no: "02",
                        designation: "Department Head",
                    },
                    {
                        serial_no: "03",
                        designation: "assistant",
                    }
                ],
                it_data: [
                    {
                        serial_no: "01",
                        designation: "Software developer",
                    },
                    {
                        serial_no: "02",
                        designation: "Grapics designer",
                    },
                    {
                        serial_no: "03",
                        designation: "Tester",
                    }
                ],
                pagination: {
                    rowsPerPage: 5
                }
            }
        }
    }
</script>
