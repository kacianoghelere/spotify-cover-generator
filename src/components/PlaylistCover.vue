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
      <label for="frame">Borda</label>
      <select class="custom-select" name="frame" id="frame" v-model="selectedFrame">
        <option
          v-for="(frame, index) in frames"
          :key="index"
          :value="frame.path"
          :label="frame.label"
        ></option>
      </select>
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
      publicPath: process.env.BASE_URL,
      titleText: '',
      output: null,
      frames: [
        { label: 'Borda 1', path: 'border-1.png' },
        { label: 'Borda 2', path: 'border-2.png' },
        { label: 'Borda 3', path: 'border-3.png' },
        { label: 'Borda 4', path: 'border-4.png' },
        { label: 'Borda 5', path: 'border-5.png' }
      ],
      selectedFrame: 'border-1.png'
    }
  },
  computed: {
    customStyles () {
      const border = `url("${this.publicPath}assets/${this.selectedFrame}")`;

      const gradient = 'linear-gradient(transparent, rgba(0,0,0,.3), transparent)';

      const background = `url(${this.background})`;

      return {
        'background-image': `${border}, ${gradient}, ${background}`
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

      const canvas = await this.$html2canvas(el);

      const a = document.createElement('a');
      a.href = canvas.toDataURL('image/jpeg').replace('image/jpeg', 'image/octet-stream');
      a.download = 'playlist-cover.jpg';
      a.click();
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
