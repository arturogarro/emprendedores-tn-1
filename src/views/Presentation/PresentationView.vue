<script>
import NavbarDefault from "../../examples/navbars/NavbarDefault.vue";
import DefaultFooter from "../../examples/footers/FooterDefault.vue";
import Header from "../../examples/Header.vue";
import HorizontalTeamCard from "@/examples/cards/teamCards/HorizontalTeamCard.vue";
import vueMkHeader from "@/assets/img/vue-mk-header.jpg";
import MaterialButton from "@/components/MaterialButton.vue";

export default {
  components: {
    NavbarDefault,
    DefaultFooter,
    Header,
    HorizontalTeamCard,
    vueMkHeader,
    MaterialButton,
  },
  data() {
    return {
      jsonData: [],
      filtro: "",
      esCategoria: false,
      mostrarBotonArriba: false,
      path: "assets/img/emprendedores/",
      categorias: ["comida", "productos", "servicios", "salud"],
    };
  },
  mounted() {
    fetch("assets/json/emprendedores.json")
      .then((response) => response.json())
      .then((data) => {
        this.jsonData = data;
      })
      .catch((error) => {
        console.error("Error al cargar el archivo JSON:", error);
      });
    window.addEventListener("scroll", this.handleScroll);
    const params = new URLSearchParams(window.location.search);
    const searchQuery = params.get("buscar");
    if (searchQuery) {
      this.filtro = decodeURIComponent(searchQuery);
    }
  },
  computed: {
    filteredData() {
      var datosFiltrados = {};
      if (this.esCategoria) {
        datosFiltrados = this.jsonData.filter((item) => {
          const searchTerm = this.filtro
            .toLowerCase()
            .normalize("NFD")
            .replace(/[\u0300-\u036f]/g, "");
          return item.categoria
            .toLowerCase()
            .normalize("NFD")
            .replace(/[\u0300-\u036f]/g, "")
            .includes(searchTerm);
        });
      } else {
        datosFiltrados = this.jsonData.filter((item) => {
          const searchTerm = this.filtro
            .toLowerCase()
            .normalize("NFD")
            .replace(/[\u0300-\u036f]/g, "");
          return (
            item.nombre
              .toLowerCase()
              .normalize("NFD")
              .replace(/[\u0300-\u036f]/g, "")
              .includes(searchTerm) ||
            item.description
              .toLowerCase()
              .normalize("NFD")
              .replace(/[\u0300-\u036f]/g, "")
              .includes(searchTerm)
          );
        });
      }
      this.esCategoria = false;
      return datosFiltrados;
    },
  },
  methods: {
    updateURL() {
      const queryParam = encodeURIComponent(this.filtro);
      const newUrl = `${window.location.pathname}?buscar=${queryParam}`;
      window.history.replaceState(null, "", newUrl);
    },
    scrollArriba() {
      window.scrollTo({ top: 0, behavior: "smooth" });
    },
    handleScroll() {
      this.mostrarBotonArriba = window.scrollY > 300;
    },
    filtrarCategorias(index) {
      this.esCategoria = true;
      this.filtro = index.target.id;
      this.updateURL();
    },
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.handleScroll);
  },
};
</script>
<template>
  <div class="container position-sticky z-index-sticky top-0">
    <div class="row">
      <div class="col-12">
        <NavbarDefault :sticky="true" />
      </div>
    </div>
  </div>
  <Header>
    <div
      class="page-header min-vh-50"
      style="background-image: url('assets/img/vue-mk-header.jpg')"
      loading="lazy"
    >
      <div class="container">
        <div class="row">
          <div class="col-lg-7 text-center mx-auto position-relative">
            <h1
              class="text-white pt-3 mt-n5 me-2"
              :style="{ display: 'inline-block ' }"
            >
              EMPRENDEDORES TN
            </h1>
            <p class="lead text-white px-5 mt-3" :style="{ fontWeight: '500' }">
              Del condominio, para el condominio
            </p>
          </div>
        </div>
      </div>
    </div>

    <div class="row p-2">
      <div class="col-12 col-lg-7 mx-auto">
        <div
          class="card card-body d-block mb-6 mx-3 mx-md-4 mt-n7 mx-3 mx-lg-7"
        >
          <div class="input-group">
            <input
              type="text"
              v-model="filtro"
              @input="updateURL"
              id="filtro"
              placeholder="Buscar por nombre o descripción"
              class="form-control"
            />
            <span class="input-group-text"
              ><i class="fa fa-search mr-2" aria-hidden="true"></i
            ></span>
          </div>
        </div>
      </div>
    </div>
  </Header>

  <div
    class="card card-body blur shadow-blur mx-3 mx-md-4 mt-n6 bg-gradient-dark pb-6"
  >
    <div class="container p-0">
      <div class="row">
        <div class="col-lg-6 mx-auto">
          <MaterialButton
            v-for="(categoria, index) in categorias"
            :key="index"
            :id="categoria"
            variant="gradient"
            color="secondary"
            class="w-auto me-2"
            @click="filtrarCategorias"
            >{{ categoria }}</MaterialButton
          >
        </div>
      </div>
      <div class="row">
        <div
          class="col-lg-6 mt-5"
          v-for="(item, index) in filteredData"
          :key="index"
        >
          <HorizontalTeamCard
            class="mt-4"
            :image="path + item.img"
            :profile="{ name: item.nombre }"
            :position="{ label: item.representante, color: 'success' }"
            :description="item.description"
            :casa="item.casa"
            :links="{
              whatsapp: item.whatsapp,
              correo: item.correo,
              linkFB: item.linkFB,
              linkIG: item.linkIG,
            }"
          />
        </div>
      </div>
    </div>
  </div>
  <button
    v-show="mostrarBotonArriba"
    class="btn-ir-arriba"
    @click="scrollArriba"
  >
    ↑
  </button>
  <DefaultFooter />
</template>

<style>
.btn-ir-arriba {
  position: fixed;
  bottom: 20px;
  right: 3rem;
  background-color: #3498db;
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  border-radius: 5px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
  z-index: 1000;
}

.btn-ir-arriba:hover {
  background-color: #2980b9;
}

/* Mostrar el botón cuando se activa v-show */
[v-show] {
  display: block !important;
}
</style>
