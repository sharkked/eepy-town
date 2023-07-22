<script setup lang="ts">
import { ref, computed, getCurrentInstance } from "vue";

import Button from "./ui/UIButton.vue";
import Close from "./icons/IconClose.vue";
import Maximize from "./icons/IconMaximize.vue";
import Minimize from "./icons/IconMinimize.vue";

interface WindowState {
  mode: "windowed" | "fullscreen" | "minimized";
  size: {
    w: number;
    h: number;
  };
  pos: {
    x: number;
    y: number;
  };
}

const state = ref<WindowState>({
  mode: "windowed",
  size: {
    w: 480,
    h: 360,
  },
  pos: {
    x: 120,
    y: 120,
  },
});

const offset = ref<{
  x: number;
  y: number;
}>({
  x: 0,
  y: 0,
});

const style = computed(() => {
  const { size, pos } = state.value;
  const width = `width: ${size.w * 0.1}rem`;
  const height = `height: ${size.h * 0.1}rem`;
  const left = `left: ${pos.x * 0.1}rem`;
  const top = `top: ${pos.y * 0.1}rem`;

  return [width, height, left, top].join("; ");
});

function onMouseMove(event: MouseEvent) {
  state.value.pos = {
    x: event.clientX - offset.value.x,
    y: event.clientY - offset.value.y,
  };
}

function onTitlebarMousedown(event: MouseEvent) {
  if (event.button == 0) {
    const { pos } = state.value;
    offset.value = {
      x: event.clientX - pos.x,
      y: event.clientY - pos.y,
    };
    addEventListener("mousemove", onMouseMove);
  }
}

function onTitlebarMouseup(event: MouseEvent) {
  if (event.button == 0) {
    console.log("clicked");
    removeEventListener("mousemove", onMouseMove);
  }
}

function onMinimize(event: Event) {
  state.value.mode = "minimized";
}

function onMaximize(event: MouseEvent) {
  state.value.mode = "fullscreen";
}

function onClose() {}

function stopImmediatePropagation(event: Event) {
  event.stopImmediatePropagation();
}
</script>

<template>
  <div
    class="window"
    focus="true"
    v-if="state.mode == 'windowed'"
    :style="style"
  >
    <div
      class="titlebar"
      @mousedown="onTitlebarMousedown"
      @mouseup="onTitlebarMouseup"
    >
      <div class="window-title">
        <div class="favicon"></div>
        <div class="label">eepy town</div>
      </div>
      <div class="window-actions">
        <Button @click="onMinimize" @mousedown="stopImmediatePropagation" class="minimize"><Minimize /></Button>
        <Button @click="onMaximize" @mousedown="stopImmediatePropagation" class="maximize"><Maximize /></Button>
        <Button @click="onClose" @mousedown="stopImmediatePropagation" class="close"><Close /></Button>
      </div>
    </div>
    <div class="menubar">
      <slot name="menubar"></slot>
    </div>
    <div class="body">
      <div class="sidebar">
        <slot name="sidebar"></slot>
      </div>
      <div class="content">
        <slot name="default"></slot>
      </div>
    </div>
  </div>
</template>

<style>
.window {
  position: fixed;
  display: flex;
  flex-direction: column;
  color: var(--window-color);
  background: var(--window-bg);
  border: 1px solid var(--window-border);
  overflow: hidden;
}

.window .titlebar {
  display: flex;
  height: 3.6rem;
  margin-bottom: 0.4rem;
  color: var(--window-title-color);
  background-color: var(--window-title-inactive-bg);
  user-select: none;
}

.window[focus] .titlebar {
  background-color: var(--window-title-focus-bg);
}

.window-title {
  display: flex;
  align-items: center;
  flex: auto;
}

.window-actions {
  display: flex;
}

.window-actions .button {
  color: currentColor;
  margin: 0;
  padding: 0.3rem;
  width: 4rem;
}

.window-actions .icon {
  width: 1rem;
  height: 1rem;
  background-repeat: no-repeat;
  background-size: cover;
}

.window .body {
  flex: auto;
  display: flex;
}

.content {
  padding: 1.5rem;
  flex: auto;
}
</style>
