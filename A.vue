<template>
  <div class="home">
    <button>{{ dis }}</button>
    <br />
    <span>所在城市</span>
    <select v-model="city">
      <option v-for="(item, index) in cityList" :key="index">
        {{ item.name }}
      </option>
    </select>
    <br />
    <span>用户名:</span>
    <input type="text" v-model="name" />
    <br />
    <span>年龄:</span>
    <input type="text" v-model="age" />
    <br>
    <p>
      用户名搜索:<input
        type="text"
        placeholder="输入关键字"
        @input="searchs"
        v-model="search"
      />
    </p>
    <p>
      城市搜索:<input
        type="text"
        placeholder="输入关键字"
        @input="searchscity"
        v-model="cityss"
      />
    </p>
    <p>
      年龄搜索:
      <input type="text" placeholder="输入最小年龄" v-model.number="Xage" />
      -
      <input type="text" placeholder="输入最大年龄" v-model.number="Dage" />
      <button  @click="searchage">搜索</button>
      <button  @click="Reset">重置</button>
      <br />
    </p>
    <button @click="add">{{ addBtn }}</button>
      <button @click="remove">选中删除</button>
    <br />
    

    <table border="1" style="border-collapse: collapse" width="800">
      <tr>
        <td>选择</td>
        <td>ID</td>
        <td>用户名</td>
        <td @click="ageSort">年龄{{ ages }}</td>
        <td>所在城市<span @click="CitySort">|排序</span></td>
        <td>操作</td>
      </tr>
      <tr
        align="center"
        v-for="(item, index) in goodsInfo"
        :key="index"
        :class="{ even: index % 2 == 0, odd: index % 2 !== 0 }"
      >
        <th>
          <input type="checkbox" :checked="item.check" @change="change(item)" />
        </th>
        <th>{{ item.id }}</th>
        <th>{{ item.userName }}</th>
        <th>{{ item.age }}</th>
        <th>{{ item.city }}</th>
        <th>
          <button @click="change(index)">修改</button>
          <button @click="del(index)">删除</button>
        </th>
      </tr>
    </table>
  </div>
</template>

<script>
import cityList from "@/assets/data.json";
export default {
  name: "Home",
  components: {},
  data() {
    return {
      addBtn: "提交",
      dis: "锁定",
      cityList: cityList.data,
      name: "",
      age: "",
      city: "",
      goodsInfo: [],
      DataList2: [],
      num: 0,
      Index: -1,
      disabled: false,
      res: 3,
      ages: "👆",
      search: "",
      cityss: "",
      Dage: "",
      Xage: "",
    };
  },
  methods: {
    //提交
    add() {
      // if (this.userName == "" || this.age == "" || this.city == "") {
      //   alert("任何文本框都不能为空");
      // } else {
      if (this.addBtn == "提交") {
        this.num++;
        let obj = {
          id: this.num,
          userName: this.name,
          age: this.age,
          city: this.city,
          check: false,
        };
        this.goodsInfo.push(obj);
        this.DataList2.push(obj)
      } else {
        let obj = {
          id: this.goodsInfo[this.Index].id,
          userName: this.name,
          age: this.age,
          city: this.city,
          check: false,
        };
        this.goodsInfo.splice(this.Index, 1, obj);
        this.addBtn = "提交";
      }

      this.name = "";
      this.age = "";
      this.city = "";
      // }
    },
    //删除
    del(index) {
      this.goodsInfo.splice(index, 1);
    },
    //修改
    change(index) {
      this.Index = index;
      this.name = this.goodsInfo[index].userName;
      this.age = this.goodsInfo[index].age;
      this.city = this.goodsInfo[index].city;
      this.addBtn = "确认修改";
    },
    //选中删除
    remove() {
      this.goodsInfo = this.goodsInfo.filter((item) => {
        if (item.check == false) {
          return item;
        }
      });
    },
    change(item) {
      if (item.check == false) {
        item.check = true;
      } else if (item.check == true) {
        item.check = false;
      }
    },
    //年龄排序
    ageSort() {
      if (this.ages == "👆") {
        this.goodsInfo.sort((a, b) => {
          return b.age - a.age;
        });
        this.ages = "👇";
      } else if (this.ages == "👇") {
        this.goodsInfo.sort((a, b) => {
          return a.age - b.age;
        });
        this.ages = "👆";
      }
    },
    //用户名搜索
    searchs() {
      this.goodsInfo = this.DataList2.filter((item) => {
        if (item.userName.includes(this.search)) {
          return item;
        }
      });
    },
    //城市搜索
    searchscity() {
      this.goodsInfo = this.DataList2.filter((item) => {
        if (item.city.includes(this.cityss)) {
          return item;
        }
      });
    },
    //年龄搜索
    searchage() {
      this.goodsInfo = this.DataList2.filter((item) => {
        return item.age >= this.Xage && item.age <= this.Dage;
      });
    },
    //重置
    Reset() {
      this.goodsInfo = this.DataList2;
      this.Dage = "";
      this.Xage = "";
    },
    //城市排序
    CitySort() {
      this.goodsInfo = this.DataList2.sort((item1, item2) => {
        return item1.city.localeCompare(item2.city);
      });
      console.log(this.goodsInfo);
    },
  },
};
</script>
<style lang="scss">
.even {
  background-color: aqua;
}
.odd {
  text-align: center;
  background-color: rgb(157, 243, 86);
}
</style>
