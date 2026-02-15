<template>
  <div class="hero">
    <div class="center-content">
      <h1 class="main-title">
        <div ref="titleEl" class="title-part">
          {{ phrases[currentIndex].title }}
        </div>
        <span ref="spanEl" class="highlight-part">
          {{ phrases[currentIndex].highlight }}
        </span>
      </h1>
      <p class="main-pragraph">A design team. ready to serve ➡ always</p>
      <button class="main-btn">learn more</button>
    </div>

    <div class="circle-area">
      <div
        v-for="(item, index) in items"
        :key="index"
        class="moving-item"
        :style="{
          '--delay': (index - 1) * -30.5 + 's',
          '--duration': index + 10 + 's',
        }"
      >
        <div class="inner-img">
          <img v-bind:src="item.src" alt="" />
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import gsap from "gsap";
import PhotoOne from "../assets/adrian-ordonez-P0W27GRvyww-unsplash.jpg";
import PhotoTwo from "../assets/clement-lauwaert-ftewoitMhUs-unsplash.jpg";
import PhotoThree from "../assets/dhiva-krishna-YApS6TjKJ9c-unsplash.jpg";
import PhotoFour from "../assets/dlxmedia-hu-UkpTGYox6RM-unsplash.jpg";
import PhotoFive from "../assets/kari-shea-1SAnrIxw5OY-unsplash.jpg";
import PhotoSxs from "../assets/logan-voss-YRU6_P4mSvw-unsplash.jpg";
import PhotoSive from "../assets/logan-weaver-lgnwvr-xs0R7ltTdTM-unsplash.jpg";
import PhotoUat from "../assets/tereza-ruba-TK-rrTgYqzo-unsplash.jpg";

const items = ref([
  { src: PhotoOne },
  { src: PhotoTwo },
  { src: PhotoThree },
  { src: PhotoFour },
  { src: PhotoFive },
  { src: PhotoSxs },
  { src: PhotoSive },
  { src: PhotoUat },
]);

const titleEl = ref(null); 
const spanEl = ref(null); 
const currentIndex = ref(0);

const phrases = [
  { title: "Websites that", highlight: "perform" },
  { title: "Apps that", highlight: "deliver" },
  { title: "Designs that", highlight: "resonate" },
];

let masterTl;

onMounted(() => {
  masterTl = gsap.timeline({ repeat: -1 });

  phrases.forEach((phrase, index) => {
    const tl = gsap.timeline({
      onStart: () => {
        currentIndex.value = index;
      },
    });

   //بيدخل من تحت 
    tl.fromTo(
      titleEl.value,
      { opacity: 0, y: 30 },
      { opacity: 1, y: 0, duration: 0.6, ease: "power2.out" },
    )
      .fromTo(
        spanEl.value,
        { opacity: 0, y: 30 },
        { opacity: 1, y: 0, duration: 0.6, ease: "power2.out" },
        "-=0.4", // يبدأ قبل م الاعلان يخلص ب 0.4s
      )

      //لما يظهر يقف شويه
      .to({}, { duration: 2 })

      //  بيخرج من فوق
      .to(titleEl.value, {
        opacity: 0,
        y: -30,
        duration: 0.5,
        ease: "power2.in",
      })
      .to(
        spanEl.value,
        { opacity: 0, y: -30, duration: 0.5, ease: "power2.in" },
        "-=0.4", //بيخرج وراه بتأخير بسيط كدا
      );

    masterTl.add(tl);
  });
});

onUnmounted(() => {
  if (masterTl) masterTl.kill();
});
</script>
<style scoped>
.hero {
  background-color: #f8f7f3;
  position: relative;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

.circle-area {
  position: absolute;
  width: 800px;
  height: 800px;
}

.moving-item {
  position: absolute;
  width: 120px;
  height: 160px;

  offset-path: path(
    "M 400,400 m -350,0 a 350,350 0 1,0 700,0 a 250,250 0 1,0 -700,0"
  );

  animation: moveOnPath 250s linear infinite;
  animation-delay: var(--delay);
  offset-rotate: 0deg;
}

.inner-img {
  width: 100%;
  height: 100%;
  /* الدوران حول نفسها واهتزاز خفيف */
  animation: wobble 20s ease-in-out infinite;
}

.inner-img img {
  width: 110px;
  height: 180px;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
  animation: imgrotate var(--delay) infinite;
  animation-duration: var(--duration);
object-fit: cover;
}
@keyframes imgrotate {
  0% {
    transform: rotateY(0deg);
  }
  100% {
    transform: rotateY(180deg);
  }
}
/* حركة المشي على الدايرة */
@keyframes moveOnPath {
  from {
    offset-distance: 0%;
  }
  to {
    offset-distance: 100%;
  }
}
.center-content {
  display: grid;
  align-items: center;
  justify-content: center;
  justify-items: center;
}
.main-title {
  font-size: 3rem;
  font-weight: bold;
  color: black;
  font-family: Verdana;
}
.text {
  color: rgba(0, 0, 0, 0.781);
}
.title {
  color: rgba(0, 0, 0, 0.521);
  font-family: Verdana;
}
.title-part {
  color: black;
  text-transform: capitalize;
}
.highlight-part {
  color: rgba(0, 0, 0, 0.521);
  text-transform: capitalize;
}
.main-pragraph {
  text-align: center;
  font-size: 20px;
  font-weight: 700;
}
.main-btn {
  width: 120px;
  height: 60px;
  border-radius: 20px;
  background-color: black;
  color: #ffffff;
  outline: none;
  border: none;
  z-index: 1;
  transition:
    transform 0.4s ease,
    box-shadow 0.4s ease;
}
.main-btn:hover {
  background: linear-gradient(
    165deg,
    rgba(204, 126, 0, 1) 0%,
    rgba(43, 0, 89, 1) 50%,
    rgba(0, 0, 0, 1) 100%
  );
  cursor: pointer;
  transform: scale(1) translateZ(5px);
  /* box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3); */
}
</style>
