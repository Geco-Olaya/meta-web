<template>
  <div class="principal" v-loading="loading">
    <el-breadcrumb separator="/" class="breadcrumb">
    <el-breadcrumb-item :to="{ path: '/' }">Inicio</el-breadcrumb-item>
    <el-breadcrumb-item>{{datos.data[0].titulo}}</el-breadcrumb-item>
    </el-breadcrumb>
  <BannerMicro :imagen="`https://intranet.meta.gov.co/micrositio_banners/${datos.data[0].banners[0].banner}`"
  v-if="datos.data[0].banners[0]">
  </BannerMicro>

  <!-- <el-carousel :interval="9000" arrow="always" indicator-position="outside" >
    <el-carousel-item v-for="item in datos.data[0].banners[0].banner[0]" :key="item">
      <img :src="`https://intranet.meta.gov.co/imagen_banners/${item.imagen}`">
    </el-carousel-item>
  </el-carousel> -->

  <div class="contenido">
    <div class="texto">
      <div class="info_micrositio">
        <h2>{{datos.data[0].titulo}}</h2>
        <br><br>
        <p v-html="datos.data[0].texto"></p>
        <br><br>
      </div>

      <el-collapse v-model="activeNames" @change="handleChange" accordion>
        <el-collapse-item  :title="setTitle(item.titulo)" :name="item.id" v-bind:id="item.id" v-for="(item,index) in datos.data[0].secciones" :key="index" v-bind:class="index">

            <div class="sub_texto" v-for="(texto_seccion,index) in item.textos" :key="index">
              <p v-html="texto_seccion.texto"> </p>
            </div><br>

            <el-input placeholder="Buscador" v-model="inputsearch" @keyup.native="buscarnorma" class="input-search">
              <i slot="prefix" class="el-input__icon el-icon-search"></i>        
            </el-input>  
             

            <div class="secciones_docs" >
              <div class="secciones_docs_descarga secciones_docs_fecha">
                <p>Fecha</p>
              </div>
              <div class="secciones_docs_descarga">
                <p> Descripción Corta</p>
              </div>
              <div class="secciones_docs_descarga">
                <p> Titulo</p>
              </div>
            </div>

            <div  class="secciones_docs tr" v-for="(archivo,index) in item.archivos" :key="index">
              <i class="el-icon-document"></i>
              <div class="secciones_docs_descarga ">
                <p class="secciones_docs_fechap">{{archivo.updated_at}}</p>
                <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam mollis felis ac dolor blandit facilisis.</p>    
              </div>
              <div class="secciones_docs_descarga">
                <p> {{archivo.titulo}}</p>
                <a :href="`https://intranet.meta.gov.co/secciones_archivos/${archivo.archivo}`" target="_blank">
                <el-button type="primary">Descarga</el-button>
                </a>
              </div>
            </div>

            <iframe v-for="(video,index) in item.videos" :key="index"
            width="640" height="360"
            :src="`https://www.youtube.com/embed/${videoYoutube(video.video)}`"
            frameborder="0" gesture="media"
            allowfullscreen>
            </iframe>

            <div class="galeria" v-if="item.imagenes[0]">
              <br><br>
              <template>
                <el-carousel :interval="12000" style="width:100%; max-width:700px,">
                  <el-carousel-item v-for="(foto,index) in item.imagenes" :key="index">
                    <img :src="`https://intranet.meta.gov.co/secciones_imagenes/${foto.imagen}`"
                          width="auto" style="max-height:100%">
                  </el-carousel-item>
                </el-carousel>
              </template>
            </div><br>

            <div class="social_btn" v-if="item.urls[0]">
              <div class="social_botones_url_externa" v-for="(item,index) in item.urls" :key="index" >
                <a :href="item.url" target="_blank">
                  <img :src="`https://intranet.meta.gov.co/secciones_images_url_externas/${item.imagen}`" alt="">
                </a>
              </div>
            </div>

            <div class="iframe_cuadro"
            v-for="(iframe,index) in item.iframes" v-html="iframe.iframe" :key="index">
            </div>

              <div class="sub_secciones" v-if="item.sub_secciones[0]">
                  <el-tabs v-model="activeName" @tab-click="handleClick" type="border-card" tab-position="top" style="height: auto;" >
                      <el-tab-pane :label="sub.titulo" v-for="(sub,index) in item.sub_secciones" :key="index" :name="sub.id" class="reverse" >



                        <div class="sub_texto" v-for="(texto,index) in sub.textos" :key="index">
                          <p v-html="texto.texto"> </p>
                        </div>

                        <iframe v-for="(video,index) in sub.videos" :key="index"
                        width="640" height="360"
                        :src="`https://www.youtube.com/embed/${videoYoutube(video.video)}`"
                        frameborder="0" gesture="media"
                        allowfullscreen>
                        </iframe>

                        <div class="secciones_docs" v-for="(archivo, index) in sub.archivos" :key="index">
                          <i class="el-icon-document"></i>
                          <div class="secciones_docs_descarga">
                            <p>{{archivo.titulo}}</p>
                            <a :href="`https://intranet.meta.gov.co/secciones_archivos/${archivo.archivo}`" target="_blank">
                            <el-button type="primary">Descarga</el-button>
                            </a>
                          </div>
                        </div>

                        <div class="galeria" v-if="sub.imagenes[0]">
                          <br><br><template>
                            <el-carousel :interval="8000" style="width:100%; max-width:700px,">
                              <el-carousel-item v-for="(foto, index) in sub.imagenes" :key="index">
                                <img :src="`https://intranet.meta.gov.co/secciones_imagenes/${foto.imagen}`"
                                      width="auto" style="max-height:100%">
                              </el-carousel-item>
                            </el-carousel>
                          </template>
                        </div><br>

                        <div class="iframe_cuadro"
                        v-for="(iframe,index) in sub.iframes" v-html="iframe.iframe" :key="index">
                        </div>

                        <div class="social_btn" v-if="sub.urls[0]">
                            <div class="social_botones_url_externa" v-for="(item,index) in sub.urls" :key="index">
                              <a :href="item.url" target="_blank">
                                <img :src="`https://intranet.meta.gov.co/secciones_images_url_externas/${item.imagen}`" alt="">
                              </a>
                            </div>
                        </div>

                        <div class="sub_sub_secciones" v-if="sub.sub_secciones[0]">
                            <el-tabs v-model="activeNameSub" @tab-click="handleClickSub" type="border-card" tab-position="top" style="height: auto;" >
                                <el-tab-pane :label="subsub.titulo" v-for="(subsub,index) in sub.sub_secciones" :name="subsub.id" class="reverse" :key="index">



                                  <div class="sub_texto"  v-for="(texto, index) in subsub.textos" :key="index">
                                    <p v-html="texto.texto"> </p>
                                  </div>

                                  <iframe v-for="(video,index) in subsub.videos" :key="index"
                                  width="640" height="360"
                                  :src="`https://www.youtube.com/embed/${videoYoutube(video.video)}`"
                                  frameborder="0" gesture="media"
                                  allowfullscreen>
                                  </iframe>

                                  <div class="secciones_docs" v-for="(archivo, index) in subsub.archivos" :key="index">
                                    <i class="el-icon-document"></i>
                                    <div class="secciones_docs_descarga">
                                      <p>{{archivo.titulo}}</p>
                                      <a :href="`https://intranet.meta.gov.co/secciones_archivos/${archivo.archivo}`" target="_blank">
                                      <el-button type="primary">Descarga</el-button>
                                      </a>
                                    </div>
                                  </div>

                                  <div class="galeria" v-if="subsub.imagenes[0]">
                                    <br><br><template>
                                      <el-carousel :interval="8000" style="width:100%; max-width:700px,">
                                        <el-carousel-item v-for="(foto,index) in subsub.imagenes" :key="index">
                                          <img :src="`https://intranet.meta.gov.co/secciones_imagenes/${foto.imagen}`"
                                                width="auto" style="max-height:100%">
                                        </el-carousel-item>
                                      </el-carousel>
                                    </template>
                                  </div><br>

                                  <div class="social_botones_cuadro"
                                  v-for="(iframe,index) in subsub.iframes" v-html="iframe.iframe" :key="index">
                                  </div>

                                  <div class="social_botones_url_externa" v-for="(item, index) in subsub.urls" :key="index">
                                    <a :href="item.url" target="_blank">
                                      <img :src="`https://intranet.meta.gov.co/secciones_images_url_externas/${item.imagen}`" alt="">
                                    </a>
                                  </div>


                                        <div class="sub_sub_secciones">
                                            <el-tabs v-model="activeNameSubH" @tab-click="handleClickSubH" tab-position="top" style="height: auto;" >
                                                <el-tab-pane :label="subsub.titulo" v-for="(subsub,index) in subsub.sub_secciones" class="reverse" :key="index">



                                                  <div class="sub_texto" v-for="(texto,index) in subsub.textos" :key="index">
                                                    <p v-html="texto.texto"> </p>
                                                  </div>

                                                  <iframe v-for="(video,index) in subsub.videos" :key="index"
                                                  width="640" height="360"
                                                  :src="`https://www.youtube.com/embed/${videoYoutube(video.video)}`"
                                                  frameborder="0" gesture="media"
                                                  allowfullscreen>
                                                  </iframe>

                                                  <div class="secciones_docs" v-for="(archivo,index) in subsub.archivos" :key="index">
                                                    <i class="el-icon-document"></i>
                                                    <div class="secciones_docs_descarga">
                                                      <p>{{archivo.titulo}}</p>
                                                      <a :href="`https://intranet.meta.gov.co/secciones_archivos/${archivo.archivo}`" target="_blank">
                                                      <el-button type="primary">Descarga</el-button>
                                                      </a>
                                                    </div>
                                                  </div>

                                                  <div class="galeria" v-if="subsub.imagenes[0]">
                                                    <br><br><template>
                                                      <el-carousel :interval="8000" style="width:100%; max-width:700px,">
                                                        <el-carousel-item v-for="(foto,index) in subsub.imagenes" :key="index">
                                                          <img :src="`https://intranet.meta.gov.co/secciones_imagenes/${foto.imagen}`"
                                                                width="auto" style="max-height:100%">
                                                        </el-carousel-item>
                                                      </el-carousel>
                                                    </template>
                                                  </div><br>

                                                  <div class="social_botones_cuadro"
                                                  v-for="(iframe,index) in subsub.iframes" v-html="iframe.iframe" :key="index">
                                                  </div>

                                                  <div class="social_botones_url_externa" v-for="(item,index) in subsub.urls" :key="index">
                                                    <a :href="item.url" target="_blank">
                                                      <img :src="`https://intranet.meta.gov.co/secciones_images_url_externas/${item.imagen}`" alt="">
                                                    </a>
                                                  </div>

                                                </el-tab-pane>
                                            </el-tabs>
                                        </div>


                                </el-tab-pane>
                            </el-tabs>
                        </div>

                      </el-tab-pane>
                  </el-tabs>
              </div>

        </el-collapse-item>
      </el-collapse>

      <br><br><h2 v-if="datos.data[0].galeria[0]">Galeria</h2><br>
      <div class="galeria" v-if="datos.data[0].galeria[0]">
        <template>
          <el-carousel :interval="7000" type="card" height="200px" style="width:100%">
            <el-carousel-item v-for="(foto,index) in datos.data[0].galeria" :key="index">
              <img :src="`https://intranet.meta.gov.co/micrositio_galeria/${foto.imagen}`" alt="titulo" width="100%">
            </el-carousel-item>
          </el-carousel>
        </template>
      </div><br>

      <div class="contacto">
        <!-- <br><h2>Contacto</h2><br> -->
        <div class="contacto_telefono" v-if="datos.data[0].telefono">
          <p><b>Telefono:</b> {{datos.data[0].telefono}}</p>
        </div>

        <div class="contacto_telefono" v-if="datos.data[0].direccion">
          <p><b>Dirección:</b> {{datos.data[0].direccion}}</p>
        </div>

        <div class="contacto_telefono" v-if="datos.data[0].email">
          <p><b>Correo electronico:</b> {{datos.data[0].email}}</p>
        </div>

        <div class="contacto_redes" v-if="datos.data[0].facebook">
          <a :href="datos.data[0].facebook" target="_blank">
            <img src="../assets/facebook.svg">
          </a>
          <a :href="datos.data[0].twitter" target="_blank">
           <img src="../assets/twitter.svg" >
          </a>
        </div>
      </div>

    </div>

    <div class="lateral" v-if="datos.data[0].menu_lateral == 1">
      <div class="social_botones_url_externa" v-for="(boton,index) in datos.data[0].url_externa" :key="index">
        <a :href="boton.url" target="_blank">
          <img :src="`https://intranet.meta.gov.co/micrositio_img_urls/${boton.imagen}`" alt="">
        </a>
      </div>

      <div class="social_botones_cuadro" v-for="(iframe,index) in datos.data[0].iframes" :key="index" v-html="iframe.iframe">
      </div>
    </div>

  </div>
</div>
</template>

<script>

import BannerMicro from './bannerMicro.vue'
import Breadcrumb from './breadcrumb.vue'
import axios from 'axios';

export default {
  components: {BannerMicro, Breadcrumb},
  name: 'app',
  created(){
    var url = window.location+"";
    var open = url.split("open=");

    if (typeof open[1] === 'undefined') {
      axios.get(`https://intranet.meta.gov.co/api/micrositio/informacion/${this.$route.params.id}`)
      .then( response => {
          this.datos = response.data;
          if(this.$route.params.id==96){

  //Normatividad Inicio
          var x;
          var y;
          var i;
          var arrayfinal= new Array();
          var valorllega1=this.datos.data[0].secciones;
          var valorllega=this.datos.data[0].secciones[0].archivos;
          
          for(y=0;y<valorllega1.length;y++){
              
              var valorllega=this.datos.data[0].secciones[y].archivos;
            for (x=0;x<valorllega.length;x++){
                var res = valorllega[x].updated_at.slice(0,10);
                
                arrayfinal[x]=res;
                this.datos.data[0].secciones[y].archivos[x].updated_at= arrayfinal[x]; 
            }  
          }
          //Normatividad Fin
          }
          if(this.datos.data[0].secciones[0])
          {
            this.activeNames = this.datos.data[0].secciones[0].id
          }
          if(this.datos.data[0].url_redireccion == null)
          {
            setTimeout( (()=>this.loading = false), 2000)          
          }
          else {  window.location = this.datos.data[0].url_redireccion  }
      }) 
    }else{
      var div2 = open[1].split("_");
      if (typeof div2[1] != 'undefined') {
        this.activeNames = parseInt(div2[0]);
      }else{
        this.activeNames = parseInt(open[1]);
      }
      axios.get(`https://intranet.meta.gov.co/api/micrositio/informacion/${this.$route.params.id}`)
      .then( response => {
          this.datos = response.data;
          if(this.$route.params.id==96){
           //Normatividad Inicio
          var x;
          var y;
          var i;
          var arrayfinal= new Array();
          var valorllega1=this.datos.data[0].secciones;
          var valorllega=this.datos.data[0].secciones[0].archivos;
          
          for(y=0;y<valorllega1.length;y++){
              
              var valorllega=this.datos.data[0].secciones[y].archivos;
            for (x=0;x<valorllega.length;x++){
                var res = valorllega[x].updated_at.slice(0,10);
                
                arrayfinal[x]=res;
                this.datos.data[0].secciones[y].archivos[x].updated_at= arrayfinal[x]; 
            }  
          }
          //Normatividad Fin
          }

          if (typeof div2[1] === 'undefined') {
            for (var i in this.datos.data[0].secciones) {
              if (this.datos.data[0].secciones[i].id == this.activeNames) {
                this.pos = i;
                break;
              }
            }
            if(this.datos.data[0].secciones[this.pos].sub_secciones[0])
            {
              this.activeName = this.datos.data[0].secciones[this.pos].sub_secciones[0].id
            }
          }else{
            this.activeName = parseInt(div2[1]);
            if (typeof div2[2] === 'undefined') {
              if (this.pos == 0) {
                for (var i in this.datos.data[0].secciones) {
                  if (this.datos.data[0].secciones[i].id == this.activeNames) {
                    this.pos = i;
                    break;
                  }
                }
              }
              for (var j in this.datos.data[0].secciones[this.pos].sub_secciones) {
                if (this.datos.data[0].secciones[this.pos].sub_secciones[j].id == this.activeName) {
                  this.posSub = j;
                  break;
                }
              }
              if(this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones[0])
              {
                this.activeNameSub = this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones[0].id
              }
            }else{
              this.activeNameSub = parseInt(div2[2]);
              if (typeof div2[3] === 'undefined') {
                if (this.pos == 0) {
                  for (var i in this.datos.data[0].secciones) {
                    if (this.datos.data[0].secciones[i].id == this.activeNames) {
                      this.pos = i;
                      break;
                    }
                  }
                }
                if (this.posSub == 0) {
                  for (var j in this.datos.data[0].secciones[this.pos].sub_secciones) {
                    if (this.datos.data[0].secciones[this.pos].sub_secciones[j].id == this.activeName) {
                      this.posSub = j;
                      break;
                    }
                  }
                }
                for (var x in this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones) {
                  if (this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones[x].id == this.activeNameSub) {
                    this.posSubH = x;
                    break;
                  }
                }
                if(this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones[this.posSubH].sub_secciones[0])
                {
                  this.activeNameSubH = this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones[this.posSubH].sub_secciones[0].id
                }
              }else{
                this.activeNameSubH = parseInt(div2[3]);
              }
            }
          }
          if(this.datos.data[0].url_redireccion == null)
          {
            setTimeout( (()=>this.loading = false), 2000)          
          }
          else {  window.location = this.datos.data[0].url_redireccion  }
      }) 
    }
  },
  watch:{
    "$route" (){
      axios.get(`https://intranet.meta.gov.co/api/micrositio/informacion/${this.$route.params.id}`)
      .then( response => {
        this.datos = response.data;
      })
      .catch(e => {
       this.errors.push(e)
     })
    }
  },
  data(){
    return{
      imagenBanner:'http://www.meta.gov.co/web/sites/default/files/img_micrositios/Prensa-01.png',
      tabPosition: 'left',
      activeNames: ['1'],
      activeName: null,
      activeNameSub: null,
      activeNameSubH: null,
      datos: '',
      loading: true,
      pos: 0,
      posSub: 0,
      posSubH: 0,
      inputsearch: ''
    }
  },
  methods:{
    buscarnorma: function(event) {
      var table;
      var filter;
      var i;
      var td;
      filter = this.inputsearch.toUpperCase();
      table = document.getElementsByClassName("tr");
      for (i = 0; i < table.length; i++) {
        td = table[i].textContent;
          if (td) {
            if (td.toUpperCase().indexOf(filter) > -1) {
              table[i].style.display = "";
            } else {
              table[i].style.display = "none";
            }
          } 
        }

    },
    setTitle(value){
       if(value){
         value = value.toLowerCase();
         return value.replace(/^\w|\s\w/g, l => l.toUpperCase())
       }
     },
     videoYoutube(urlVideo){
       let index;
    			if(urlVideo.includes('?v=')){
    				index = urlVideo.indexOf('?v=')+3;
    			}else{
    				index = urlVideo.indexOf('.be/')+4;
    			}
        	let idYoutube = urlVideo.substring(index);
    			return idYoutube;
     },
     handleChange(val) {
        guardar(val,window.location);
        axios.get(`https://intranet.meta.gov.co/api/micrositio/informacion/${this.$route.params.id}`)
        .then( response => {
            this.datos = response.data;

          if(this.$route.params.id==96){

           
             //Normatividad Inicio
          var x;
          var y;
          var i;
          var arrayfinal= new Array();
          var valorllega1=this.datos.data[0].secciones;
          var valorllega=this.datos.data[0].secciones[0].archivos;
          
          for(y=0;y<valorllega1.length;y++){
              
              var valorllega=this.datos.data[0].secciones[y].archivos;
            for (x=0;x<valorllega.length;x++){
                var res = valorllega[x].updated_at.slice(0,10);
             
                arrayfinal[x]=res;
                this.datos.data[0].secciones[y].archivos[x].updated_at= arrayfinal[x]; 
            }  
          }
          //Normatividad Fin
          } 
            for (var i in this.datos.data[0].secciones) {
              if (this.datos.data[0].secciones[i].id == val) {
                this.pos = i;
                break;
              }
            }
            if(this.datos.data[0].secciones[this.pos].sub_secciones[0])
            {
              this.activeName = this.datos.data[0].secciones[this.pos].sub_secciones[0].id
            }
        }) 
     },
     handleClick(tab, event) {
        var id = tab.$el.id+"";
        id = id.split("pane-");
        var val = id[1];
        var url = window.location+"";
        var div = url.split("open=");
        var div2 = div[1].split("_");
        if (typeof div2[1] != 'undefined') {
          val = div2[0]+"_"+val; 
        }else{
          val = div[1]+"_"+val; 
        }
        guardar(val,window.location);
        axios.get(`https://intranet.meta.gov.co/api/micrositio/informacion/${this.$route.params.id}`)
        .then( response => {
            this.datos = response.data;

          if(this.$route.params.id==96){
             //Normatividad Inicio
          var x;
          var y;
          var i;
          var arrayfinal= new Array();
          var valorllega1=this.datos.data[0].secciones;
          var valorllega=this.datos.data[0].secciones[0].archivos;
          
          for(y=0;y<valorllega1.length;y++){
              
              var valorllega=this.datos.data[0].secciones[y].archivos;
            for (x=0;x<valorllega.length;x++){
                var res = valorllega[x].updated_at.slice(0,10);
              
                arrayfinal[x]=res;
                this.datos.data[0].secciones[y].archivos[x].updated_at= arrayfinal[x]; 
            }  
          }
          //Normatividad Fin}
          }
            if (this.pos == 0) {
              for (var i in this.datos.data[0].secciones) {
                if (this.datos.data[0].secciones[i].id == this.activeNames) {
                  this.pos = i;
                  break;
                }
              }
            }
            for (var j in this.datos.data[0].secciones[this.pos].sub_secciones) {
              if (this.datos.data[0].secciones[this.pos].sub_secciones[j].id == id[1]) {
                this.posSub = j;
                break;
              }
            }
            if(this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones[0])
            {
              this.activeNameSub = this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones[0].id
            }
        }) 
     },
     handleClickSub(tab, event) {
        var id = tab.$el.id+"";
        id = id.split("pane-");
        var val = id[1];
        var url = window.location+"";
        var div = url.split("open=");
        var div2 = div[1].split("_");
        val = div2[0]+"_"+div2[1]+"_"+val; 
        guardar(val,window.location);
        axios.get(`https://intranet.meta.gov.co/api/micrositio/informacion/${this.$route.params.id}`)
        .then( response => {
            this.datos = response.data;
            if (this.pos == 0) {
              for (var i in this.datos.data[0].secciones) {
                if (this.datos.data[0].secciones[i].id == this.activeNames) {
                  this.pos = i;
                  break;
                }
              }
            }
            if (this.posSub == 0) {
              for (var j in this.datos.data[0].secciones[this.pos].sub_secciones) {
                if (this.datos.data[0].secciones[this.pos].sub_secciones[j].id == this.activeName) {
                  this.posSub = j;
                  break;
                }
              }
            }
            for (var x in this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones) {
              if (this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones[x].id == id[1]) {
                this.posSubH = x;
                break;
              }
            }
            if(this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones[this.posSubH].sub_secciones[0])
            {
              this.activeNameSubH = this.datos.data[0].secciones[this.pos].sub_secciones[this.posSub].sub_secciones[this.posSubH].sub_secciones[0].id
            }
        }) 
     },
     handleClickSubH(tab, event) {
        var id = tab.$el.id+"";
        id = id.split("pane-");
        var val = id[1];
        var url = window.location+"";
        var div = url.split("open=");
        var div2 = div[1].split("_");
        val = div2[0]+"_"+div2[1]+"_"+div2[2]+"_"+val; 
        guardar(val,window.location);
     }
  }
}
</script>

<style scoped>
.breadcrumb{
  display: flex;
  align-self: flex-start;
  margin-bottom: 15px;
  background-color: #fafafa;
  width: 100%;
  padding-top: 5px;
  padding-bottom: 5px;
  padding-left: 15px
}
p{
  font-size: 10pt;
  font-weight: 400;
  color: #4a4a4a;
  letter-spacing: 0.2px;
}
h1, h2, h3{
  color: #48576a;
}
.principal{
  height: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: white;
  width: 100%;
  padding: 12px
}
.contenido{
  display: flex;
  flex-wrap: wrap;
  margin-top: 10px;
  width: 100%
}
.texto{
  display: flex;
  flex: 5;
  min-width: 400px;
  box-shadow: 0 0 8px 0 rgba(94, 92, 92, 0.20);
  padding: 15px;
  margin-right: 20px;
  height: 100%;;
  text-align: justify;
  flex-direction: column;
}
.lateral{
  display: flex;
  flex: 1;
  min-width: 350px;
  flex-direction: column;
}
.social_botones_google_maps{
  display: flex;
  min-width: 300px;
  height: 300px;
  margin-bottom: 15px
}
.social_botones_cuadro{
  display: flex;
  max-height: 600px;
  max-width: 400px;
  background-color: #f6f6f6;
  box-shadow: 0 2px 4px 0 rgba(154, 152, 152, 0.5);
  overflow:hidden;
  margin-bottom: 20px
}
.iframe_cuadro{
  width: 100%;
  background-color: #f6f6f6;
  box-shadow: 0 2px 4px 0 rgba(154, 152, 152, 0.5);
  overflow:hidden;
  margin-bottom: 20px
}

.iframe_cuadro p iframe{
  background-color: #f6f6f6;
  box-shadow: 0 2px 4px 0 rgba(154, 152, 152, 0.5);
  overflow:hidden;
  margin-bottom: 100px
}

.social_botones_cuadro iframe{
  height: 100% !important;
  width: 100% !important
}
.social_btn{
  display: flex;
  margin-bottom: 20px;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
}
.social_botones_url_externa{
  display: flex;
  height: auto;
  max-width: 400px;
  width: auto;
  flex-wrap: wrap;
  background-color: #f6f6f6;
  box-shadow: 0 1px 1px 0 rgba(154, 152, 152, 0.5);
  overflow:auto;
  justify-content: center;
  align-items: center;
  padding: 5px 10px

}
.social_botones_url_externa img{
  width: 100%
}
.galeria{
  display: flex;
  flex-direction: row;
  flex: 3
}
.el-card{
  margin-bottom: 10px;
  display: flex;
  flex: 1;
  height: 200px
}
.image {
  width: 100%;
  display: block;
}

.clearfix:before,
.clearfix:after {
    display: table;
    content: "";
}

.clearfix:after {
    clear: both
}
.contacto_telefono{
  margin-bottom: 4px
}
.contacto_redes {
  margin-top: 10px
}
.contacto_redes a{
  margin-right: 20px
}
.el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 200px;
    margin: 0;
  }

  .el-carousel__item:nth-child(2n) {
    background-color: white;
  }

  .el-carousel__item:nth-child(2n+1) {
    background-color: white;
  }
  .el-carousel__container{
    height: 500px !important;
    display: flex;
    justify-content: center;
  }
  .secciones_docs{
    display: flex;
    flex-direction: row;
    width: 100%;
    padding: 10px;
    margin-top: 10px;
    align-items: center;
    background-color: #f6f6f6;
    box-shadow: 0 2px 4px 0 rgba(154, 152, 152, 0.5);
  }
  .secciones_docs_descarga{
    display: flex;
    justify-content: space-between;
    margin-left: 30px;
    width: 100%
  }
  .secciones_docs_fecha{
    width: 15%;
  }
  .secciones_docs_fechap{
    width: 25%;
  }
  .reverse{
    display: flex;
    flex-direction: column-reverse;
  }


@media screen and (max-width: 1000px) {

  .lateral {
    min-width: 400px;
    flex:1;
  }
  .texto{
    margin-right: 10px;
    margin-bottom: 15px;
  }
  .social_botones_cuadro{
    max-width: 100%;
    min-width: 300px;
    margin-bottom: 15px;
  }

}
@media screen and (max-width: 500px) {
  p{
    font-size: 10pt;
  }
  .contenido{
    flex-direction: column;
  }
  .lateral{
    width: 100%;
    min-width: 0px;
    flex-direction: column;
    align-items: center
  }
  .texto{
    width: 100%;
    min-width: 250px;
    box-sizing: border-box;
    margin-right: 0px
  }
  .social_botones_cuadro{
    margin-right: 0px;
    min-width: 290px;
    padding: 0px;
    width: 100%;
  }

}
</style>
