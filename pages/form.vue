<template>
  <div class="text-center" :items="personlist">
    <br />
    <h1>แบบฟอร์มสำหรับกรอกข้อมูล</h1>

    <v-card flat>
      <v-form ref="form" @submit.prevent="submit">
        <v-container fluid>
          <v-row>
            <!-- CID -->
            <v-col cols="6" sm="6">
              <v-text-field
                v-model="editedItem.Cid"
                label="เลขบัตรประจำตัวประชาชน"
                required
              ></v-text-field>
            </v-col>

            <!-- HN -->
            <v-col cols="6" sm="6">
              <v-text-field 
                v-model="editedItem.HN"
                label="เลขผู้ป่วยนอก" 
                required></v-text-field>
            </v-col>

            <!-- Title / Prename -->
            <v-col cols="3" sm="2">
              <v-combobox
                v-model="editedItem.Title"
                :items="['เด็กชาย', 'เด็กหญิง', 'นาย', 'นาง', 'นางสาว']"
                label="คำนำหน้าชื่อ"
              ></v-combobox>
            </v-col>

            <!-- First Name -->
            <v-col cols="4" sm="5">
              <v-text-field
                v-model="editedItem.Name"
                color="purple darken-2"
                label="ชื่อจริง"
              ></v-text-field>
            </v-col>

            <!-- Last Name -->
            <v-col cols="4" sm="5">
              <v-text-field
                v-model="editedItem.Lname"
                color="purple darken-2"
                label="นามสกุล"
              ></v-text-field>
            </v-col>

            <!-- Address -->
            <v-col cols="2" sm="1">
              <v-text-field
                v-model="editedItem.Adress"
                color="purple darken-2"
                label="บ้านเลขที่"
              ></v-text-field>
            </v-col>

            <!-- Moo -->
            <v-col cols="2" sm="1">
              <v-combobox
                v-model="editedItem.Moo"
                :items="[1, 2, 3, 4, 5, 6, 7]"
                label="หมู่"
              ></v-combobox>
            </v-col>

            <!-- Village -->
            <v-col cols="3" sm="2">
              <v-combobox
                v-model="editedItem.Vid"
                :items="[
                  1, 2, 3
                ]"
                label="ชื่อหมู่บ้าน"
              ></v-combobox>
            </v-col>

            <!-- PCC 
            <v-col cols="3" sm="2">
              <v-combobox
                v-model=""
                :items="[1, 2, 3, 4]"
                label="เขตรับผิดชอบ (PCC)"
              ></v-combobox>
            </v-col>-->

            <!-- Amphur -->
            <v-col cols="4" sm="4">
              <v-combobox
                v-model="editedItem.Amphur"
                :items="[
                  'เมืองเชียงราย',
                  'เวียง',
                  'รอบเวียง',
                  'บ้านดู่',
                  'นางแล',
                  'แม่ข้าวต้ม',
                  'แม่ยาว',
                ]"
                label="อำเภอ"
              ></v-combobox>
            </v-col>

            <!-- Type area -->
            <v-col cols="2" sm="2">
              <v-combobox
                v-model="editedItem.TY_Araea"
                :items="['1', '2', '3', '4']"
                label="Type area"
              ></v-combobox>
            </v-col>

            <!-- Tel -->
            <v-col cols="3" sm="3">
              <v-text-field 
                v-model="editedItem.TEL"
                label="เบอร์โทรศัพท์"
              ></v-text-field>
            </v-col>

            <!-- birhtdate -->
            <v-col cols="3" sm="3">
              <v-menu
                ref="birthdate"
                v-model="birthdate"
                :close-on-content-click="false"
                :return-value.sync="b_date"
                transition="scale-transition"
              >
                <template v-slot:activator="{ on }">
                  <v-text-field
                    v-model="b_date"
                    label="วันที่เกิด"
                    prepend-icon="mdi-calendar"
                    readonly
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="b_date" no-title scrollable>
                  <v-spacer></v-spacer>
                  <v-btn text color="primary" @click="birthdate = false">
                    Cancel
                  </v-btn>
                  <v-btn
                    text
                    color="primary"
                    @click="$refs.birthdate.save(b_date)"
                  >
                    OK
                  </v-btn>
                </v-date-picker>
              </v-menu>
            </v-col>

            <!-- status -->
            <v-col cols="3" sm="3">
              <v-combobox
                v-model="editedItem.Dead"
                :items="['เสียชีวิต', 'ยังมีชีวิตอยู่']"
                label="สถานะการมีอยู่"
              ></v-combobox>
            </v-col>

            <!-- dead date -->
            <v-col cols="3" sm="3">
              <v-menu
                ref="deaddate"
                v-model="deaddate"
                :close-on-content-click="false"
                :return-value.sync="d_date"
                transition="scale-transition"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="d_date"
                    label="วันที่เสียชีวิต"
                    prepend-icon="mdi-calendar"
                    readonly
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="d_date" no-title scrollable>
                  <v-spacer></v-spacer>
                  <v-btn text color="primary" @click="deaddate = false">
                    Cancel
                  </v-btn>
                  <v-btn
                    text
                    color="primary"
                    @click="$refs.deaddate.save(d_date)"
                  >
                    OK
                  </v-btn>
                </v-date-picker>
              </v-menu>
            </v-col>
          </v-row>
        </v-container>
      </v-form>
    </v-card>

    <v-btn elevation="3" plain color="blue darken-5" @click="snackbar = true">
      บันทึกข้อมูล
      <v-snackbar :timeout="timeout" color="success" text outlined top right dense>
      <strong>{{ text }}</strong>
    </v-snackbar>
    </v-btn>
    <!-- <v-btn color="blue darken-1" text @click="save"> Save </v-btn> -->
  </div>
</template>

<script>

const url = "http://localhost:5000/api/person";

// function birth() {
//   var date = new Date();
//   date.toISOString().slice(0, 19).replace('T', ' ');
// }



export default {
  data: () => ({
    b_date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),

    d_date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    d_date: null,
    b_date: null,
    birthdate: false,
    deaddate: false,

    snackbar: false,
    text: 'บันทึกข้อมูลเรียบร้อย',
    timeout: 1000,

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
      Dead_date: "",
      Birthdate: null,
      TEL: 0,
      TY_Araea: 0,
      D_update: "",
      Vid: 0,
      
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
  },

  methods: {
    async snackbar() {
      
      this.personlist.push(this.editedItem);
      try {
        const res = await this.$axios.post(url, this.editedItem, this.b_date, this.d_date);
      }catch (e){
        console.log(e)
      }
      //this.close();
    },
  },
};
</script>