<template>
  <el-tabs
    v-model="activeName"
    class="demo-tabs"
    type="card"
    @tab-click="handleClick"
  >
    <el-tab-pane label="Team Standings" name="teamStandings">
      <div
        ref="teamTable"
        class="table"
        @mousedown="down"
        @mouseup="up($event, 'teamTable')"
      >
        <div class="col" v-for="col of teamData" :key="col">
          <div class="header">{{ col.year }}</div>
          <div
            v-for="item of col.list"
            :key="item"
            class="content"
            :class="getClassName(item.team)"
          >
            {{ item.team }}
            <span class="position">No.{{ item.position }}</span>
            <span class="point">{{ item.points }}</span>
          </div>
        </div>
      </div>
    </el-tab-pane>
    <el-tab-pane label="Driver Standings" name="driverStandings">
      <div
        ref="driverTable"
        class="table"
        @mousedown="down"
        @mouseup="up($event, 'driverTable')"
      >
        <div class="col" v-for="col of driverData" :key="col">
          <div class="header">{{ col.year }}</div>
          <div
            v-for="item of col.list"
            :key="item"
            class="content"
            :class="getClassName(item.car)"
          >
            {{ item.driver }}
            <span class="position">No.{{ item.position }}</span>
            <div class="point">{{ item.points }}</div>
          </div>
        </div>
      </div>
    </el-tab-pane>
  </el-tabs>
</template>

<script setup lang="ts">
import { onMounted, ref, watch } from "vue";
import axios from "axios";
import type { TabsPaneContext } from "element-plus";

const teamTable = ref();
const driverTable = ref();
const activeName = ref("teamStandings");
const teamData: any = ref([]);
const driverData: any = ref([]);
const startX = ref();
const startY = ref();
const endX = ref();
const endY = ref();

const handleClick = (tab: TabsPaneContext, event: Event) => {
  console.log(tab, event);
};

const getData = () => {
  axios.get("/api/team-standings").then((res) => {
    teamData.value = res.data.data;
  });

  axios.get("/api/driver-standings").then((res) => {
    driverData.value = res.data.data;
  });
};

const getClassName = (name: string) => {
  return name.replace(/\s/g, "-");
};

const down = (e: any) => {
  startX.value = e.clientX;
  startY.value = e.clientY;
};

const up = (e: any, type: string) => {
  endX.value = e.clientX;
  endY.value = e.clientY;

  switch (type) {
    case "teamTable":
      teamTable.value.scrollLeft += startX.value - endX.value;
      teamTable.value.scrollTop += startY.value - endY.value;
      break;
    case "driverTable":
      driverTable.value.scrollLeft += startX.value - endX.value;
      driverTable.value.scrollTop += startY.value - endY.value;
      break;
  }
};

onMounted(() => {
  getData();
});
</script>

<style>
html,
body {
  margin: 0;
}

::-webkit-scrollbar {
  display: none;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000;
}

.table {
  display: flex;
  height: calc(100vh - 56px);
  overflow: auto;
  cursor: grab;
  user-select: none;
}

.col {
  display: flex;
  flex-direction: column;
}

.col:first-of-type {
  margin-left: 15px;
}

.col:last-of-type {
  margin-right: 15px;
}

.col + .col {
  margin-left: 15px;
}

.header {
  width: 240px;
  line-height: 50px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.12), 0 0 6px rgba(0, 0, 0, 0.04);
  background-color: #e5eaf3;
}

.content {
  position: relative;
  display: flex;
  flex-direction: column;
  width: 240px;
  height: 100px;
  line-height: 100px;
  font-size: 14px;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.12), 0 0 6px rgba(0, 0, 0, 0.04);
  margin-top: 15px;
}

.content:last-of-type {
  margin-bottom: 15px;
}

.position {
  position: absolute;
  top: 5px;
  left: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 25px;
  height: 25px;
}

.point {
  position: absolute;
  bottom: 5px;
  right: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 25px;
  height: 25px;
  /* padding: 0 5px; */
  /* border-bottom: 1px solid; */
  /* border-radius: 4px; */
}

.Ferrari {
  background-color: #f91536;
}

.Mercedes {
  background-color: #6cd3bf;
}

.STR-Cosworth,
.STR-Ferrari,
.STR-Renault,
.Scuderia-Toro-Rosso-Honda,
.Toro-Rosso-Ferrari,
.Toro-Rosso,
.RBR-Cosworth,
.RBR-Ferrari,
.RBR-Renault,
.Red-Bull-Renault,
.Red-Bull-Racing-Renault,
.Red-Bull-Racing-TAG-Heuer,
.Red-Bull-Racing-Honda,
.Red-Bull-Racing-RBPT {
  background-color: #3671c6;
  color: #fff;
}

.McLaren-Honda,
.McLaren-Renault,
.McLaren-Mercedes {
  background-color: #f58020;
}

.Alpine-Renault {
  background-color: #2293d1;
}

.Lotus-Cosworth,
.Lotus-Renault,
.Lotus-Mercedes {
  background-color: #07080b;
  color: #ebaf2d;
}

.Alfa-Romeo-Racing-Ferrari,
.Alfa-Romeo-Ferrari {
  background-color: #c92d4b;
}

.Haas-Ferrari {
  background-color: #b6babd;
}

.AlphaTauri-RBPT,
.AlphaTauri-Honda {
  background-color: #5e8faa;
  color: #fff;
}

.Williams-BMW,
.Williams-Toyota,
.Williams-Cosworth,
.Williams-Renault,
.Williams-Mercedes {
  background-color: #37bedd;
}

.Aston-Martin-Mercedes,
.Aston-Martin-Aramco-Mercedes {
  background-color: #358c75;
  color: #fff;
}

.Caterham-Renault {
  background-color: #023500;
  color: #fff;
}

.Sauber-Petronas,
.Sauber-BMW,
.Sauber-Ferrari {
  background-color: #002777;
  color: #fff;
}

.Renault {
  background-color: #fed73a;
}

.Marussia-Cosworth,
.Marussia-Ferrari {
  background-color: #f76137;
}

.HRT-Cosworth {
  background-color: #e5dfe0;
}

.Virgin-Cosworth {
  background-color: #ee3e1a;
  color: #0b0b0b;
}

.MRT-Mercedes {
  background-color: #0278cb;
  color: #f76a38;
}

.Brawn-Mercedes {
  background-color: #dadcdb;
  /* color: #d6f652; */
}

.Jaguar-Cosworth {
  background-color: #28725a;
  color: #fff;
}

.MF1-Toyota {
  background-color: #ef2314;
  color: #fff;
}

.Super-Aguri-Honda {
  color: #c1171a;
}

.Spyker-Ferrari {
  background-color: #f15000;
  color: #fff;
}

.BAR-Honda {
  color: #c10001;
}

.Honda {
  color: #e62110;
}

.Toyota {
  color: #db333b;
}

.Prost-Acer {
  background-color: #0a2ba2;
  color: #fff;
}

.Benetton-Playlife,
.Benetton-Renault {
  background-color: #247fc3;
}

.Arrows-Supertec,
.Arrows-Asiatech,
.Arrows-Cosworth {
  background-color: #ef7126;
}

.Jordan-Mugen-Honda,
.Jordan-Honda,
.Jordan-Ford,
.Jordan-Toyota {
  background-color: #f7f72a;
}

.Minardi-Asiatech,
.Minardi-Cosworth {
  background-color: #060606;
  color: #fff;
}

.Force-India-Ferrari,
.Force-India-Mercedes,
.Force-India-Sahara,
.Racing-Point-BWT-Mercedes {
  background-color: #efacce;
}
</style>
