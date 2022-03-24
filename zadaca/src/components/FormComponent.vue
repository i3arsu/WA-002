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
    async getAge(value) {
      let tempAge = await fetch("https://api.agify.io?name=" + value);
      this.ageData = await tempAge.json();
        
    },
    async getGender(value) {
      let tempGender = await fetch("https://api.genderize.io?name=" + value);
      this.genderData = await tempGender.json();
    },
    async getNationality(value) {
      let tempNationalityData = await fetch("https://api.nationalize.io?name=" + value);
      this.nationalityData = await tempNationalityData.json();
    },
    async getAll() {
      await this.getAge(this.name);
      await this.getGender(this.name);
      await this.getNationality(this.name);
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