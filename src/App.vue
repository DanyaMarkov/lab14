<template>
  <div id="app">
    <mainlist></mainlist>
  </div>
</template>

<script>

  import MainList from './components/mainlist.vue';

  export default {
  name: 'App',
  components: {
  mainlist:MainList,
  }
  }
</script>


<style>
  #app {

  background-color: white;
  width: 1000px;
  height: 800px;
  position: absolute;
  left: 15%;

  }

  .menu {
  text-align: center;
  background-color: #98FB98;
  width: 300px;
  height: 420px;
  position: absolute;
  top:0%;
  padding-top:5px;
  }

  .delete {

  text-align: center;
  background-color: #F08080;
  width: 300px;
  height: 140px;
  position: absolute;
  top:45%;
  padding-top:5px;

  }

  .change {

  text-align: center;
  background-color: #FFFF00;
  width: 300px;
  height: 425px;
  position: absolute;
  top:62%;
  padding-top:5px;

  }

  .book {

  background-color: #DAA520;
  width: 700px;
  height: 920px;
  position: absolute;
  left: 30%;
  
  }

  h2 {
  text-align: center;
  background: #77F7DE;
  color: black;
  letter-spacing: 0.1em;
  }

  .radio p {
  margin-left: 25%;
  text-align: center;
  display: inline-block;
  transform:scale(1.7);
  }

  button {
  font-weight: 700;
  color: white;
  text-decoration: none;
  padding: .8em 1em calc(.8em + 3px);
  border-radius: 3px;
  background: rgb(64,199,129);
  box-shadow: 0 -3px rgb(53,167,110) inset;
  transition: 0.2s;
  }

  button:hover {
  background: rgb(53, 167, 110);
  }

  button:active {
  background: rgb(33,147,90);
  box-shadow: 0 3px rgb(33,147,90) inset;
  }
</style>