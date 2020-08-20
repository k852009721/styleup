<template>
  <div id="calendar">
    <div>{{ msg }}</div>
    <table>
      <thead>
        <tr>
          <th style="">123</th>
          <th style="">123</th>
          <th style="">123</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>1</td>
          <td>2</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Calendar",
  props: {
    msg: String,
  },
  data: function() {
    return {

    }
  },
  mounted: function getOrderTime() {
    axios
      .get("https://l-beta.styleup.biz/fakeapi.php")
      .then(function (response) {
        console.log("succes");
        console.log(response.data);
      })
      .catch(function (error) {
        console.log("Errr");
        console.log(error);
      });
  },
  methods: {
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
  table {
    width: 100%;
    border-collapse: collapse;
    border-spacing: 0;
    border: 1px solid black;
    font-size: 1em;
    th,
    tr,
    td {
      border: 1px solid black;
    }
  }
</style>