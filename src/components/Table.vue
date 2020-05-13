<template>
  <div class="q-pa-md">
    <div class="row" style="font-size: 2em">
      <div class="col">
        <q-btn color="primary" icon="compare_arrows" :disable="!(selected.length >= 2)" :label="selected.length >= 2 ? 'COMPARE' : ''" />
      </div>
      <div class="col" v-if="selected.length >= 1">
        <q-btn color="primary" icon="star" @click="handleSelect()">
          <q-tooltip>
            Favorite
          </q-tooltip>
        </q-btn>
        <q-btn color="primary" icon="archive">
          <q-tooltip>
            Archive
          </q-tooltip>
        </q-btn>
        <q-btn color="primary" icon="report">
          <q-tooltip>
            Report
          </q-tooltip>
        </q-btn>
      </div>
      <div class="col">
        <div v-if="$q.screen.gt.xs" class="col">
          <q-btn-dropdown auto-close stretch flat label="Columns">
            <q-list>
              <q-item>
                <q-toggle v-model="visibleColumns" val="calories" label="Calories" />
              </q-item>
              <q-item>
                <q-toggle v-model="visibleColumns" val="fat" label="Fat" />
              </q-item>
              <q-item>
                <q-toggle v-model="visibleColumns" val="carbs" label="Carbs" />
              </q-item>
              <q-item>
                <q-toggle v-model="visibleColumns" val="protein" label="Protein" />
              </q-item>
              <q-item>
                <q-toggle v-model="visibleColumns" val="sodium" label="Sodium" />
              </q-item>
              <q-item>
                <q-toggle v-model="visibleColumns" val="calcium" label="Calcium" />
              </q-item>
              <q-item>
                <q-toggle v-model="visibleColumns" val="iron" label="Iron" />
              </q-item>
            </q-list>
          </q-btn-dropdown>
        </div>
        <q-select
          v-else
          v-model="visibleColumns"
          multiple
          borderless
          dense
          options-dense
          :display-value="$q.lang.table.columns"
          emit-value
          map-options
          :options="columns"
          option-value="name"
          style="min-width: 150px"
        />
      </div>
    </div>

    <q-table
      :data="data"
      :columns="columns"
      row-key="name"
      :selected-rows-label="getSelectedString"
      selection="multiple"
      :selected.sync="selected"
      :visible-columns="visibleColumns"
    >

      <template v-slot:body-cell-desc="props">
        <q-td :props="props">
          <div>
            {{ props.value }}
            <q-icon v-if="props.row.favorite" size="sm" name="star" style="color:#FFDF00" />
          </div>
        </q-td>
      </template>

    </q-table>
  </div>
</template>

<script>
  export default {
    name: 'Table',
    data () {
      return {
        selected: [],
        visibleColumns: [ 'calories', 'desc', 'fat', 'carbs', 'protein', 'sodium', 'calcium', 'iron' ],
        columns: [
          {
            name: 'desc',
            required: true,
            label: 'Dessert (100g serving)',
            align: 'left',
            field: row => row.name,
            format: val => `${val}`,
            sortable: true
          },
          { name: 'calories', align: 'center', label: 'Calories', field: 'calories', sortable: true },
          { name: 'fat', label: 'Fat (g)', field: 'fat', sortable: true },
          { name: 'carbs', label: 'Carbs (g)', field: 'carbs' },
          { name: 'protein', label: 'Protein (g)', field: 'protein' },
          { name: 'sodium', label: 'Sodium (mg)', field: 'sodium' },
          { name: 'calcium', label: 'Calcium (%)', field: 'calcium', sortable: true, sort: (a, b) => parseInt(a, 10) - parseInt(b, 10) },
          { name: 'iron', label: 'Iron (%)', field: 'iron', sortable: true, sort: (a, b) => parseInt(a, 10) - parseInt(b, 10) }
        ],
        data: [
          {
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            sodium: 87,
            calcium: '14%',
            iron: '1%',
            favorite: false
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            sodium: 129,
            calcium: '8%',
            iron: '1%',
            favorite: false
          },
          {
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            sodium: 337,
            calcium: '6%',
            iron: '7%',
            favorite: false
          },
          {
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
            sodium: 413,
            calcium: '3%',
            iron: '8%',
            favorite: false
          },
          {
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
            sodium: 327,
            calcium: '7%',
            iron: '16%',
            favorite: false
          },
          {
            name: 'Jelly bean',
            calories: 375,
            fat: 0.0,
            carbs: 94,
            protein: 0.0,
            sodium: 50,
            calcium: '0%',
            iron: '0%',
            favorite: false
          },
          {
            name: 'Lollipop',
            calories: 392,
            fat: 0.2,
            carbs: 98,
            protein: 0,
            sodium: 38,
            calcium: '0%',
            iron: '2%',
            favorite: false
          },
          {
            name: 'Honeycomb',
            calories: 408,
            fat: 3.2,
            carbs: 87,
            protein: 6.5,
            sodium: 562,
            calcium: '0%',
            iron: '45%',
            favorite: false
          },
          {
            name: 'Donut',
            calories: 452,
            fat: 25.0,
            carbs: 51,
            protein: 4.9,
            sodium: 326,
            calcium: '2%',
            iron: '22%',
            favorite: false
          },
          {
            name: 'KitKat',
            calories: 518,
            fat: 26.0,
            carbs: 65,
            protein: 7,
            sodium: 54,
            calcium: '12%',
            iron: '6%',
            favorite: false
          }
        ]
      }
    },
    methods: {
      getSelectedString() {
        return this.selected.length === 0 ? '' : `${this.selected.length} record${this.selected.length > 1 ? 's' : ''} selected of ${this.data.length}`;
      },
      handleSelect() {
        this.selected.forEach((item) => {
          item.favorite = !item.favorite;
        });
      }
    }
  }
</script>
