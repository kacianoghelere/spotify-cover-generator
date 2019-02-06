<template>
  <div class="row justify-content-center mt-3">
    <div class="col-12 col-md-4">
      <h5>Configuração</h5>
      <div class="form-group">
        <label class="text-muted" for="background">Imagem da capa</label>
        <div class="custom-file">
          <input type="file" class="custom-file-input" id="backgroundFile" @change="changeBackground">
          <label class="custom-file-label" for="backgroundFile">{{ backgroundName }}</label>
        </div>
      </div>
      <div class="form-group">
        <label class="text-muted" for="background-horizontal-axis">Posição X da imagem</label>
        <input
          type="range"
          class="custom-range"
          min="0"
          max="100"
          step="1"
          id="background-horizontal-axis"
          v-model="backgroundHorizontalAxis"
        />
      </div>
      <div class="form-group">
        <label class="text-muted" for="titleText">Texto da capa</label>
        <textarea class="form-control" placeholder="Texto" id="titleText" v-model="titleText"></textarea>
      </div>
      <div class="form-group">
        <label class="text-muted" for="font">Fonte do Texto</label>
        <select class="custom-select" name="font" id="font" :value="selectedFont" @change="selectFont">
          <option
            v-for="(font, index) in fonts"
            :key="index"
            :value="font.path"
            :label="font.label"
          ></option>
        </select>
      </div>
      <div class="form-group">
        <label class="text-muted" for="font-size">Tamanho da Fonte</label>
        <select class="custom-select" name="font-size" id="font-size" v-model="selectedFontSize">
          <option
            v-for="(size, index) in fontSizes"
            :key="index"
            :value="size.value"
            :label="size.label"
          ></option>
        </select>
      </div>
      <div class="form-group">
        <label class="text-muted" for="font-size">Cor da Fonte</label>
        <input class="form-control" type="color" v-model="selectedFontColor" />
      </div>
      <div class="form-group">
        <label class="text-muted" for="frame">Borda</label>
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
    </div>
    <div class="col-12 col-md-6 mt-3 mt-md-0">
      <div class="playlist-data" id="playlist-data">
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
  import WebFont from 'webfontloader';

  export default {
    name: 'PlaylistCover',
    data () {
      return {
        background: '',
        backgroundHorizontalAxis: 50,
        backgroundVerticalAxis: 50,
        backgroundName: 'Selecionar',
        publicPath: process.env.BASE_URL,
        titleText: '',
        output: null,
        fonts: [
          { label: 'Acme', path: 'Acme' },
          { label: 'Aladin', path: 'Aladin' },
          { label: 'Amethysta', path: 'Amethysta' },
          { label: 'Amita', path: 'Amita' },
          { label: 'Aref Ruqaa', path: 'Aref Ruqaa' },
          { label: 'Bad Script', path: 'Bad Script' },
          { label: 'Calligraffitti', path: 'Calligraffitti' },
          { label: 'Cardo', path: 'Cardo' },
          { label: 'Cinzel Decorative', path: 'Cinzel Decorative' },
          { label: 'Cinzel', path: 'Cinzel' },
          { label: 'Cormorant Infant', path: 'Cormorant Infant' },
          { label: 'EB Garamond', path: 'EB Garamond' },
          { label: 'El Messiri', path: 'El Messiri' },
          { label: 'Fondamento', path: 'Fondamento' },
          { label: 'Forum', path: 'Forum' },
          { label: 'Gabriela', path: 'Gabriela' },
          { label: 'Gilda Display', path: 'Gilda Display' },
          { label: 'Gloria Hallelujah', path: 'Gloria Hallelujah' },
          { label: 'Kalam', path: 'Kalam' },
          { label: 'Kaushan Script', path: 'Kaushan Script' },
          { label: 'Kurale', path: 'Kurale' },
          { label: 'Lobster Two', path: 'Lobster Two' },
          { label: 'Lobster', path: 'Lobster' },
          { label: 'Love Ya Like A Sister', path: 'Love Ya Like A Sister' },
          { label: 'Merienda One', path: 'Merienda One' },
          { label: 'Merienda', path: 'Merienda' },
          { label: 'Nothing You Could Do', path: 'Nothing You Could Do' },
          { label: 'Signika', path: 'Signika' },
          { label: 'Tangerine', path: 'Tangerine' },
        ],
        fontSizes: [
          { label: 'Pequeno', value: '3em' },
          { label: 'Normal', value: '4.5em' },
          { label: 'Grande', value: '6em' }
        ],
        frames: [
          { label: 'Borda 1', path: 'border-1.png' },
          { label: 'Borda 2', path: 'border-2.png' },
          { label: 'Borda 3', path: 'border-3.png' },
          { label: 'Borda 4', path: 'border-4.png' },
          { label: 'Borda 5', path: 'border-5.png' },
          { label: 'Borda 6', path: 'border-6.png' }
        ],
        selectedFont: 'Fondamento',
        selectedFontSize: '4.5em',
        selectedFontColor: '#ffffff',
        selectedFrame: 'border-1.png'
      }
    },
    computed: {
      customStyles () {
        const border = `url("${this.publicPath}assets/${this.selectedFrame}")`;

        const gradient = 'linear-gradient(transparent, rgba(0,0,0,.30), rgba(0,0,0,.30), transparent)';

        const background = `url(${this.background})`;

        return {
          'background-image': `${border}, ${gradient}, ${background}`,
          'background-position': `${this.backgroundHorizontalAxis}%`,
          'color': `${this.selectedFontColor}`,
          'font-family': `${this.selectedFont}`,
          'font-size': `${this.selectedFontSize}`,
        };
      }
    },
    mounted () {
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
      },
      selectFont ({ target: { value: font }}) {
        WebFont.load({
          google: {
            families: [font]
          }
        });

        this.selectedFont = font;
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
  .playlist-cover {
    background-size: cover;
    background-position: center;
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