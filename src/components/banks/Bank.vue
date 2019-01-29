<template >
<div v-if="banks">
    
<div>
    <table style="width:100%">
  <tr>
    <th>Bank Name</th>
    <th>IFSC</th> 
    <th>Address</th>

  </tr>
  <tr v-for="bank in banks">
    <td>{{bank.bank_name}}</td>
    <td>{{bank.ifsc}}</td>
    <td>{{bank.address}}</td>
  </tr>
 
</table>
</div>
</div>
</template>

<script>
export default {
    props:['banks']
}
</script>

<style scoped>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}


</style>
