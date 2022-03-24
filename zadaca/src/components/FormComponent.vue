<template>
  <v-form>
    <v-container>
        <v-text-field
            label="name"
            v-model="name"
            :counter="10">
        </v-text-field>
        <v-btn
            color="success"
            class="mr-4"
            @click="getAll()">
            Submit
        </v-btn>
        <div>
            <v-data-table
            :headers="headers"
            :items="newData"
            :items-per-page="10"
            class="elevation-1"
        ></v-data-table>
        </div>
    </v-container>
  </v-form>
</template>

<script>
  export default {
    name:"FormComponent",

    data: () => ({
    ageData: {},
    name: "",
    genderData: {},
    nationalityData: {},
    allData: [],
    newData: [],
    headers: [
      {
        text: "Name",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "Country", value: "country" },
      { text: "Probability", value: "probability of country" },
      { text: "Age", value: "age" },
      { text: "Gender", value: "gender" },
      { text: "Probability of Gender", value: "probability of gender" },
    ],
  }),
  methods: {
    getAge(value) {
      fetch("https://api.agify.io?name=" + value)
        .then((response) => response.json())
        .then((data) => (this.ageData = data));
    },
    getGender(value) {
      fetch("https://api.genderize.io?name=" + value)
        .then((response) => response.json())
        .then((data) => (this.genderData = data));
    },
    getNationality(value) {
      fetch("https://api.nationalize.io?name=" + value)
        .then((response) => response.json())
        .then((data) => (this.nationalityData = data));
    },
    getAll() {
      this.getAge(this.name);
      this.getGender(this.name);
      this.getNationality(this.name);
      let array1 = [];
      this.allData = [this.ageData, this.genderData, this.nationalityData];
      this.allData = this.allData[2].country.forEach((country) =>
        array1.push([this.allData[0], this.allData[1], country])
      );
      let array2 = [];
      array1.forEach((array) =>
        array2.push({
          name: array[0].name,
          age: array[0].age,
          gender: array[1].gender,
          "probability of gender": array[1].probability,
          country: array[2].country_id,
          "probability of country": array[2].probability,
        })
      );
      this.newData = array2;
      console.log(this.newData)
    },
  },
};
</script>