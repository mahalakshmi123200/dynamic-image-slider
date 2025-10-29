# dynamic-image-slider
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Dynamic Image Slider with Controls</title>
<style>
@import
url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');
* {
box-sizing: border-box;
}
body {
margin: 0;
background: #1e1e2f;
font-family: 'Poppins', sans-serif;
color: #fff;
display: flex;
justify-content: center;
align-items: center;
height: 100vh;
flex-direction: column;
padding: 20px;
}
.title-bar {
width: 600px;
display: flex;
justify-content: space-between;
align-items: center;
background: #2a2a40;
padding: 12px 20px;
border-radius: 12px;
margin-bottom: 20px;
box-shadow: 0 4px 12px rgba(0,0,0,0.5);
}
.title-bar h1 {
margin: 0;
font-weight: 600;
font-size: 1.8rem;
letter-spacing: 1.1px;
}
.btn-group button {
background: #4a90e2;
border: none;

color: white;
padding: 10px 18px;
margin-left: 12px;
border-radius: 30px;
font-weight: 600;
cursor: pointer;
transition: background 0.3s, transform 0.2s;
box-shadow: 0 4px 8px rgba(74,144,226,0.4);
user-select: none;
font-size: 0.9rem;
letter-spacing: 0.8px;
}
.btn-group button:hover {
background: #357abd;
transform: translateY(-2px);
box-shadow: 0 6px 12px rgba(53,122,189,0.6);
}
.btn-group button:active {
transform: translateY(0);
box-shadow: 0 3px 6px rgba(53,122,189,0.4);
}
.slider {
position: relative;
width: 600px;
height: 350px;
overflow: hidden;
border-radius: 16px;
box-shadow: 0 10px 30px rgba(0,0,0,0.7);
background: #111122;
}
.slides {
display: flex;
width: 100%;
height: 100%;
transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}
.slides img {
width: 600px;
height: 350px;
object-fit: cover;
user-select: none;
border-radius: 16px;
filter: brightness(0.9);
transition: filter 0.3s ease;
}
.slides img:hover {
filter: brightness(1);
}
.nav-btn {
position: absolute;

top: 50%;
transform: translateY(-50%);
background: rgba(0, 0, 0, 0.4);
border: none;
color: white;
font-size: 2.4rem;
padding: 10px 16px;
cursor: pointer;
border-radius: 50%;
user-select: none;
transition: background 0.4s, transform 0.3s;
box-shadow: 0 4px 10px rgba(0,0,0,0.7);
z-index: 10;
}
.nav-btn:hover {
background: rgba(0, 0, 0, 0.75);
transform: translateY(-50%) scale(1.15);
box-shadow: 0 6px 14px rgba(0,0,0,0.9);
}
.prev {
left: 18px;
}
.next {
right: 18px;
}
.dots {
position: absolute;
bottom: 18px;
width: 100%;
text-align: center;
z-index: 10;
}
.dot {
display: inline-block;
width: 14px;
height: 14px;
margin: 0 7px;
background: rgba(255, 255, 255, 0.5);
border-radius: 50%;
cursor: pointer;
transition: background 0.35s, transform 0.35s;
}
.dot.active {
background: #4a90e2;
transform: scale(1.3);
box-shadow: 0 0 8px #4a90e2;
}
</style>
</head>
<body>

<div class="title-bar">
<h1>Dynamic Image Slider</h1>
<div class="btn-group">
<button id="startBtn">Start</button>
<button id="pauseBtn">Pause</button>
<button id="resetBtn">Reset</button>
</div>
</div>
<div class="slider">
<div class="slides">
<img src="C:\Users\ADMIN\Pictures\image 2.png" alt="Slide 1" />
<img src="C:\Users\ADMIN\Pictures\images (2).jpg" alt="Slide 2" />
<img src="C:\Users\ADMIN\Pictures\image 3.jpg" alt="Slide 3" />
<img src="C:\Users\ADMIN\Pictures\image 4.jpg" alt="Slide 4" />
</div>
<button class="nav-btn prev" aria-label="Previous slide">&#10094;</button>
<button class="nav-btn next" aria-label="Next slide">&#10095;</button>
<div class="dots"></div>
</div>
<script>
const slidesContainer = document.querySelector('.slides');
const slides = document.querySelectorAll('.slides img');
const prevBtn = document.querySelector('.prev');
const nextBtn = document.querySelector('.next');
const dotsContainer = document.querySelector('.dots');
const startBtn = document.getElementById('startBtn');
const pauseBtn = document.getElementById('pauseBtn');
const resetBtn = document.getElementById('resetBtn');
let currentIndex = 0;
const totalSlides = slides.length;
let slideInterval = null;
// Create dots dynamically
for (let i = 0; i < totalSlides; i++) {
const dot = document.createElement('span');
dot.classList.add('dot');
if (i === 0) dot.classList.add('active');
dot.dataset.index = i;
dotsContainer.appendChild(dot);
}
const dots = document.querySelectorAll('.dot');
function updateSlider() {
slidesContainer.style.transform = `translateX(-${currentIndex * 600}px)`;
dots.forEach(dot => dot.classList.remove('active'));

dots[currentIndex].classList.add('active');
}
function nextSlide() {
currentIndex = (currentIndex + 1) % totalSlides;
updateSlider();
}
function prevSlide() {
currentIndex = (currentIndex - 1 + totalSlides) % totalSlides;
updateSlider();
}
function startAutoSlide() {
if (!slideInterval) {
slideInterval = setInterval(nextSlide, 3000);
}
}
function pauseAutoSlide() {
clearInterval(slideInterval);
slideInterval = null;
}
function resetSlider() {
currentIndex = 0;
updateSlider();
pauseAutoSlide();
}
// Event listeners
nextBtn.addEventListener('click', () => {
nextSlide();
resetAutoSlide();
});
prevBtn.addEventListener('click', () => {
prevSlide();
resetAutoSlide();
});
dots.forEach(dot => {
dot.addEventListener('click', e => {
currentIndex = parseInt(e.target.dataset.index);
updateSlider();
resetAutoSlide();
});
});
startBtn.addEventListener('click', () => {

startAutoSlide();
});
pauseBtn.addEventListener('click', () => {
pauseAutoSlide();
});
resetBtn.addEventListener('click', () => {
resetSlider();
});
function resetAutoSlide() {
pauseAutoSlide();
startAutoSlide();
}
// Start auto slide on page load
startAutoSlide();
</script>
</body>
</html>
