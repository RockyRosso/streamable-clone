<template>
    <div class="vid-container">
        <video class="vid-src">
            <source src="../../assets/media/javascript.mp4" type="video/mp4" />
        </video>

        <div class="vid-controls">
            <div class="vid-control-sec">
                <button class="vid-controls-btn"><i class="fa-solid fa-play"></i></button>
                <div class="vid-progress">
                    <div class="vid-progress-filled"></div>
                </div>
            </div>
            <div class="vid-control-sec">
                <input type="range" min="0" max="100" />
            </div>
        </div>
    </div>
</template>

<style src="../../assets/css/components/videoPlayer.scss" />