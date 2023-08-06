<template>
  <div class="container">
    <div class="sidebar">
      <div class="logo">
        <img alt="Vue logo" class="logo-img" src="../assets/logo.svg" />
      </div>
      <ul>
        <li
          v-for="items in employeeList"
          :key="items.name"
          :style="cssStyle(items.popularity)"
          :class="{ 'active': data.active === items.name, 'add': items.add }"
          @click="handleClick(items)"
        >
          {{ items.name }}
        </li>
      </ul>
    </div>
    <div class="content-bg">
      <div class="sidebar-space"></div>
      <div class="content-container">
        <div class="content-head">
          <img class="avatar" :src="data.imgEmployee" alt="" />
        </div>
        <div class="content">
          <div class="info-user">
            <div class="title">{{ data.nameEmployee }}</div>
            <div class="range-wrap">
              <div class="popularity">Popularity</div>
              <div class="range">
                <input class="slider" type="range" min="12" max="40" step="1" v-model="data.value">
              </div>
            </div>
          </div>
          <div class="biography">{{ data.biographyEmployee }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, computed, onMounted } from "vue"
import EmployeeData from "@/lib-utils/EmployeeData.json"

const employees = EmployeeData.employees as Array<EmployeeDataModel>

const data = reactive({
    value: employees[0].popularity,
    active: employees[0].name as string,
    imgEmployee: employees[0].image,
    nameEmployee: employees[0].name,
    biographyEmployee: employees[0].biography,
    colleagues: employees[0].colleagues,
    dataList: [] as Array<EmployeeDataModel>
})

const cssStyle = ((i: number) => ({
    "font-size": i + "px"
}))

const employeeList = computed((): Array<EmployeeDataModel> => {
    data.dataList = data.dataList.length > 0 ? data.dataList : employees
    const d = data.dataList.map((el) =>
        EmployeeDataModel.createInstance({
            name: el.name,
            popularity: data.active === el.name ? data.value : el.popularity,
            biography: el.biography,
            image: el.image,
            colleagues: el.colleagues,
            add: el.add
        })
    )
    data.dataList = d
    return d
})

const handleClick = (item: EmployeeDataModel) => {
  data.value = item.popularity
  data.active = item.name,
  data.imgEmployee = item.image,
  data.nameEmployee = item.name,
  data.biographyEmployee = item.biography
  data.dataList = data.dataList.map((el) =>
    EmployeeDataModel.createInstance({
        name: el.name,
        popularity: el.popularity,
        biography: el.biography,
        image: el.image,
        colleagues: el.colleagues,
        add: item.colleagues.indexOf(el.name) >= 0 ? true : false,
    })
  )
}

onMounted(() => {
    data.dataList = data.dataList.map((el) =>
      EmployeeDataModel.createInstance({
          name: el.name,
          popularity: el.popularity,
          biography: el.biography,
          image: el.image,
          colleagues: el.colleagues,
          add: data.colleagues.indexOf(el.name) >= 0 ? true : false,
      })
    )
})
</script>

<script lang="ts">
interface EmployeeDataParams {
    name: string
    popularity: number
    biography: string
    image: string
    colleagues: Array<string>
    add?: boolean
}

export class EmployeeDataModel {
    readonly name: string
    readonly popularity: number
    readonly biography: string
    readonly image: string
    readonly colleagues: Array<string>
    readonly add?: boolean

    private constructor(params: EmployeeDataParams) {
        this.name = params.name
        this.popularity = params.popularity
        this.biography = params.biography
        this.image = params.image
        this.colleagues = params.colleagues
        this.add = params.add
    }

    public static createInstance(params: Partial<EmployeeDataParams>) {
        return new EmployeeDataModel({
            name: params.name,
            popularity: params.popularity,
            biography: params.biography,
            image: params.image,
            colleagues: params.colleagues,
            add: params.add,
        })
    }
}
</script>

<style scoped>
.container {
  position: relative;
  width: 100%;
  height: 100vh;
  background-image: url('../assets/images/Header-image/Godfather-header.jpg');
  background-color: #cccccc;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.sidebar {
  width: 380px;
  height: 100%;
  position: absolute;
  z-index: 1;
  background: #25252d;
  opacity: 0.8;
  color: #fff;
}

.sidebar ul {
  padding: 50px 0;
}

.sidebar li {
  list-style: none;
  height: 56px;
  line-height: 56px;
  text-align: center;
  font-family: "Roboto-Light", Helvetica, Arial;
  cursor: pointer;
}

.sidebar li:hover {
  background: #404146;
}

.sidebar li.active {
  background: #404146;
  color: #3bd3fa;
}

.sidebar li.add {
  color: #3bd3fa;
}

.logo {
  height: 250px;
}

.logo-img {
  display: block;
  margin: 58px auto;
}

.background {
  width: 100%;
  height: 100vh;
  background-color: #cccccc;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.content-bg {
  display: flex;
  position: absolute;
  height: calc(100vh - 300px);
  width: 100%;
  bottom: 0;
  background-color: #2c2b31;
}

.sidebar-space {
  min-width: 380px;
}

.avatar {
  width: 150px;
  height: 150px;
  border: 1px #737278 solid;
  border-radius: 4px;
  margin-top: 5px;
}

.content-container {
  display: flex;
  width: 100%;
  color: #fff;
  position: relative;
  padding: 80px;
  padding: 0 80px 80px;
  top: -50px;
}

.content {
  padding-left: 50px;
}

.title {
  font-size: 36px;
  line-height: 36px;
  margin-bottom: 14px;
}

.range-wrap {
  display: flex;
  padding: 40px 0;
}

.popularity {
  font-size: 24px;
}

.range{
  width: 100%;
  line-height: 32px;
  margin-left: 30px;
}

.slider {
  -webkit-appearance: none;
  width: 100%;
  height: 5px;
  border-radius: 5px;
  background: #1a1d24;
  outline: none;
  -webkit-transition: .2s;
  transition: opacity .2s;
}

.slider:hover {
  opacity: 1;
}

.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: #FFF;
  cursor: pointer;
}

.slider::-moz-range-thumb {
  width: 25px;
  height: 25px;
  border-radius: 50%;
  background: #FFF;
  cursor: pointer;
}

.biography {
  background: #1b1d24;
  border-radius: 4px;
  padding: 15px 20px;
  color: #ccc;
}
</style>
