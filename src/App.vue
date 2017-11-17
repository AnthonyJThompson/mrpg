<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'app'
}
if ('serviceWorker' in navigator && process.env.NODE_ENV === 'production') {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/sw.js').then(registration => {
      console.log('SW registered: ', registration)
    }).catch(registrationError => {
      console.log('SW registration failed: ', registrationError)
    })
  })
}
</script>

<style>
* {
     user-select: none;
    -khtml-user-select: none;
    -o-user-select: none;
    -moz-user-select: -moz-none;
    -webkit-user-select: none;
}

::selection { background: transparent;color:inherit; }
::-moz-selection { background: transparent;color:inherit; }
</style>
