<template>
  <div class="mt-3">
    <div class="form-group">
      <label for="titleText">Texto da capa</label>
      <textarea class="form-control" placeholder="Texto" id="titleText" v-model="titleText"></textarea>
    </div>
    <div class="form-group">
      <label for="background">Fundo da capa</label>
      <div class="custom-file">
        <input type="file" class="custom-file-input" id="backgroundFile" @change="changeBackground">
        <label class="custom-file-label" for="backgroundFile">{{ backgroundName }}</label>
      </div>
    </div>
    <div class="form-group">
      <button class="btn btn-primary" type="button" @click="printPlaylistCover">Gerar</button>
    </div>
    <div class="playlist-data row my-3" id="playlist-data">
      <div class="col-6">
        <h5>Preview</h5>
        <div
          class="playlist-cover"
          id="playlist-cover"
          v-html="titleText"
          :style="customStyles"
          ref="playlistCover"
        ></div>
      </div>
      <div class="col-6">
        <h5>Output</h5>
        <img :src="output" alt="" class="playlist-cover">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PlaylistCover',
  data () {
    return {
      background: '',
      backgroundName: 'Selecionar',
      titleText: '',
      output: null
    }
  },
  computed: {
    customStyles () {
      return {
        'background-image': `url(${this.background})`
      };
    }
  },
  methods: {
    downloadURI (uri, name) {
      var link = document.createElement("a");
      link.download = name;
      link.href = uri;
      link.click();
    },
    changeBackground ({ target }) {
      const file = target.files[0];

      this.background = URL.createObjectURL(file);
      this.backgroundName = file.name;
    },
    async printPlaylistCover () {
      const el = this.$refs.playlistCover;
      // add option type to get the image version
      // if not provided the promise will return
      // the canvas.
      const options = {
        type: 'dataURL'
      }

      this.output = await this.$html2canvas(el, options);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css?family=Fondamento');

  .playlist-cover {
    background-size: cover;
    background-position: center;
    font-family: "Fondamento";
    color: white;
    text-shadow: 0 3px 2px rgba(black, .9);
    font-size: 4.5em;
    text-align: center;
    background-color: #eee;
    align-items: center;
    display: flex;
    justify-content: center;
    height: 500px;
    width: 500px;
  }
</style>
