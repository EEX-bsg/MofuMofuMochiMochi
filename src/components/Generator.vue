<script setup>
  import {ref} from 'vue';
  let charCount = 4;
  let settingChecked = ref(false);
  const result = ref("もちもふジェネレータ")
  const isVisiableTweet = ref(false);
  const firstCharFixToMo = ref(true);
  const oddCharFixToMo = ref(false);
  const lukkeyMode = ref(true);

  const generateText =()=> {
    let text = "";
    if(oddCharFixToMo.value){
      firstCharFixToMo.value = true
    }
    if(firstCharFixToMo.value){
      text = "も";
    }
    let lukkey = Math.random()*100 < 1;
    if(lukkeyMode.value&&lukkey){
      text = "な";
    }
    let array = ["も", "ち", "ふ"]
    //2文字以上12文字以下にクランプ
    charCount = Math.min(Math.max(charCount, 2),100);

    let count = (firstCharFixToMo.value||(lukkeyMode.value&&lukkey) ? charCount-1 : charCount);
    for(let i=0; i<count; i++){
      if(lukkeyMode.value && lukkey){
        text += (i%2==1?"な":"で");
      }
      else if(oddCharFixToMo.value){
        if(i%2==1){
          text+="も";
        }
        else{
          if(Math.random()<0.5){
            text+="ち"
          }
          else{
            text+="ふ"
          }
        }
      }
      else{
        let r = Math.floor(Math.random()*3);
        text += array[r];
      }
    }
    //ツイートボタン表示
    isVisiableTweet.value = true;

    //描画
    result.value = text;
  }

  const tweet =()=> {
    const text = encodeURI(result.value+"\n");
    window.open("http://twitter.com/share?text="+text+"&url=https%3A%2F%2Feex-bsg.github.io%2FMofuMofuMochiMochi%2F")
  }
</script>

<template>
  <div id="app">
    <div id="generator-container">
      <h1>{{result}}</h1>
      <button type="button" @click="generateText">生成する</button>
      <button type="button" v-show="isVisiableTweet" @click="tweet" class="tweet-button">ツイートする</button>
      <div class="setting-checkbox-container"><input type="checkbox" class="setting-checkbox" v-model="settingChecked">詳細設定</div>
    </div>
    <div id="setting-container" v-show="settingChecked">
      <div>文字数:<input v-model="charCount" type="number" min="4" max="100"></div>
      <div>最初を「も」に固定<input type="checkbox" v-model="firstCharFixToMo" checked></div>
      <div>もちもふも～ど<input type="checkbox" v-model="oddCharFixToMo"></div>
      <div>らっき～も～ど<input type="checkbox" v-model="lukkeyMode" checked></div>
    </div>
  </div>
  
</template>

<style>

.tweet-button {
  background-color: #1da1f2;
}
.setting-checkbox-container{
  font-size: 1.1em;
  color: #AAAAAA;
  margin: 0.3em
}

</style>