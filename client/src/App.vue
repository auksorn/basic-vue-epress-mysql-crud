<script setup>
import Axios from "axios";
import { ref } from "vue";

const employeeList = ref([]);

const name = ref("");
const age = ref(0);
const country = ref("");
const position = ref("");
const wage = ref(0);
const newWage = ref(0);

Axios.get("http://localhost:3001/employees").then((res) => {
  employeeList.value = res.data;
});
console.log(employeeList);

const createEmployee = () => {
  Axios.post("http://localhost:3001/create", {
    name: name.value,
    age: age.value,
    country: country.value,
    position: position.value,
    wage: wage.value,
  }).then(() => {
    employeeList.value = [
      ...employeeList.value,
      {
        name: name.value,
        age: age.value,
        country: country.value,
        position: position.value,
        wage: wage.value,
      },
    ];
    Axios.get("http://localhost:3001/employees").then((res) => {
      employeeList.value = res.data;
    });
    console.log(employeeList);
  });
};

const updateEmployeeWage = (id) => {
  Axios.put("http://localhost:3001/update", {
    wage: newWage.value,
    id: id,
  }).then(() => {
    employeeList.value = employeeList.value.map((val) => {
      return val.id == id
        ? {
            id: val.id,
            name: val.name,
            age: val.age,
            country: val.country,
            position: val.position,
            wage: newWage,
          }
        : val;
    });
  });
};

const deleteEmployee = (id) => {
  Axios.delete(`http://localhost:3001/delete/${id}`).then(() => [
    (employeeList.value = employeeList.value.filter((val) => {
      return val.id != id;
    })),
  ]);
};
</script>

<template>
  <html class="font-kanit bg-neutral-900 p-5">
    <h1 class="font-bold text-3xl text-white text-center">Vue CRUD</h1>
    <div class="flex justify-center mt-5 w-full">
      <div class="bg-white rounded-lg w-1/3 p-3">
        <h2 class="font-semibold text-2xl">Create</h2>
        <label for="name" class="font-semibold">Name :</label>
        <input
          type="text"
          v-model="name"
          class="px-2 py-1 border border-neutral-500 rounded-md mb-2 w-full"
        />
        <label for="age" class="font-semibold">Age : </label>
        <input
          type="number"
          v-model="age"
          class="px-2 py-1 border border-neutral-500 rounded-md mb-2 w-full"
        />
        <label for="country" class="font-semibold">Country : </label>
        <input
          type="text"
          v-model="country"
          class="px-2 py-1 border border-neutral-500 rounded-md mb-2 w-full"
        />
        <label for="position" class="font-semibold">Position : </label>
        <input
          type="text"
          v-model="position"
          class="px-2 py-1 border border-neutral-500 rounded-md mb-2 w-full"
        />
        <label for="wage" class="font-semibold">Wage : </label>
        <input
          type="number"
          v-model="wage"
          class="px-2 py-1 border border-neutral-500 rounded-md mb-2 w-full"
        />
        <button
          @click="createEmployee"
          class="p-3 bg-green-600 text-white font-semibold rounded-lg"
        >
          Add employee
        </button>
      </div>
      <hr />
    </div>
    <div class="flex justify-center mt-5 w-full">
      <div class="bg-white rounded-lg w-1/3 p-3 space-y-3">
        <h2 class="font-semibold text-2xl">Employees</h2>
        <div
          v-for="employee in employeeList"
          class="border border-neutral-500 rounded-md p-3"
        >
          <div>
            <span class="font-semibold">Name : </span>{{ employee.name }}
          </div>
          <div><span class="font-semibold">Age : </span>{{ employee.age }}</div>
          <div>
            <span class="font-semibold">Country : </span>{{ employee.country }}
          </div>
          <div>
            <span class="font-semibold">Position : </span
            >{{ employee.position }}
          </div>
          <div>
            <span class="font-semibold">Wage : </span>{{ employee.wage }}
          </div>
          <input
            v-model="newWage"
            type="number"
            class="px-2 py-1 border border-neutral-500 rounded-md mb-2 w-full"
          />
          <button
            @click="updateEmployeeWage(employee.id)"
            class="px-2 py-1 bg-yellow-600 text-white font-semibold mr-2 rounded-lg"
          >
            update
          </button>
          <button
            @click="deleteEmployee(employee.id)"
            class="px-2 py-1 bg-red-600 text-white font-semibold rounded-lg"
          >
            Delete
          </button>
        </div>
      </div>
    </div>
  </html>
</template>
