<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="desserts"
      sort-by="calories"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>จัดการข้อมูล</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            dark
            class="mb-2"
            @click="openDialog('add', 'defaultItem')"
          >
            เพิ่มข้อมูล
          </v-btn>
        </v-toolbar>
      </template>

      <template v-slot:[`item.actions`]="{ item }">
        <v-btn
          small
          outlined
          class="mr-2"
          @click="openDialog('edit', item)"
          color="blue"
        >
          <v-icon> mdi-pencil </v-icon>
        </v-btn>
        <v-btn small outlined @click="deleteItem(item)" color="red">
          <v-icon> mdi-delete </v-icon>
        </v-btn>
      </template>

      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize"> Reset </v-btn>
      </template>
    </v-data-table>

    <v-dialog v-model="dialogCreate" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="text-h5">{{ formTitle }}</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="6" md="4">
                <v-text-field
                  v-model="editedItem.name"
                  label="ชื่อของหวาน"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-text-field
                  v-model="editedItem.calories"
                  label="แคลลอรี่"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-text-field
                  v-model="editedItem.fat"
                  label="ไขมัน (กรัม)"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-text-field
                  v-model="editedItem.carbs"
                  label="คาร์โบไฮเดต (กรัม)"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-text-field
                  v-model="editedItem.protein"
                  label="โปรตีน (กรัม)"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="close"> ยกเลิก </v-btn>
          <v-btn color="blue darken-1" text @click="save(formTitle)">
            บันทึกข้อมูล
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialogDelete" max-width="500px">
      <v-card>
        <v-card-title class="text-h5"
          >คุณต้องการลบข้อมูลนี้ไช่ไหม?</v-card-title
        >
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red darken-1" text @click="closeDelete">ยกเลิก</v-btn>
          <v-btn color="blue darken-1" text @click="deleteItemConfirm"
            >บันทึก</v-btn
          >
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data: () => ({
    dialogCreate: false,
    dialogDelete: false,
    formTitle: '',
    headers: [
      {
        text: 'ชื่อของหวาน (100g serving)',
        align: 'start',
        sortable: false,
        value: 'name'
      },
      { text: 'แคลลอรี่', value: 'calories' },
      { text: 'ไขมัน (กรัม)', value: 'fat' },
      { text: 'คาร์โบไฮเดต (กรัม)', value: 'carbs' },
      { text: 'โปรตีน (กรัม)', value: 'protein' },
      { text: 'จัดการ', value: 'actions', sortable: false }
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      name: '',
      calories: 0,
      fat: 0,
      carbs: 0,
      protein: 0
    },
    defaultItem: {
      name: '',
      calories: 0,
      fat: 0,
      carbs: 0,
      protein: 0
    },
    formTitle: ''
  }),

  watch: {
    dialog(val) {
      val || this.close()
    },
    dialogDelete(val) {
      val || this.closeDelete()
    }
  },

  created() {
    this.initialize()
  },

  methods: {
    initialize() {
      this.desserts = [
        {
          name: 'Frozen Yogurt',
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0
        },
        {
          name: 'Ice cream sandwich',
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3
        },
        {
          name: 'Eclair',
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0
        },
        {
          name: 'Cupcake',
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3
        },
        {
          name: 'Gingerbread',
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9
        },
        {
          name: 'Jelly bean',
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0
        },
        {
          name: 'Lollipop',
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0
        },
        {
          name: 'Honeycomb',
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5
        },
        {
          name: 'Donut',
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9
        },
        {
          name: 'KitKat',
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7
        }
      ]
    },

    openDialog(Actions, item) {
      //console.log(Actions, item)
      console.log('index item', this.desserts.indexOf(item))
      this.formTitle = ''
      if (Actions === 'add') {
        this.dialogCreate = true
        this.formTitle = 'เพิ่มข้อมูล'
        this.editItem = item
      } else {
        this.dialogCreate = true
        this.formTitle = 'แก้ไขข้อมูล'
        this.editItem = item
        this.editedIndex = this.desserts.indexOf(item)
      }
    },

    editItem(item) {
      consoloe.log('item select', item)
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem(item) {
      this.editedIndex = this.desserts.indexOf(item)
      // this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
      this.editedItem = item
    },

    deleteItemConfirm() {
      this.desserts.splice(this.editedIndex, 1)
      this.closeDelete()
    },

    close() {
      this.dialogCreate = false
      this.editedItem = []
      this.editedIndex = -1
      // this.defaultItem = {
      //   name: '',
      //   calories: 0,
      //   fat: 0,
      //   carbs: 0,
      //   protein: 0
      // }
    },

    save(actions) {
      if (actions === 'เพิ่มข้อมูล') {
        this.desserts.push(this.editItem)
      } else {
        // this.dialogCreate = false
        Object.assign(this.desserts[this.editedIndex], this.editItem)
      }
      this.close()
    },
    closeDelete() {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    }
  }
}
</script>

<style></style>
