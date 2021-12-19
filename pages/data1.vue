<template>
  <v-data-table
    :headers="headers"
    :items="personlist"
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>My CRUD</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
              เพิ่มข้อมูล
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.Cid"
                      label="เลขบัตรประชาชน"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.HN"
                      label="หมายเลขผู้ป่วยนอก"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-select
                      v-model="editedItem.Title"
                      :items="['เด็กชาย', 'เด็กหญิง', 'นาย', 'นาง', 'นางสาว']"
                      label="คำนำหน้า"
                    ></v-select>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.Name"
                      label="ชื่อ"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.Lname"
                      label="นามสกุล"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.Address"
                      label="บ้านเลขที่"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.Moo"
                      label="หมู่ที่"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.Tambon"
                      label="ตำบล"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.Amphur"
                      label="อำเภอ"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.Birthdate"
                      label="วันเกิด"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.TEL"
                      label="เบอร์ติดต่อ"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-select v-model="editedItem.Dead" :items="['เสียชีวิต', 'ยังมีชีวิต']" label="สถานะการเสียชีวิต"></v-select>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      v-model="editedItem.Dead_date"
                      label="วันที่เสียชีวิต"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-select
                      v-model="editedItem.TY_Area"
                      :items="[1, 2, 3, 4]"
                      label="TY_Area"
                    ></v-select>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.D_update"
                      label="D_update"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.Vid"
                      label="Vid"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close"> Cancel </v-btn>
              <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5"
              >Are you sure you want to delete this item?</v-card-title
            >
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete"
                >Cancel</v-btn
              >
              <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                >OK</v-btn
              >
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
      <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize"> Reset </v-btn>
    </template>
  </v-data-table>
</template>

<script>
const url = "http://localhost:5000/api/person";

export default {
  data: () => ({
    
    dialog: false,
    dialogDelete: false,
    headers: [
      { text: "เลขบัตรประชาชน", value: "Cid", width: 200 },
      { text: "หมายเลขผู้ป่วยนอก", value: "HN", width: 200 },
      { text: "คำนำหน้า", value: "Title", width: 120 },
      { text: "ชื่อ", value: "Name", width: 150 },
      { text: "นามสกุล", value: "Lname", width: 150 },
      { text: "บ้านเลขที่", value: "Address", width: 120 },
      { text: "หมู่ที่", value: "Moo", width: 100 },
      { text: "ตำบล", value: "Tambon", width: 150 },
      { text: "อำเภอ", value: "Amphur", width: 200 },
      { text: "ว/ด/ป เกิด", value: "Birthdate", width: 250 },
      { text: "เบอร์ติดต่อ", value: "TEL", width: 200 },
      { text: "Type Area", value: "TY_Area", width: 120 },
      { text: "สถานะการเสียชีวิต", value: "Dead", width: 200 },
      { text: "ว/ด/ป เสียชีวิต", value: "Dead_date", width: 200 },
      { text: "	D_update", value: "	D_update", width: 200 },
      { text: "	Vid", value: "Vid", width: 100 },
      { text: "Actions", value: "actions", sortable: false },
    ],
    personlist: [],
    editedIndex: -1,
    editedItem: {
      Cid: 0,
      HN: 0,
      Titel: "",
      Name: 0,
      Lname: "",
      Address: 0,
      Moo: 0,
      Tambon: "",
      Amphur: "",
      Dead: "",
      Dead_date: Date,
      Birthdate: Date,
      TEL: 0,
      TY_Area: 0,
      D_update: Date,
      Vid: 0,
    },
    defaultItem: {
      Cid: 0,
      HN: 0,
      Titel: "",
      Name: 0,
      Lname: "",
      Address: 0,
      Moo: 0,
      Tambon: "",
      Amphur: "",
      Dead: "",
      Dead_date: Date,
      Birthdate: Date,
      TEL: 0,
      TY_Area: 0,
      D_update: Date,
      Vid: 0,
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created() {
    this.initialize();
  },

  methods: {
    async initialize() {
      const res = await this.$axios.get(url);
      this.personlist = res.data.response;
    },

    editItem(item) {
      this.editedIndex = this.personlist.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = this.personlist.indexOf(item);
      this.editedItem = Object.assign({}, item);
      //console.log(this.editItem);
      this.dialogDelete = true;
    },

    async deleteItemConfirm() {
      const delurl = url + "/" + this.editedItem.Cid;
      try {
        const res = await this.$axios.delete(delurl);
        this.personlist.splice(this.editedIndex, 1);
      } catch (e) {
        console.log(e);
      }
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    async save() {
      if (this.editedIndex > -1) {
        const puturl = url + "/" + this.editedItem.Cid;
        Object.assign(this.personlist[this.editedIndex], this.editedItem);
        //PUT API here
        try {
          const res = await this.$axios.put(puturl, this.editedItem);
        } catch (e) {
          console.log(e);
        }
      } else {
        //POST API here
        this.personlist.push(this.editedItem);
        try {
          const res = await this.$axios.post(url, this.editedItem);
          //initialize();
        } catch (e) {
          console.log(e);
        }
      }
      this.close();
    },
  },
};
</script>