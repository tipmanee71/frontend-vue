<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="employeeItem"
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
      <template v-slot:[`item.role`]="{ item }">
        {{ item.role.name }}
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
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="productId"
                  label="productId
"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="productName"
                  label="productName"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="productPrice"
                  label=" productPrice"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="productAmount"
                  label="productAmount"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="productDetail"
                  label="productDetail"
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
    productId: '',
    productName: '',
    productPrice: '',
    productAmount: '',
    productDetail: '',
    dialogCreate: false,
    dialogDelete: false,
    formTitle: '',
    idProduct: '',
    idForDelete: '',
    headers: [
      {
        text: 'ไอดี',
        align: 'start',
        sortable: false,
        value: 'productId'
      },
      { text: 'productId', value: 'productId' },
      { text: 'productName', value: 'productName' },
      { text: 'productPrice', value: 'productPrice' },
      { text: 'productAmount', value: 'productAmount' },
      { text: 'productDetail', value: 'productDetail' },
      { text: 'จัดการ', value: 'actions', sortable: false }
    ],

    employeeItem: [],
    editedIndex: -1,

    editedItem: {
      productId: '',
      productName: '',
      productPrice: '',
      productAmount: '',
      productDetail: ''
    },

    defaultItem: {
      productId: '',
      productName: '',
      productPrice: '',
      productAmount: '',
      productDetail: ''
    }
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
    async initialize() {
      this.employeeItem = []
      try {
        var data = await this.axios.get('http://localhost:9000/product')
        console.log('data employee ===>', data)
        this.employeeItem = data.data
      } catch (error) {
        console.log(error.message)
      }
    },

    openDialog(Actions, item) {
      //console.log(Actions, item)
      console.log('index item', this.employeeItem.indexOf(item))
      this.formTitle = ''

      if (Actions === 'add') {
        this.dialogCreate = true
        this.formTitle = 'เพิ่มข้อมูล'
        // this.editedItem = item
      } else {
        //console.log(item)
        this.dialogCreate = true
        this.formTitle = 'แก้ไขข้อมูล'
        // this.editedItem = item
        // this.editedIndex = this.employeeItem.indexOf(item)
        this.productId = item.productId
        this.productName = item.productName
        this.productPrice = item.productPrice
        this.productDetail = item.role.productDetail
        this.productAmount = item.role.productAmount
        this.idProduct = item.productId
      }
    },

    editItem(item) {
      consoloe.log('item select', item)
      consoloe.log('index item', this.employeeItem.indexOf(item))
      // this.editedIndex = this.employeeItem.indexOf(item)
      //this.editedItem = Object.assign({}, item)
      //this.dialog = true
    },

    deleteItem(item) {
      //this.editedIndex = this.employeeItem.indexOf(item)
      // this.editedItem = Object.assign({}, item)
      //this.editedItem = item
      this.idForDelete = item.productId
      this.dialogDelete = true
    },

    async deleteItemConfirm() {
      try {
        var response = await this.axios.delete(
          'http://localhost:9000/product/' + this.idForDelete
        )
        this.initialize()
      } catch (error) {
        console.log(error.message)
      }
      this.closeDelete()
    },

    close() {
      this.dialogCreate = false
      //   this.productId = ''
      //   this.productName = ''
      //   this.productPrice = ''
      //   this.productAmount = ''
      //   this.productDetail = ''
    },

    async save(actions) {
      var data = {
        productId: this.productId,
        productName: this.productName,
        productPrice: this.productPrice,
        productDetail: this.productDetail,
        productAmount: this.productAmount
      }

      if (actions === 'เพิ่มข้อมูล') {
        // this.employeeItem.push(this.editedItem)
        // this.close()
        // console.log('data after send===>', data)
        try {
          var dataResponse = await this.axios.post(
            'http://localhost:9000/product',
            data
          )
          console.log('dataResponse ===>', dataResponse)
          this.close()
          this.initialize()
        } catch (error) {
          console.log(error.message)
        }
      } else {
        try {
          var dataResponse = await this.axios.put(
            'http://localhost:9000/product/' + this.productId,
            data
          )

          console.log('dataResponse ===>', dataResponse)
          this.close()
          this.initialize()
        } catch (error) {
          console.error('PUT request failed:', error)
          console.log(error.message)
        }
        this.close()

        // this.dialogCreate = false
        //Object.assign(this.desserts[this.editedIndex], this.editedItem)
        //this.close()
      }
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
