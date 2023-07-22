<template>
  <a class="button" onmousedown="">
    <slot></slot>
  </a>
</template>

<style>
.button {
  display: flex;
  width: fit-content;
  background-color: var(--color-bg);
  justify-content: center;
}

.button:hover {
  cursor: pointer;
}

.button > * {
  margin: auto;
}
</style>
