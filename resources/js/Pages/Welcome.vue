<template>
    <div class="app-container">
      <Head title="Welcome" />
      <!-- Container pentru aplicația Pixi.js -->
      <div ref="pixiContainer"></div>
  
      <!-- Butonul de autentificare și înregistrare -->
      <div v-if="canLogin" class="auth-buttons">
        <Link v-if="$page.props.auth.user" :href="route('dashboard')" class="auth-button">Dashboard</Link>
        <template v-else>
          <Link :href="route('login')" class="auth-button">Log in</Link>
          <Link v-if="canRegister" :href="route('register')" class="auth-button">Register</Link>
        </template>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import { Head, Link } from '@inertiajs/vue3';
  import * as PIXI from 'pixi.js';
  
  defineProps({
    canLogin: Boolean,
    canRegister: Boolean,
    laravelVersion: String,
    phpVersion: String,
  });
  
  const pixiContainer = ref(null);
  
  onMounted(() => {
    const app = new PIXI.Application({ resizeTo: window });
  
    pixiContainer.value.appendChild(app.view);
  
    window.addEventListener('resize', () => {
      app.renderer.resize(window.innerWidth, window.innerHeight);
    });
  
    const background = PIXI.Sprite.from('/assets/new_bacc.jpg');
    background.anchor.set(0.5);
    background.x = app.screen.width / 2;
    background.y = app.screen.height / 2;
  
    const container = new PIXI.Container();
    container.addChild(background);
    app.stage.addChild(container);
});
</script>
  
  <style>
  .app-container {
    position: relative;
  }
  
  .auth-buttons {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 1; /* Asigură că butoanele de autentificare și înregistrare sunt desenate deasupra aplicației Pixi.js */
  }
  
  .auth-button {
    font-weight: bold;
    text-decoration: none;
    color: white;
    background-color: #2f0056;
    padding: 25px 25px;
    margin: 30px 0;
    border-radius: 5px;
    display: block;
    width: 350px;
    text-align: center;
    font-size: 20px;
    font-weight: bold;
    transition: background-color 1s; 
    position: relative;
    overflow: hidden;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  }
  
  .auth-button::before {
  content: "";
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(to right, rgba(255, 255, 255, 0.3), rgba(255, 255, 255, 0));
  transition: left 0.5s, background-color 0.3s;
}

.auth-button:hover::before {
  left: 100%;
  background-color: transparent;
}

.auth-button:hover {
  background-color: #d64d00;
  font-size: 30px;
}
 </style>
  