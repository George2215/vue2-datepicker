<template>
  <div id="app">
    <form @submit.prevent="submit" class="form-horizontal">
      <div class="form-group row">
        <label class="col-md-3 form-control-label" for="text-input">DateBirth</label>
        <div class="col-md-9">
          <date-picker
            v-model="form.dob"
            name="form.dob"
            format="YYYY-MM-DD"
            date-format="YYYY-MM-DD"
            type="date"
            confirm
          ></date-picker>
          <div class="error" v-if="form.errors.has('dob')">{{ form.errors.get("dob") }}</div>
        </div>
      </div>
      <br>
      <button type="submit" class="btn btn-outline-primary btn-sm">Validate</button>
      <button type="button" @click="reset()">Reset</button>
    </form>
  </div>
</template>

<script>
import Form from "vform";
import axios from "axios";
import MockAdapter from "axios-mock-adapter";
import DatePicker from "vue2-datepicker";
import "vue2-datepicker/index.css";

// Mock the response
const mockAdapter = new MockAdapter(axios);
mockAdapter.onPost("/foo").reply(config => {
  const data = JSON.parse(config.data);
  if (data.dob) {
    return [200, {}];
  }

  return [
    422,
    {
      dob: ["You must pick a value"]
    }
  ];
});

export default {
  components: {
    DatePicker
  },
  data() {
    return {
      form: new Form({
        dob: ""
      })
    };
  },
  methods: {
    submit() {
      this.form.post("/foo");
    },
    reset() {
      this.form.clear();
      this.form.reset();
    }
  }
};
</script>

<style>
.error {
  width: 100%;
  margin-top: 0.25rem;
  font-size: 80%;
  color: #dc3545;
}
</style>
