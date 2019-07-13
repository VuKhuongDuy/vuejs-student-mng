<template>
  <div id="left">
    <div id="search">
      <input
        type="text"
        class="inputSearch"
        v-model="name_student_search"
        v-on:keyup.enter="doSearch"
        placeholder="Tìm tên học sinh"
      />
      <button id="tab-delete" @click="click_delete">Xóa</button>
    </div>
    <div id="list-student">
      <table>
        <tr style="position: 'fixed'">
          <th v-for="field in fields" class="th col" :key="field">{{field}}</th>
        </tr>
        <tr
          v-for="(student,index) in students_all"
          :style="style_row_student(index)"
          :key="student.id"
          class="row"
          @click="click_student(student, index)"
        >
          <td class="td">{{student.name}}</td>
          <td class="td">{{student.birthday}}</td>
          <td class="td">{{student.math}}</td>
          <td class="td">{{student.literary}}</td>
          <td class="td">{{student.english}}</td>
          <td class="td">{{student.sex}}</td>
          <td class="td">{{student.phone}}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import config from "../../config";
export default {
  props: ["load_data"],
  data: function() {
    return {
      fields: [
        "Name",
        "Birthday (mm/dd/yyyy)",
        "Math",
        "Literary",
        "English",
        "Sex",
        "Phone"
      ],
      name_student_search: "",
      student_choose: [],
      students_all: [],
      index_student_choose: 0,
      reRender: 0
    };
  },

  methods: {
    style_row_student: function(index) {
      return {
        backgroundColor: index === this.index_student_choose ? "#4d4d4d" : "",
        color: index === this.index_student_choose ? "white" : "black"
      };
    },

    doSearch: function(event) {
      const url = config.url + "/api/students/" + event.target.value;
      axios
        .get(url)
        .then(
          function(res) {
            this.students_all = res.data.data;
            this.convert_string_birthday(this.students_all);
          }.bind(this)
        )
        .catch(function(err) {
          if (err.message === "Network Error") alert("ERROR NETWORK ERROR");
          else alert(`ERROR 400: Bad request`);
        });
    },

    click_delete: function() {
      if (this.student_choose.length === 0) {
        alert("Hãy chọn 1 học sinh để thực hiện xóa.");
        return;
      }
      const willDelete = confirm(
        `Bạn sẽ xóa học sinh có tên là: ${this.student_choose.name} ?`
      );
      if (willDelete && this.student_choose.length != 0) {
        this.delete_data_students(this.student_choose._id);
      }
    },

    click_student: function(student, index) {
      this.student_choose = student;
      this.index_student_choose = index;
      this.$emit("click_student", student);
    },

    get_data_students: function() {
      const url = config.url + "/api/students";
      axios
        .get(url)
        .then(
          function(res) {
            this.students_all = res.data.data;
            this.convert_string_birthday(this.students_all);
          }.bind(this)
        )
        .catch(err => {
          if (err.message === "Network Error") alert("ERROR NETWORK ERROR");
          else alert(`ERROR 400: Bad request`);
        });
    },

    delete_data_students: function(id) {
      const url = config.url + "/api/students/" + id;
      axios
        .delete(url)
        .then(
          function(res) {
            alert("Xóa thành công");
            this.students_all = res.data.data;
            this.convert_string_birthday(this.students_all);
            this.student_choose = this.students_all[this.index_student_choose];
          }.bind(this)
        )
        .catch(err => {
          if (err.message === "Network Error") alert("ERROR NETWORK ERROR");
          else alert(`ERROR 400: Bad request`);
        });
    },

    convert_string_birthday: function(arr_student) {
      arr_student.forEach(student => {
        let birthday = new Date(student.birthday);
        console.log(student.birthday + "-------" + birthday.getMonth());
        let strBirthday =
          Number(birthday.getMonth()) +
          1 +
          "/" +
          birthday.getDate() +
          "/" +
          birthday.getFullYear();
        student.birthday = strBirthday;
      });
    }
  },

  watch: {
    load_data: function() {
      this.get_data_students();
    }
  },

  mounted: function() {
    this.get_data_students();
  }
};
</script>

<style scoped>
#left {
  width: 70%;
  height: 100%;
  background-color: #d9d9d9;
  box-shadow: gray 0px 0px 10px;
  float: left;
}

#search {
  width: 100%;
  height: 70px;
  text-align: center;
  padding-top: 10px;
}

#list-student {
  width: 100%;
  height: 84%;
  overflow: auto;
}

.inputSearch {
  width: 300px;
  height: 40px;
  line-height: 40px;
  font-size: 19px;
  padding-left: 7px;
  padding-right: 7px;
}

#tab-delete {
  width: 100px;
  height: 60px;
  font-size: 23px;
  margin-left: 740px;
  border-radius: 14px;
  background-color: #990000;
}

#tab-delete:hover {
  background-color: #ff1a1a;
}

table {
  width: 100%;
}
/* 
.row:hover {
  background-color: #4d4d4d;
  color: white;
} */

.th {
  background-color: darkgray;
  text-align: center;
}

.col {
  width: 10%;
  height: 40px;
  line-height: 35px;
}

.col:nth-child(1),
.col:nth-child(2),
.col:nth-child(7) {
  width: 20%;
}

.td {
  height: 40px;
  text-align: right;
}

.td:nth-child(1) {
  text-align: left;
}

.td:nth-child(3),
.td:nth-child(4),
.td:nth-child(5),
.td:nth-child(6) {
  text-align: center;
}
</style>

