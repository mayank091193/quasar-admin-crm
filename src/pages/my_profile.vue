<template>
  <q-page>
    <div class="row q-col-gutter-sm q-ma-xs">
      <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
        <q-card class="my-card" flat bordered>
          <q-card-section horizontal>
            <q-card-section class="q-pt-xs">
              <div class="text-overline">US Region</div>
              <div class="text-h5 q-mt-sm q-mb-xs">Mayank Patel</div>
              <div class="text-caption text-grey">
                Sales and Marketing Executive | graduate and past committee | keynote speaker on Selling and Recruiting
                Topics
              </div>
            </q-card-section>

            <q-card-section class="col-5 flex flex-center">
              <q-img
                class="rounded-borders"
                src="https://cdn.quasar.dev/img/boy-avatar.png"
              />
            </q-card-section>
          </q-card-section>

          <q-separator/>

          <q-card-actions>
            <q-btn flat round icon="event"/>
            <q-btn flat>
              7:30PM
            </q-btn>
            <q-btn flat color="primary">
              Reserve
            </q-btn>
          </q-card-actions>
        </q-card>
      </div>
      <div class="col-lg-6 col-md-6 col-sm-12 col-xs-12">
        <q-card>
          <q-card-section>
            <q-form
              class="q-gutter-md"
            >
              <q-input
                filled
                v-model="user.first_name"
                label="First Name"
                lazy-rules
              />

              <q-input
                filled
                v-model="user.last_name"
                label="Last Name"
                lazy-rules

              />

               <q-input
                filled
                v-model="user.age"
                label="Age"
                lazy-rules

              />

               <q-input
                filled
                v-model="user.phone"
                label="Phone"
                lazy-rules

              />

              <div>
                <q-btn label="Update" type="submit" color="primary"/>
              </div>
            </q-form>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
    import Vue from 'vue';
    import IEcharts from 'vue-echarts-v3/src/full.js';
    import draggable from 'vuedraggable'

    Vue.component('IEcharts', IEcharts);
    Vue.component('draggable', draggable);

    export default {
        name: "dashboard",
        data() {
            return {
                user: {first_name:'Mayank', last_name:'Patel', age: 30, phone: '98******23'},
                age: null,
                accept: false
            }
        },
    }
</script>

<style scoped>

</style>
