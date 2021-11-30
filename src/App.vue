<template>
  <div id="app">
    <div class="container">
      <h1>Chọn tỉnh</h1>
      <div class="header--select" @click="showOption">
        Chon tinh thanh
        <i class="fas fa-caret-down" style="float:right; color:black"></i>
      </div>
      <div v-if="isShowOption" class="body--select">
        <div
          v-for="(city, index) in listCity"
          :key="index"
          class="option--select"
        >
          <label class="checkbox">
            <input
              type="checkbox"
              class="checkbox--input"
              v-model="listCity[index].checked"
            />
            <div class="checkbox--box"></div>
            <p style="margin:5px 0 0">{{ city.name }}</p>
          </label>
        </div>
      </div>
      <div v-else>
        <div class="list--checked">
          <div v-for="(city, index) in listCheckedCity" :key='index' class="city--item">
            <p>{{ city.name }}</p>
            <i class="fas fa-times" @click="removeCity(city.name)"></i>
          </div>
        </div>
      </div>

      <div class="box--button" v-if="isShowOption">
        <button
          :disabled="disableButton"
          class="button button--success"
          @click="filterCity"
          :style="
            disableButton && { backgroundColor: '#dcdcdc', border: 'none' }
          "
        >
          Đồng ý
        </button>
        <button class="button button--cancel" @click="showOption">Hủy</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      listCity: [],
      isShowOption: false,
      disableButton: true,
      listCheckedCity: []
    };
  },
  mounted: async function() {
    try {
      let res = await fetch("https://provinces.open-api.vn/api/", {
        method: "GET"
      });
      let data = await res.json();
      data.forEach((city, index) => {
        data[index].checked = false;
      });
      this.listCity = data;
    } catch (error) {
      console.log(error);
    }
  },
  watch: {
    listCity: {
      handler: function() {
        let index = this.listCity.findIndex(element => element.checked == true);
        if (index == -1) this.disableButton = true;
        else this.disableButton = false;
      },
      deep: true
    }
  },
  methods: {
    showOption() {
      this.isShowOption = !this.isShowOption;
    },
    removeCity(name) {
     
      let index = this.listCheckedCity.findIndex(city => city.name == name);
      this.listCheckedCity.splice(index,1)
      let idx = this.listCity.findIndex(city => city.name == name);
      console.log(idx)
      this.listCity[idx].checked=false
    
    },
    filterCity() {
      this.listCheckedCity = this.listCity.filter(
        element => element.checked == true
      );
      this.showOption();
    }
  }
};
</script>

<style>
.container {
  margin: auto;
  width: 50%;
  width: 450px;
}
.header--select {
  border: 1px solid #9595a2;
  height: 30px;
  padding: 10px 10px 0px 10px;
  color: #b2b2b2;
  border-radius: 5px;
  cursor: pointer;
}
.body--select {
  border: 1px solid #9595a2;
  max-height: 310px;
  overflow-y: scroll;
  position: relative;
  border-bottom: none;
}
.option--select {
  height: 35px;
  padding: 10px 10px 0;
}
.option--select:hover {
  background-color: #e7f1fd;
}
.checkbox {
  display: inline-flex;
  cursor: pointer;
  width: 100%;
}
.checkbox--input {
  display: none;
}
.checkbox--box {
  height: 1.25em;
  width: 1.25em;
  border: 2px solid #cccccc;
  border-radius: 3px;
  margin-right: 10px;
  align-items: center;
  display: flex;
  justify-content: center;
}
.checkbox--box::after {
  content: "\2714";
  color: #ffffff;
}
.checkbox--input:checked + .checkbox--box {
  background-color: #45d1c9;
  border-color: #45d1c9;
}
.box--button {
  background-color: white;
  border: 1px solid #9595a2;
  border-top: none;
  padding: 10px;
}
.button {
  width: 80px;
  height: 30px;
  cursor: pointer;
  font-size: 17px;
}
.button--cancel {
  background-color: white;
  border: none;
  color: #007bc3;
}
.button--success {
  background-color: #007bc3;
  color: white;
  border: 1px solid #007bc3;
  border-radius: 5px;
  font-weight: 700;
}
.list--checked {
  border: 1px solid #9595a2;
  min-height: 30px;
  padding: 10px 10px 0px 10px;
  border-radius: 5px;
  display: flex;
  flex-wrap: wrap;
}
.city--item {
  background-color: #dcdcdc;
  border-radius: 10px;
  display: flex;
  margin-bottom: 5px;
  margin-right: 5px;
}
.city--item p {
  margin: 6px;
}
.city--item i {
  margin: 6px;
  cursor: pointer;
}
</style>
