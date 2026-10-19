<script setup lang="ts">
import { ref, type Ref } from 'vue';

const isPlaying = ref(false);
const initialPlay = ref(false);
const mouseDown = ref(false);
const volumeHover = ref(false);

const progress = ref(0);

const volume = ref(100);
let setVolume = 0;

const controlsFadeTime = ref(2000);
const lastMouseMovement = ref(0);
const isMouseOnPlayer = ref(false);

const videoPlayer = ref();
const videoControls = ref();
const videoProgressBar = ref();

defineProps({
    src: String,
});

function togglePlay() {
    let video = videoPlayer.value;

    if (!video) return;
    if (video.tagName !== 'VIDEO') {
        video = videoPlayer.value;
    }

    if (video.paused) {
        video.play();
        isPlaying.value = true;
        return;
    }

    video.pause();
    isPlaying.value = false;
}

function updateVolume() {
    const video = videoPlayer.value;
    video.volume = volume.value / 100;
}

function toggleMute() {
    if (volume.value > 0) {
        setVolume = volume.value;
        volume.value = 0;

        updateVolume();

        return;
    }

    if (setVolume <= 0) {
        volume.value = 100;
    } else {
        volume.value = setVolume;
    }

    updateVolume();
}

function updateProgress() {
    const video = videoPlayer.value;
    const vidControls = videoControls.value;

    if (!video) return;

    progress.value = (video.currentTime / video.duration) * 100;

    if (!vidControls) return;

    if (progress.value === 100 || progress.value === 0) {
        // @ts-ignore
        return vidControls.classList.remove('fade-anim');
    }

    console.log(Date.now() - lastMouseMovement.value, controlsFadeTime.value);

    if (Date.now() - lastMouseMovement.value < controlsFadeTime.value)
        // @ts-ignore
        return vidControls.classList.remove('fade-anim');

    // @ts-ignore
    if (!isPlaying.value) return vidControls.classList.remove('fade-anim');
    console.log('hi');

    // @ts-ignore
    vidControls.classList.add('fade-anim');
}

function changeProgress(event: MouseEvent) {
    const video = videoPlayer.value;
    const progressBar = videoProgressBar.value;


    const newTime = (event.offsetX / progressBar.offsetWidth) * video.duration;
    video.currentTime = newTime;
}

function mouseMove() {
    const vidControls = videoControls;

    if (!vidControls) return;

    lastMouseMovement.value = Date.now();
}

</script>

<template>
    <div 
        class="vid-container" 
        @mouseenter="isMouseOnPlayer = true"
        @mouseleave="isMouseOnPlayer = false"
        @mousemove="mouseMove"
    >
        <video class="vid-src" ref="videoPlayer" @click="togglePlay" @timeupdate="updateProgress"
            @ended="isPlaying = false">
            <source src="../../assets/media/javascript.mp4" type="video/mp4" />
        </video>

        <div class="vid-controls" ref="videoControls">
            <div class="vid-control-sec" style="width: 90%">
                <button @click="togglePlay" alt="play" class="vid-controls-btn">
                    <i v-if="!isPlaying" class="fa-solid fa-play"></i>
                    <i v-if="isPlaying" class="fa-solid fa-pause"></i>
                </button>
                <div
                    class="vid-progress"
                    ref="videoProgressBar"

                    @mouseleave="mouseDown = false"
                    @mousemove="mouseDown && changeProgress($event)"
                    @mousedown="mouseDown = true"
                    @mouseup="mouseDown = false"
                    @click="changeProgress"
                >
                    <div
                        :style="`flex-basis: ${progress}%`"
                        class="vid-progress-filled"
                    ></div>
                </div>
            </div>
            <div class="vid-control-sec" style="margin-left: auto">
                <button
                    @click="toggleMute"
                    alt="volume"
                    class="vid-controls-btn"
                >
                    <i v-if="volume > 0" class="fa-solid fa-volume"></i>
                    <i v-if="volume <= 0" class="fa-solid fa-volume-xmark"></i>
                </button>
                <input
                    @input="updateVolume"
                    v-model="volume"
                    type="range"
                    min="0"
                    max="100"
                />
                <button class="vid-controls-btn">
                    <i class="fa-solid fa-expand"></i>
                </button>
            </div>
        </div>
    </div>
</template>

<style src="../../assets/css/components/videoPlayer.scss" />
