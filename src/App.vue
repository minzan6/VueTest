<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <v-app class="bg-grey">
    <v-container>
      <v-row no-gutters>
        <v-col v-for="n in 1" :key="n" cols="12">
          <v-sheet class="ma-1 pa-1 rounded-lg">
            <h3 class="text-grey">Employee</h3>
            <v-table>
              <thead>
                <tr>
                  <th class="text-left">
                    Name
                  </th>
                  <th class="text-left">
                    Work Type
                  </th>
                  <th class="text-left">
                    Start time
                  </th>
                  <th class="text-left">
                    End time
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in employee" :key="item.name">
                  <td>{{ item.name }}</td>
                  <td>{{ item.work_type }}</td>
                  <td>{{ item.start_time }}</td>
                  <td>{{ item.end_time }}</td>
                </tr>
              </tbody>
            </v-table>
          </v-sheet>
          <v-sheet class="ma-1 pa-1 rounded-lg">
            <h3 class="text-grey">Task</h3>
            <v-table>
              <thead>
                <tr>
                  <th class="text-left">
                    Title
                  </th>
                  <th class="text-left">
                    Duration
                  </th>
                  <th class="text-left">
                    Assign
                  </th>
                </tr>
              </thead>
              <tbody>
                
                
              </tbody>
            </v-table>
          </v-sheet>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        
      }
    },
  }

  const factories = [
    { name: 'BR1', employees: ['John', 'Alice', 'Bob', 'Jessie', 'Karen'] },
    { name: 'BR2', employees: ['Jessie', 'Karen', 'John'] },
    { name: 'BR3', employees: ['Miles', 'Eric', 'Henry', 'Bob'] },
    { name: 'BR4', employees: [] },
  ]

  const employeeType = [
    { id: 1, name: 'FullTime', work_begin: '09:00:00', work_end: '17:00:00' },
    { id: 2, name: 'MidTime', work_begin: '12:00:00', work_end: '21:00:00' },
    { id: 3, name: 'HalfTime', work_begin: '20:00:00', work_end: '00:00:00' },
  ]

  const employees = [
    { id: 1, name: 'Alice', type: 2 },
    { id: 2, name: 'Bob', type: 3 },
    { id: 3, name: 'John', type: 2 },
    { id: 4, name: 'Karen', type: 1 },
    { id: 5, name: 'Miles', type: 3 },
    { id: 6, name: 'Henry', type: 1 },
  ]

  const tasks = [
    { id: 1, title: 'task01', duration: 60 }, // min
    { id: 2, title: 'task02', duration: 120 },
    { id: 3, title: 'task03', duration: 180 },
    { id: 4, title: 'task04', duration: 360 },
    { id: 5, title: 'task05', duration: 30 },
    { id: 6, title: 'task06', duration: 220 },
    { id: 7, title: 'task07', duration: 640 },
    { id: 8, title: 'task08', duration: 250 },
    { id: 9, title: 'task09', duration: 119 },
    { id: 10, title: 'task10', duration: 560 },
    { id: 11, title: 'task11', duration: 340 },
    { id: 12, title: 'task12', duration: 45 },
    { id: 13, title: 'task13', duration: 86 },
    { id: 14, title: 'task14', duration: 480 },
    { id: 15, title: 'task15', duration: 900 },
  ]

  var namelest = [];
  function sortName(){
    for (var i = 0;i < factories.length;i++){
      for(var j = 0; j < factories[i].employees.length;j++){
        namelest.push(factories[i].employees[j]);
      }
    }
    namelest.sort();
    for(var k = 0; k < namelest.length;k++){
      if (namelest[k] == namelest[k+1]) namelest.splice(k, 1);
    }
  }

  sortName();

  //找到時段編號
  function foundtype(name){
    for(var j = 0; j < employees.length;j++){
      if(name == employees[j].name){
        return employees[j].type;
      }
    }
  }

  function foundTypeName(name){
    var id = foundtype(name);
    for(var i = 0;i < employeeType.length; i++){
      if(id == employeeType[i].id){
        return employeeType[i].name;
      }
    }
  }

  function foundStartTime(name){
    var id = foundtype(name);
    for(var i = 0;i < employeeType.length; i++){
      if(id == employeeType[i].id){
        return employeeType[i].work_begin;
      }
    }
  }

  function foundEndTime(name){
    var id = foundtype(name);
    for(var i = 0;i < employeeType.length; i++){
      if(id == employeeType[i].id){
        return employeeType[i].work_end;
      }
    }
  }

  const employee = [];
  for(var i = 0;i < namelest.length;i++){
    employee.push({name:namelest[i],work_type:foundTypeName(namelest[i]),start_time:foundStartTime(namelest[i]),end_time:foundEndTime(namelest[i])});
  }
  

  // 給定時間返回現場工作人數
  function workPerson(dayTime){
    dayTime = dayTime.split(":");
    dayTime = dayTime.map(Number);
    var count= 0;
    for(var i = 0; i < employee.length;i++){
      if(employee[i].start_time != undefined){
        var time_start = employee[i].start_time.split(":");
        time_start = time_start.map(Number);
        var time_end = employee[i].end_time.split(":");
        time_end = time_end.map(Number);
        if(dayTime[0] >= time_start[0] && dayTime[0] < time_end[0]){
          count++;
        }else if(dayTime[0] == time_end[0] && dayTime[1] < time_end[1]){
          count++;
        }else if(dayTime[0] == time_end[0] && dayTime[1] == time_end[1] && dayTime[2] <= time_end[2]){
          count++;
        }
      }
    }
    console.log("工作人數"+count);
  }

  workPerson("13:05:41");

</script>

<style scoped>
</style>
