<template>
  <div id="right">
    <div id="tabs">
      <div id="tab-add" class="tab" @click="tab='add'" v-bind:style="style_tab_add()">Thêm</div>
      <div id="tab-edit" class="tab" @click="tab='edit'" v-bind:style="style_tab_edit()">Sửa</div>
    </div>
    <template v-if="tab==='add'">
      <div id="add-left">
        <label for="i_new1">Name:</label>
        <label for="i_new2">Birthday:</label>
        <label for="i_new3">Math:</label>
        <label for="i_new4">Literary:</label>
        <label for="i_new5">English:</label>
        <label>Sex:</label>
        <label for="i_new6">Phone:</label>
      </div>
      <div>
        <input type="text" id="i_new1" placeholder="your name" v-model="student_new.name" />
        <input type="text" id="i_new2" placeholder="your birthday" v-model="student_new.birthday" />
        <input type="number" id="i_new3" placeholder="math" v-model="student_new.math" />
        <input type="number" id="i_new4" placeholder="literary" v-model="student_new.literary" />
        <input type="number" id="i_new5" placeholder="english" v-model="student_new.english" />
        <select v-model="student_new.sex">
          <option disabled value>Your sex</option>
          <option>male</option>
          <option>female</option>
        </select>
        <input type="text" id="i_new6" placeholder="your phone" v-model="student_new.phone" />
      </div>
    </template>
    <template v-if="tab==='edit'">
      <div id="add-left">
        <label for="i_new1">Name:</label>
        <label for="i_new2">Birthday:</label>
        <label for="i_new3">Math:</label>
        <label for="i_new4">Literary:</label>
        <label for="i_new5">English:</label>
        <label>Sex:</label>
        <label for="i_new6">Phone:</label>
      </div>
      <div>
        <input type="text" placeholder="your name" v-model="student_choose.name" />
        <input type="text" placeholder="your birthday" v-model="student_choose.birthday" />
        <input type="number" placeholder="math" v-model="student_choose.math" />
        <input type="number" placeholder="literary" v-model="student_choose.literary" />
        <input type="number" placeholder="english" v-model="student_choose.english" />
        <select v-model="student_choose.sex">
          <option disabled value>Your sex</option>
          <option>male</option>
          <option>female</option>
        </select>
        <input type="text" placeholder="your phone" v-model="student_choose.phone" />
      </div>
    </template>
    <button id="save" @click="save_student">Lưu</button>
  </div>
</template>

<script>
import config from "../../config.js";
export default {
  props: ["student_choose"],
  data: function() {
    return {
      tab: "add",
      student_new: {
        _id: "",
        name: "",
        birthday: "",
        math: 0,
        literary: 0,
        english: 0,
        sex: "default",
        phone: ""
      }
    };
  },

  methods: {
    style_tab_add: function() {
      return {
        backgroundColor: this.tab === "edit" ? "#d9d9d9" : "white",
        borderRadius: this.tab === "edit" ? "10px" : "0px"
      };
    },

    style_tab_edit: function() {
      return {
        backgroundColor: this.tab === "add" ? "#d9d9d9" : "white",
        borderRadius: this.tab === "add" ? "13px" : "0px"
      };
    },

    save_student: function() {
      this.tab === "add" ? this.save_new_student() : this.save_edit_student();
    },

    save_new_student: function() {
      const url = config.url + "/api/students/";
      axios
        .post(url, this.student_new, {
          header: {
            accept: "application/json",
            "accept-language": "en_US",
            "content-type": "application/x-www-form-urlencoded"
          }
        })
        .then(res => {
          alert(`Thêm thành công học sinh ${res.data.data.name}`);
          this.emit_event_load_data_to_parent();
        })
        .catch(err => {
          if (err.message === "Network Error") alert("ERROR NETWORK ERROR");
          else alert(`ERROR 400: Bad request`);
        });
    },

    save_edit_student: function() {
      const url = config.url + "/api/students/" + this.student_choose._id;
      axios
        .put(url, this.student_choose, {
          header: {
            accept: "application/json",
            "accept-language": "en_US",
            "content-type": "application/x-www-form-urlencoded"
          }
        })
        .then(res => {
          alert(`Sửa thành công ${res.data.data[0].name}`);
          this.emit_event_load_data_to_parent();
        })
        .catch(err => {
          if (err.message === "Network Error") alert("ERROR NETWORK ERROR");
          else alert(`ERROR 400: Bad request`);
          this.emit_event_load_data_to_parent();
        });
    },

    emit_event_load_data_to_parent: function() {
      this.$emit("load_data");
    }
  },

  watch: {
    student_choose: function() {
      this.tab = "edit";
    }
  }
};
</script>

<style scoped>
#right {
  width: 30%;
  height: 100%;
  margin-left: 70%;
  text-align: center;
}
#tabs {
  width: 100%;
  height: 70px;
  margin-bottom: 70px;
}

.tab {
  width: 50%;
  height: 97%;
  background-color: #d9d9d9;
  font-size: 25px;
  text-align: center;
  line-height: 50px;
  float: left;
}

input {
  width: 60%;
  height: 50px;
  line-height: 50px;
  font-size: 23px;
  padding-left: 15px;
  padding-right: 15px;
  margin: 30px;
  display: block;
}

#add-left {
  width: 30%;
  height: 100%;
  float: left;
}

#add-left label {
  width: 100%;
  height: 67px;
  line-height: 39px;
  display: block;
  font-size: 20px;
  margin-bottom: 20px;
}

select {
  width: 150px;
  height: 40px;
  line-height: 40px;
  font-size: 22px;
}

#save {
  width: 100px;
  height: 60px;
  font-size: 23px;
  background-color: forestgreen;
  border: 1px solid white;
  border-radius: 14px;
}

#save:hover {
  background-color: darkgreen;
}
</style>

//if (
      //   this.student_choose.length === 0 ||
      //   this.student_choose.name.length === 0 ||
      //   this.student_choose.birthday.length === 0 ||
      //   this.student_choose.math === null ||
      //   this.student_choose.math.length === 0 ||
      //   this.student_choose.literary === null ||
      //   this.student_choose.literary.length === 0 ||
      //   this.student_choose.english === null ||
      //   this.student_choose.english.length === 0 ||
      //   this.student_choose.sex === "default" ||
      //   this.student_choose.phone.length === 0
      // ) {
      //   alert("Hãy điền đầy đủ");
      //   this.emit_event_load_data_to_parent();
      //   return;
      // }