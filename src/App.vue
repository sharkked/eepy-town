<script setup lang="ts">
import Window from "./components/DeskWindow.vue";
import Taskbar from "./components/DeskTaskbar.vue";
import UIButton from './components/ui/UIButton.vue';
import Github from './components/icons/brands/LogoGithub.vue';
import Mastodon from './components/icons/brands/LogoMastodon.vue';
import Twitch from './components/icons/brands/LogoTwitch.vue';
import Twitter from './components/icons/brands/LogoTwitter.vue';
import TransFlag from './components/icons/flags/TransFlag.vue';
</script>

<template>
  <nav class="start-menu"></nav>
  <main class="desktop">
    <Window>
      <h1>this is my cool website</h1>
      <p>suck my fucking dick</p>
      <p class="blink">i'm still working on it ok!!!</p>
    </Window>
  </main>
  <Taskbar>
    <UIButton href="https://twitch.tv/sharkked">
      <Twitch />
    </UIButton>
    <UIButton href="https://twitter.com/sharkkedy">
      <Twitter />
    </UIButton>
    <UIButton href="https://wetdry.world/@sh" rel="me">
      <Mastodon />
    </UIButton>
    <UIButton href="https://github.com/sharkked">
      <Github />
    </UIButton>
    <UIButton href="https://youtu.be/Sw5T5d_XcUU">
      <TransFlag />
    </UIButton>
  </Taskbar>
</template>

<style scoped></style>
