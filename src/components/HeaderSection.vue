<template>
    <header>
      <div class="logo">
        <img src="../assets/parques_reunidos.jpg" alt="logo" @click="reload">
      </div>
    </header>
</template>

<script>
export default {
  name: 'HeaderSection',
  methods: {
    reload() {
      location.reload();
    }
  }
}
</script>

<style>
/* Estilo base */
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #FFFFFF;
  color: #fff;
  padding: 10px;
  border-right: 1px solid #CCCCCC;
}

.logo img {
  width: 120px;
  /* height: 60px; */
  margin-left: 100px;
  border-radius: 25px;
  cursor: pointer;
}

/* Estilo para dispositivos móviles */
@media (max-width: 768px) {
  header {
    flex-direction: column;
    padding: 20px;
  }
  .logo img {
    margin-left: 0px;
  }
}
</style>