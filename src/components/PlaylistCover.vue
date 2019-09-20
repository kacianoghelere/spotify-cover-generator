<template>
  <div class="row justify-content-center mt-3 cover-generator">
    <div class="col-12 col-md-4">
      <h5 class="text-dark">Configuração</h5>
      <div class="form-group">
        <Label for="background-file" text="Imagem da capa" />
        <div class="custom-file">
          <input
            class="custom-file-input"
            type="file"
            id="background-file"
            @change="changeBackground"
          />
          <label class="custom-file-label" for="background-file">{{ backgroundName }}</label>
        </div>
      </div>
      <div class="form-group">
        <Label for="background-horizontal-axis" text="Posição X da imagem" />
        <input
          class="custom-range"
          type="range"
          min="0"
          max="100"
          step="1"
          id="background-horizontal-axis"
          v-model="backgroundHorizontalAxis"
        />
      </div>
      <div class="form-group">
        <Label for="background-vertical-axis" text="Posição Y da imagem" />
        <input
          class="custom-range"
          type="range"
          min="0"
          max="100"
          step="1"
          id="background-vertical-axis"
          v-model="backgroundVerticalAxis"
        />
      </div>
      <div class="form-group row">
        <div class="col-12">
          <Label for="backgroundGradient" text="Gradiente de fundo" />
        </div>
        <div class="col-12" id="backgroundGradient">
          <div class="row">
            <div class="col-6">
              <input
                class="form-control"
                type="color"
                id="font-color"
                v-model="gradientColor1"
              />
            </div>
            <div class="col-6">
              <input
                class="form-control"
                type="color"
                id="font-color"
                v-model="gradientColor2"
              />
            </div>
          </div>
        </div>
      </div>
      <!-- <div class="form-group">
        <Label for="background-blend-mode" text="Mesclagem de fundos" />
        <select
          class="custom-select"
          name="background-blend-mode"
          id="background-blend-mode"
          v-model="selectedBackgroundBlendMode"
        >
          <option
            v-for="(blendMode, index) in backgroundBlendMode"
            :key="index"
            :value="blendMode"
          >{{ blendMode }}</option>
        </select>
      </div> -->
      <div class="form-group">
        <Label for="title-text" text="Texto da capa" />
        <textarea
          class="form-control"
          placeholder="Texto"
          id="title-text"
          v-model="titleText"
        ></textarea>
      </div>
      <div class="form-group">
        <Label for="text-position" text="Posição do Texto" />
        <select
          class="custom-select"
          name="text-position"
          id="text-position"
          v-model="textPosition"
        >
          <option
            v-for="(position, index) in textPositions"
            :key="index"
            :value="position.value"
            :label="position.label"
          ></option>
        </select>
      </div>
      <div class="form-group">
        <Label for="font-family" text="Fonte do Texto" />
        <select
          class="custom-select"
          name="font-family"
          id="font-family"
          :value="selectedFont"
          @change="selectFont"
        >
          <option
            v-for="(font, index) in fonts"
            :key="index"
            :value="font"
            :label="font"
          ></option>
        </select>
      </div>
      <div class="form-group">
        <Label for="font-size" text="Tamanho da Fonte" />
        <select
          class="custom-select"
          name="font-size"
          id="font-size"
          v-model="selectedFontSize"
        >
          <option
            v-for="(size, index) in fontSizes"
            :key="index"
            :value="size.value"
            :label="size.label"
          ></option>
        </select>
      </div>
      <div class="form-group">
        <Label for="font-color" text="Cor da Fonte" />
        <input
          class="form-control"
          type="color"
          id="font-color"
          v-model="selectedFontColor"
        />
      </div>
      <div class="form-group">
        <Label for="background-frame" text="Borda" />
        <select
          class="custom-select"
          name="background-frame"
          id="background-frame"
          v-model="selectedFrame"
        >
          <option
            v-for="(frame, index) in frames"
            :key="index"
            :value="frame.path"
            :label="frame.label"
          ></option>
        </select>
      </div>
      <div class="form-group">
        <Label for="background-shadow" text="Opacidade da sombra sobre o fundo" />
        <input
          class="custom-range"
          type="range"
          :min="0"
          :max="0.5"
          :step="0.1"
          id="background-shadow"
          v-model="backgroundShadowOpacity"
        />
      </div>
      <div class="form-group">
        <button
          class="btn btn-primary btn-lg btn-block"
          type="button"
          @click="printPlaylistCover"
        >
          <strong>Gerar</strong>
        </button>
      </div>
    </div>
    <div class="col-12 col-md-6 mt-3 mt-md-0">
      <div class="playlist-data" id="playlist-data">
        <h5 class="text-dark">Preview</h5>
        <div
          class="playlist-cover"
          id="playlist-cover"
          ref="playlistCover"
        >
          <div class="playlist-cover-border" :style="borderStyles"></div>
          <div class="playlist-cover-text" :style="textStyles" v-html="titleText"></div>
          <div class="playlist-cover-background" :style="backgroundStyles"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import WebFont from 'webfontloader';

  import fonts from './font-types';
  import Label from './Label.vue';

  export default {
    name: 'PlaylistCover',
    components: {
      Label
    },
    data () {
      return {
        background: '',
        backgroundBlendMode: [
          'normal',
          'multiply',
          'screen',
          'overlay',
          'darken',
          'lighten',
          'color-dodge',
          'color-burn',
          'hard-light',
          'soft-light',
          'difference',
          'exclusion',
          'hue',
          'saturation',
          'color',
          'luminosity',
        ],
        backgroundHorizontalAxis: 50,
        backgroundShadowOpacity: 0.3,
        backgroundVerticalAxis: 50,
        backgroundName: 'Selecionar',
        gradientColor1: '#ffffff',
        gradientColor2: '#ffffff',
        titleText: '',
        output: null,
        fonts,
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
          { label: 'Borda 6', path: 'border-6.png' },
          { label: 'Borda 7', path: 'border-7.png' },
          { label: 'Borda 8', path: 'border-8.png' },
          { label: 'Borda 9', path: 'border-9.png' },
          { label: 'Borda 10', path: 'border-10.png' },
          { label: 'Borda 11', path: 'border-11.png' },
        ],
        selectedBackgroundBlendMode: 'normal',
        selectedFont: 'Fondamento',
        selectedFontColor: '#ffffff',
        selectedFontSize: '4.5em',
        selectedFrame: 'border-1.png',
        textPosition: 'center',
        textPositions: [
          { label: 'Superior', value: 'flex-start' },
          { label: 'Centro', value: 'center' },
          { label: 'Inferior', value: 'flex-end' }
        ],
      }
    },
    computed: {
      borderStyle () {
        return `url("${this.borderUrl}")`
      },
      borderUrl () {
        return `./assets/${this.selectedFrame}`
      },
      backgroundStyles () {
        const gradient = `linear-gradient(${this.gradientColor1}, ${this.gradientColor2})`;

        const background = `url(${this.background})`;

        return {
          'background-image': `${background}, ${gradient}`,
          'background-position': `${this.backgroundHorizontalAxis}% ${this.backgroundVerticalAxis}%`,
          'background-blend-mode': `${this.selectedBackgroundBlendMode}`,
          'color': `${this.selectedFontColor}`,
          'font-family': `${this.selectedFont}`,
          'font-size': `${this.selectedFontSize}`,
        };
      },
      borderStyles () {
        return { 'background-image': `${this.borderStyle}` };
      },
      textStyles () {
        const shadow = `rgba(0, 0, 0, ${this.backgroundShadowOpacity})`;

        const textGradient = `linear-gradient(transparent, ${shadow}, ${shadow}, transparent)`;

        return {
          'background-image': textGradient,
          'color': `${this.selectedFontColor}`,
          'font-family': `${this.selectedFont}`,
          'font-size': `${this.selectedFontSize}`,
          'justify-content': `${this.textPosition}`
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
  @import url('https://fonts.googleapis.com/css?family=Fondamento');

  .cover-generator {
    font-family: 'Fondamento';
  }

  .playlist-cover {
    height: 500px;
    position: relative;
    text-align: center;
    width: 500px;

    .playlist-cover-background,
    .playlist-cover-border,
    .playlist-cover-text {
      height: 100%;
      position: absolute;
      width: 100%;
      z-index: 1;
    }

    .playlist-cover-background {
      background-color: #eee;
      background-position: center;
      background-size: cover;
    }

    .playlist-cover-text {
      align-items: center;
      display: flex;
      flex-direction: column;
      font-size: 4.5em;
      text-shadow: 0 3px 2px rgba(black, .9);
      z-index: 2;
    }

    .playlist-cover-border {
      background-position: center;
      background-size: cover;
      z-index: 3;
    }
  }
</style>