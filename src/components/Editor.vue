<script setup lang="ts">
import MarkdownIt from 'markdown-it'
import defaultHtml from '~/assets/default.html?raw'
import defaultText from '~/assets/default.txt?raw'
import defaultStyle from '~/styles/github.css?raw'
import '@github/markdown-toolbar-element'

const md = new MarkdownIt({
  html: true,
  linkify: true,
  typographer: true,
})
function download(filename: string, text: string) {
  const element = document.createElement('a')
  element.setAttribute('href', `data:text/plain;charset=utf-8,${encodeURIComponent(text)}`)
  element.setAttribute('download', filename)

  element.style.display = 'none'
  document.body.appendChild(element)

  element.click()

  document.body.removeChild(element)
}

const md_raw = useStorage('md-raw', defaultText ?? '')

const preview = computed(() => {
  return md.render(md_raw.value)
})
function save() {
  const compiledHTML = defaultHtml.replace('[style]', defaultStyle).replace('[html]', preview.value)

  download('index.html', compiledHTML)
  // console.log(compiledHTML)
}
</script>

<template>
  <div flex gap-5>
    <div flex flex-1 flex-col>
      <div class="toolbar">
        <markdown-toolbar for="textarea_id">
          <md-bold>
            <button>加粗</button>
          </md-bold>
          <md-header><button>标题</button></md-header>
          <md-italic><button>斜体</button></md-italic>
          <md-link><button>链接</button></md-link>
          <md-image><button>图片</button></md-image>
        </markdown-toolbar>
        <button @click="save">
          导出为html
        </button>
      </div>
      <textarea
        id="textarea_id" v-model="md_raw" border outline-1 flex-1 style="max-width: calc(100vw - 750px);resize: vertical;"
      />
    </div>
    <div class="preview" style="width:666px;">
      <div style="margin: 0px auto; text-align: center">
        <img src="http://atcumt.com/static/ico/logo.png" width="249" height="50" vspace="20">
      </div>
      <div class="content" v-html="preview" />
    </div>
  </div>
</template>

<style>
@import '../styles/github.css';
</style>

<style lang="scss">
.toolbar {
  $fuschia: white;
  $button-bg: $fuschia;
  $button-text-color: black;
  $baby-blue: #f8faff;

  button {
    height: 40px;
    font-family: 'Helvetica', 'Arial', sans-serif;
    display: inline-block;
    font-size: 16px;
    padding: 0 10px 0 10px;
    -webkit-appearance: none;
    appearance: none;
    background-color: $button-bg;
    color: $button-text-color;
    border-radius: 4px;
    margin: 4px;
    border: none;
    cursor: pointer;
    position: relative;
    transition: transform ease-in 0.1s, box-shadow ease-in 0.25s;
    border: 1px solid gray;

    &:focus {
      outline: 0;
    }

    &:before,
    &:after {
      position: absolute;
      content: '';
      display: block;
      width: 140%;
      height: 100%;
      left: -20%;
      z-index: -1000;
      transition: all ease-in-out 0.5s;
      background-repeat: no-repeat;
    }

    &:active {
      transform: scale(0.9);
      background-color: darken($button-bg, 5%);
      box-shadow: 0 2px 25px gray;
    }

    &.animate {
      &:before {
        display: block;
        animation: topBubbles ease-in-out 0.75s forwards;
      }

      &:after {
        display: block;
        animation: bottomBubbles ease-in-out 0.75s forwards;
      }
    }
  }

  @keyframes topBubbles {
    0% {
      background-position: 5% 90%, 10% 90%, 10% 90%, 15% 90%, 25% 90%, 25% 90%, 40% 90%, 55% 90%, 70% 90%;
    }

    50% {
      background-position: 0% 80%, 0% 20%, 10% 40%, 20% 0%, 30% 30%, 22% 50%, 50% 50%, 65% 20%, 90% 30%;
    }

    100% {
      background-position: 0% 70%, 0% 10%, 10% 30%, 20% -10%, 30% 20%, 22% 40%, 50% 40%, 65% 10%, 90% 20%;
      background-size: 0% 0%, 0% 0%, 0% 0%, 0% 0%, 0% 0%, 0% 0%;
    }
  }

  @keyframes bottomBubbles {
    0% {
      background-position: 10% -10%, 30% 10%, 55% -10%, 70% -10%, 85% -10%, 70% -10%, 70% 0%;
    }

    50% {
      background-position: 0% 80%, 20% 80%, 45% 60%, 60% 100%, 75% 70%, 95% 60%, 105% 0%;
    }

    100% {
      background-position: 0% 90%, 20% 90%, 45% 70%, 60% 110%, 75% 80%, 95% 70%, 110% 10%;
      background-size: 0% 0%, 0% 0%, 0% 0%, 0% 0%, 0% 0%, 0% 0%;
    }
  }
}
</style>
