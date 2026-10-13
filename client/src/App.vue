<script setup lang="ts">
import VideoPlayer from './components/VideoPlayer.vue';

</script>

<template>
    <main>
        <h1>You did it!</h1>
        <p>
            yo yo yo whats up
        </p>
        <div class="vid-test">
            <VideoPlayer />
        </div>
    </main>
</template>

<style scoped src="../assets/css/base.scss"></style>
<style scoped lang="scss">

.vid-test {
    max-width: 90%;
    position: relative;
}

</style>
