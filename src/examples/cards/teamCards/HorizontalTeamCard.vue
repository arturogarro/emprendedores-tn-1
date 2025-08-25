<script setup>
defineProps({
  image: {
    type: String,
    required: true,
  },
  profile: {
    type: Object,
    required: true,
    name: String,
  },
  position: {
    type: Object,
    label: String,
    color: String,
    required: true,
  },
  description: {
    type: String,
    required: true,
  },
  casa: {
    type: String,
    required: true
  },
  links: {
    type: Object,
    correo: String,
    whatsapp: String,
    linkFB: String,
    linkIG: String,
    required: true
  }
});
</script>
<template>
  <div class="card card-profile">
    <div class="row">
      <div class="col-lg-4 col-md-6 col-12 mt-n5">
          <div class="p-3 pe-md-0" v-if="image != 'assets/img/emprendedores/'">
            <img
              class="w-100 border-radius-md shadow-lg"
              :src="image"
              :alt="profile.name"
             
            />
          </div>
          <div class="p-3 pe-md-0 h-100" style="max-height: 150px;" v-if="image == 'assets/img/emprendedores/'">
            <div class="w-100 text-center bg-gradient-success border-radius-md h-100 shadow-lg p-3"><h4 class="text-white">{{ profile.name }}</h4></div>
          </div>
      </div>
      <div class="col-lg-8 col-md-6 col-12 my-auto">
        <div class="card-body ps-lg-0">
          <h3 class="mb-0">{{ profile.name }}</h3>
          <h5 :class="`text-${position.color}`">{{ position.label }}</h5>
          <p class="mb-0">
            {{ description }}
          </p>
          <p><strong>Casa:  {{ casa }}</strong> <a :href="`assets/localizacion/index.html?casa=${casa}`" target="_blank">Localizar casa</a></p>
          <p>
            <a :href="`https://api.whatsapp.com/send?phone=506${links.whatsapp}&text=Hola!+Me+interesa+contactarme+con+usted`" class="d-block" target="_blank" v-if="links.whatsapp != ''"><i class="fa fa-whatsapp mr-2"></i> {{ links.whatsapp }}</a>
            <a :href="`mailto:${links.correo}`" class="d-block" target="_blank" v-if="links.correo != ''"><i class="fa fa-envelope mr-2"></i> {{ links.correo }}</a>
            <a :href="links.linkIG" class="d-block" target="_blank" v-if="links.linkIG != ''"><i class="fa fa-instagram mr-2"></i> Instagram</a>
            <a :href="links.linkFB" class="d-block" target="_blank" v-if="links.linkFB != ''"><i class="fa fa-facebook-square mr-2"></i> Facebook</a>
          </p>
          
        </div>
      </div>
    </div>
  </div>
</template>
