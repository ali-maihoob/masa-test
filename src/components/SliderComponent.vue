<template>
    <div class="slider-container" @mousemove="onMouseMove"
         @mouseenter="onMouseEnter" @mouseleave="onMouseLeave">
        <div class="top-nav hidden lg:block">
            <ul class="button-list">
                <li>
                    <a class="black" href="#">partners <i class="fas fa-chevron-down"></i></a>
                </li>
                <li>
                    <a href="#">counsels <i class="fas fa-chevron-down"></i></a>
                </li>
                <li>
                    <a href="#">Associates</a>
                </li>
                <li>
                    <a class="red" href="#">Recruitment</a>
                </li>
            </ul>
        </div>
        <div class="slides" :style="{ transform: 'translateX(' + (currentIndex * -100) + '%)' }">
            <div v-for="(slide, index) in items" :key="index" class="slide">
                <div :style="imageContainerStyle" class="image-container" @mousemove="onMouseMove"
                     @mouseenter="onMouseEnter" @mouseleave="onMouseLeave" @click="onClick">
                    <div class="info">
                        <h2>{{ slide.name }}</h2>
                        <p>{{ slide.title }}</p>
                        <a class="block text-white mt-3" href="#">
                            <svg width="40px" height="40px" viewBox="0 0 80 80" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
                                <title>Group 13</title>
                                <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
                                    <g id="Group-13" transform="translate(0.000000, -0.000000)" stroke="#FFFFFF" stroke-width="4">
                                        <g id="Group-6" transform="translate(38.000000, 0.000000)">
                                            <line x1="2.0004" y1="0" x2="2.0004" y2="80" id="Stroke-1"></line>
                                        </g>
                                        <g id="Group-6" transform="translate(40.000000, 40.000000) rotate(-270.000000) translate(-40.000000, -40.000000) translate(38.000000, 0.000000)">
                                            <line x1="2.0004" y1="0" x2="2.0004" y2="80" id="Stroke-1"></line>
                                        </g>
                                    </g>
                                </g>
                            </svg>
                        </a>
                    </div>
                    <div class="focus-overlay" :style="isHovering ? hoverFocusOverlayStyle : focusOverlayStyle">
                        <img :src="getImagePath(currentItem.first)" alt="Your Image">
                    </div>
                    <div class="reveal-circle"></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            currentIndex: 0,
            currentItem: {},
            circleX: '50%',
            circleY: '50%',
            isHovering: false,
            items: [
                {
                    "first": "one-front.jpg",
                    "second": "one-back.jpg",
                    "name": "Jaques Messeca",
                    "title": "Tax - Sport"
                },
                {
                    "first": "two-front.jpg",
                    "second": "two-back.jpg",
                    "name": "Ali Maihoob",
                    "title": "Development - Web"
                }
            ],
        };
    },
    computed: {
        imageContainerStyle() {
            return {
                position: 'relative',
                overflow: 'hidden',
                cursor: 'none',
                backgroundImage: `url(${require('@/assets/images/' + this.currentItem.second)})`,
                backgroundSize: 'cover',
                backgroundPosition: 'center',
            };
        },
        focusOverlayStyle() {
            return {
                "clip-path": `circle(0 at ${this.circleX} ${this.circleY})`,
                transition: 'all 0.2s linear'
            };
        },
        hoverFocusOverlayStyle() {
            return {
                "clip-path": `circle(150px at ${this.circleX} ${this.circleY})`
            };
        },
    },
    methods: {
        prevSlide() {
            this.currentIndex = (this.currentIndex - 1 + this.items.length) % this.items.length;
            this.currentItem = this.items[this.currentIndex];
        },
        nextSlide() {
            this.currentIndex = (this.currentIndex + 1) % this.items.length;
            this.currentItem = this.items[this.currentIndex];
        },
        getImagePath(imageName) {
            return require(`@/assets/images/${imageName}`);
        },
        handleKeyboardNavigation(event) {
            if (event.key === "ArrowLeft") {
                this.prevSlide();
            } else if (event.key === "ArrowRight") {
                this.nextSlide();
            }
        },
        onMouseMove(event) {
            const x = event.pageX - event.currentTarget.offsetLeft;
            const y = event.pageY - event.currentTarget.offsetTop;
            this.circleX = `${x - 75}px`; // Subtract half the width of the circle
            this.circleY = `${y - 75}px`; // Subtract half the height of the circle
        },
        onMouseEnter() {
            this.isHovering = true;
        },
        onMouseLeave() {
            this.isHovering = false;
        },
        onClick() {
            let temp = this.currentItem.first;
            this.currentItem.first = this.currentItem.second;
            this.currentItem.second = temp;
        }
    },
    created() {
        this.currentItem = this.items[0];
    },
    mounted() {
        document.addEventListener("keydown", this.handleKeyboardNavigation);
    },
    beforeUnmount() {
        document.removeEventListener("keydown", this.handleKeyboardNavigation);
    },
};
</script>

<style>
:root {
    --x: 50%;
    --y: 50%;
}
.slider-container {
    position: relative;
    overflow: hidden;
    height: 100vh; /* Set the container height to full screen */
}

.top-nav {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    width: 100%;
    z-index: 2;
    padding: 30px 50px;
}

.top-nav ul {
    display: flex;
    justify-content: space-between;
}
.top-nav ul a {
    font-size: 1.3rem;
    border: solid 2px #fff;
    width: 260px;
    text-align: center;
    color: #fff;
    -webkit-transition: border .4s ease-in-out;
    -o-transition: border .4s ease-in-out;
    transition: border .4s ease-in-out;
    cursor: pointer;
    display: block;
    padding: 0.5rem 1rem;
    font-weight: lighter;
    text-transform: capitalize;
    position: relative;
}
.top-nav ul a.black {
    background: #000;
    border: solid 2px #000;
}
.top-nav ul a.red {
    border: solid 2px red;
    color: red;
}
.top-nav ul a i {
    position: absolute;
    right: 20px;
    top: 30%;
}
.slides {
    display: flex;
    transition: transform 0.3s ease;
    height: 100%; /* Ensure the slides take full height */
}

.slide {
    flex: 0 0 100%;
    position: relative;
}
.slide .info {
    position: absolute;
    top: 140px;
    padding: 0 50px;
    text-align: left;
    z-index: 2;
}
.slide .info h2 {
    font-family: 'oswald', sans-serif;
    white-space: nowrap;
    font-size: 2.5rem;
    text-transform: uppercase;
    letter-spacing: .2rem;
    color: #d2d2d2;
    text-decoration: none;
    margin-bottom: 20px;
}
.slide .info p {
    white-space: nowrap;
    font-family: 'oswald', sans-serif;
    font-size: 2rem !important;
    color: #212529;
}
.slide img {
    object-fit: cover; /* Ensure images cover the entire slide area */
    width: 100%;
    height: 100%;
}

.prev-btn,
.next-btn {
    position: fixed;
    top: 50%;
    transform: translateY(-50%);
    background-color: #007bff;
    color: #fff;
    border: none;
    padding: 8px;
    cursor: pointer;
    font-size: 24px;
}

.prev-btn {
    left: 0;
}

.next-btn {
    right: 0;
}

body {
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background-color: #f1f1f1;
}

img {
    max-width: 100%;
    height: auto;
    display: block;
    cursor: none;
}

.reveal-circle {
    position: absolute;
    clip-path: circle(0 at var(--x) var(--y));
    border-radius: 50%;
    width: 150px;
    height: 150px;
    pointer-events: none; /* To prevent the circle from capturing clicks */
    display: none; /* Hide the circle initially */
    cursor: none;
}

</style>