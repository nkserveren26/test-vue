# Cardコンポーネントのheightを画面サイズに応じてレスポンシブに設定する

## 環境情報
Vue：3.2.0
Vuetify：3.0.0

## 手順
まず、画面にCardコンポーネントを表示します。
```App.vue
<template>
  <v-app>
    <v-card
        width="400"
        title="This is a title"
        subtitle="This is a subtitle"
        text="This is content"
      >
    </v-card>
  </v-app>
</template>
```
<br/>

こんな感じに表示されます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2485934/d4af4845-0b89-bb00-20c4-dd9c7b28f7dc.png)

次にVuetifyインスタンスを取得する関数を追加します。
scriptタグに以下のソースコードを追加します。
```App.vue
<script lang="ts" setup>
import { computed,getCurrentInstance } from 'vue';

const usevuetify = () => {
  const instance = getCurrentInstance()
  if (!instance) {
    throw new Error(`vuetify instance doesn't exist`)
  }
  return instance.proxy.$vuetify
}
</script>
```

次に、画面サイズに応じてCardの高さを返すcomputedプロパティを定義します。
この中では、Vuetifyインスタンスのdisplayプロパティを使用します。
displayプロパティの中に、画面幅のブレイクポイントのプロパティが含まれています（xsやmdなど）。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2485934/bb3726e3-370c-1ffd-3073-a10a13ea7538.png)

各ブレイクポイントのプロパティはboolean値です。
画面が各ブレイクポイントで指定されている幅未満だった場合trueとなります。
例えば、xsプロパティは画面が600px未満（スマホサイズ）の時にtrueとなります。

これらのブレイクポイントプロパティを使って、画面サイズに応じてCardの高さを動的に返すcomputedプロパティ（getCardHeight）を定義します。
今回はスマホサイズとそれ以外でCardの高さを変えてみます。
スマホとそれ以外で高さを変えたいので、xsプロパティを使います。
以下では、画面がスマホサイズ（=xsがtrue）の場合、450を返し、
それ以外（=xsがfalse）では250を返します。
```App.vue
<script lang="ts" setup>
import { computed,getCurrentInstance } from 'vue';

const usevuetify = () => {
  const instance = getCurrentInstance()
  if (!instance) {
    throw new Error(`vuetify instance doesn't exist`)
  }
  return instance.proxy.$vuetify
}

const getCardHeight = computed(function()  { 
  try {
    const vuetify = usevuetify();
    return vuetify.display.xs ? 450 : '250';
  } catch(err) {
    console.log(err);

  }
})
</script>
```

templateタグのCardコンポーネントのheightプロパティに、↑で定義したgetCardHeightを指定します。
```App.vue
<template>
  <v-app>
    <v-card
        width="400"
        :height="getCardHeight"
        title="This is a title"
        subtitle="This is a subtitle"
        text="This is content"
      >
    </v-card>
  </v-app>
</template>
```

最終的にソースコードは以下のようになります。
```App.vue
<script lang="ts" setup>
import { computed,getCurrentInstance } from 'vue';

const usevuetify = () => {
  const instance = getCurrentInstance()
  if (!instance) {
    throw new Error(`vuetify instance doesn't exist`)
  }
  return instance.proxy.$vuetify
}

const getCardHeight = computed(function()  { 
  try {
    const vuetify = usevuetify();
    return vuetify.display.xs ? 450 : '250';
  } catch(err) {
    console.log(err);
  }
})
</script>

<template>
  <v-app>
    <v-card
        width="400"
        :height="getCardHeight"
        title="This is a title"
        subtitle="This is a subtitle"
        text="This is content"
      >
    </v-card>
  </v-app>
</template>
```
