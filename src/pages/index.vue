<script lang="ts" setup>

let members = ref(["", ""])
let king = ref()
let others = ref()
let numbers = ref()
let isOpen = ref(true)

const onAdd = () => {
  members.value = members.value.concat([""])
}

const onChoice = () => {
  const ms = members.value.filter(m => Boolean(m))
  const k = Math.floor(Math.random() * (ms.length))
  king.value = ms[k]
  others.value = ms.slice(0, k).concat(ms.slice(k+1))
  numbers.value = null
}

const onShuffle = () => {
  const ns = Array.from(Array(others.value.length), (v, k) => k + 1)
  for (let i = ns.length; 1 < i; i--) {
    const k = Math.floor(Math.random() * i);
    [ns[k], ns[i - 1]] = [ns[i - 1], ns[k]];
  }
  numbers.value = ns
}
</script>

<template lang="pug">
div
  header
    nav.navbar.is-light
      div.navbar-brand
        div.navbar-item
          h1.title.is-4 オンライン王様ゲーム
  main
    section.section.py-3
      h2.title.is-4(:class="isOpen ? '' : 'my-0'") 1.参加者を決める
        | #[button.button.is-small.ml-2(@click="isOpen = !isOpen") 隠す]  
      div(:class="isOpen ? 'is-block' : 'is-hidden'")
        div.field(v-for="(member, index) in members" :key="index")
          div.control
            input.input(type="text" :placeholder="`参加者${index+1}`" v-model="members[index]")
        div.field
          div.control
            button.button.is-primary.is-fullwidth(@click="onAdd") 追加
        p.mt-2 ※空白は参加者にカウントされません
    section.section.py-3
      h2.title.is-4 2.王様を決める
      div
        div.field
          div.control
            button.button.is-warning.is-large.is-fullwidth(@click="onChoice") 王様だ～れだ
        p.is-size-2.has-text-weight-bold 王様: {{ king }}
    section.section.py-3
      h2.title.is-4 3.命令を決める
      div
        p.mt-2 例：○番が△番に□□□
        p.mt-2.mb-2 例：○番が王様に□□□
        div.field
          div.control
            button.button.is-danger.is-large.is-fullwidth(@click="onShuffle") 番号公開
        p.is-size-2.has-text-weight-bold(v-for="(member, index) in others" :key="index" v-if="numbers") {{ member }}: {{ numbers[index] }}
</template>
