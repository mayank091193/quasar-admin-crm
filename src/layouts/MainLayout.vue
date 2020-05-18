<template>
  <q-layout view="lHh LpR lFf">
    <q-header reveal elevated style="background: linear-gradient(145deg, rgb(74, 94, 137) 15%, rgb(30, 75, 131) 70%);">
      <q-toolbar>
        <q-btn @click="left = !left" flat round dense icon="menu" class="q-mr-sm"/>
        <!--          <q-avatar>-->
        <!--            <img src="https://cdn.quasar.dev/logo/svg/quasar-logo.svg">-->
        <!--          </q-avatar>-->

        <q-toolbar-title>CRM Admin</q-toolbar-title>
        <a style="font-size: 25px;" class="float-right q-mr-sm" href="https://github.com/sponsors/mayank091193"
           target="_blank" title="Donate"><i class="fas fa-heart" style="color: #eb5daa"></i></a>
        <q-btn flat round dense icon="search" class="q-mr-xs"/>
        <q-btn flat round dense icon="fas fa-sign-out-alt" to="/"/>
      </q-toolbar>
    </q-header>
    <q-drawer class="left-navigation text-white"
              show-if-above v-model="left"
              style="background-image: url(https://demos.creative-tim.com/vue-material-dashboard/img/sidebar-2.32103624.jpg) !important;"
              side="left"
              elevated>
      <div style="height: calc(100% - 117px);padding:10px">
        <q-toolbar>
          <q-avatar>
            <img src="https://cdn.quasar.dev/img/boy-avatar.png">
          </q-avatar>

          <q-toolbar-title>Mayank Patel</q-toolbar-title>
        </q-toolbar>
        <hr/>
        <q-scroll-area style="height:100%;">
          <q-list padding>
            <q-item active-class="tab-active" to="/dashboard" exact class="q-ma-sm navigation-item" clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="dashboard"/>
              </q-item-section>

              <q-item-section>
                Dashboard
              </q-item-section>
            </q-item>

            <q-item active-class="tab-active" to="/customer_management" class="q-ma-sm navigation-item" clickable
                    v-ripple>
              <q-item-section avatar>
                <q-icon name="star"/>
              </q-item-section>

              <q-item-section>
                Customer Management
              </q-item-section>
            </q-item>

            <q-item active-class="tab-active" to="/change_request" class="q-ma-sm navigation-item" clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="send"/>
              </q-item-section>

              <q-item-section>
                Change Request
              </q-item-section>
            </q-item>

            <q-item active-class="tab-active" to="/sales_invoices" class="q-ma-sm navigation-item" clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="attach_money"/>
              </q-item-section>

              <q-item-section>
                Sales Invoices
              </q-item-section>
            </q-item>

            <q-item active-class="tab-active" to="/quotes" class="q-ma-sm navigation-item" clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="money"/>
              </q-item-section>

              <q-item-section>
                Quotes
              </q-item-section>
            </q-item>

            <q-item active-class="tab-active" to="/transactions" class="q-ma-sm navigation-item" clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="assignment"/>
              </q-item-section>

              <q-item-section>
                Transactions
              </q-item-section>
            </q-item>

            <q-item active-class="tab-active" to="/employee_salary_list" class="q-ma-sm navigation-item" clickable
                    v-ripple>
              <q-item-section avatar>
                <q-icon name="list"/>
              </q-item-section>

              <q-item-section>
                Employee Salary List
              </q-item-section>
            </q-item>

            <q-item active-class="tab-active" to="/calendar" class="q-ma-sm navigation-item" clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="calendar_today"/>
              </q-item-section>

              <q-item-section>
                Calendar
              </q-item-section>
            </q-item>

            <q-item active-class="tab-active" to="/department" class="q-ma-sm navigation-item" clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="business"/>
              </q-item-section>

              <q-item-section>
                Department
              </q-item-section>
            </q-item>

            <q-item active-class="tab-active" to="/my_profile" class="q-ma-sm navigation-item" clickable v-ripple>
              <q-item-section avatar>
                <q-icon name="drafts"/>
              </q-item-section>

              <q-item-section>
                My Profile
              </q-item-section>
            </q-item>
          </q-list>
        </q-scroll-area>
      </div>
    </q-drawer>

    <q-page-container>
      <q-page class="row no-wrap">
        <div class="col">
          <div class="column full-height">
            <q-scroll-area class="col q-pr-sm" visible>
              <router-view />
            </q-scroll-area>
          </div>
        </div>
      </q-page>
    </q-page-container>


  </q-layout>
</template>

<script>
    export default {
        data() {
            return {
                left: false
            }
        }
    }
</script>

<style>
  .q-drawer {
    /*background-image: url(https://demos.creative-tim.com/vue-material-dashboard/img/sidebar-2.32103624.jpg) !important;*/
    background-image: url('/statics/images/lake.jpg') !important;
    background-size: cover !important;
  }

  .q-drawer__content {
    background-color: rgba(1, 1, 1, 0.75);
  }

  .navigation-item {
    border-radius: 5px;
  }

  .tab-active {
    background-color: green;
  }
</style>
