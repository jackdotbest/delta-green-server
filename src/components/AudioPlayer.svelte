<script>
    import { onMount, onDestroy } from "svelte";

    export let src = ""; // Accept the audio source dynamically as a prop
    let audio;
    let isPlaying = false;
    let progress = 0;
    let volume = 1; // Default volume

    // Update progress while the audio is playing
    const updateProgress = () => {
        if (audio) {
            progress = (audio.currentTime / audio.duration) * 100 || 0;
        }
    };

    // Toggle play/pause
    const togglePlay = () => {
        if (audio) {
            if (isPlaying) {
                audio.pause();
            } else {
                audio.play();
            }
        }
    };

    // Handle volume change
    const changeVolume = (e) => {
        if (audio) {
            volume = e.target.value;
            audio.volume = volume;
        }
    };

    // Seek to a new position
    const seek = (e) => {
        if (audio) {
            const rect = e.target.getBoundingClientRect();
            const clickPosition = e.clientX - rect.left;
            const newTime = (clickPosition / rect.width) * audio.duration;
            audio.currentTime = newTime;
        }
    };

    // Initialize the audio element
    onMount(() => {
        audio = new Audio(src);
        audio.volume = volume;
        audio.addEventListener("timeupdate", updateProgress);
        audio.addEventListener("play", () => (isPlaying = true));
        audio.addEventListener("pause", () => (isPlaying = false));
        audio.addEventListener("ended", () => {
            isPlaying = false;
            progress = 0;
        });
    });

    // Clean up event listeners
    onDestroy(() => {
        if (audio) {
            audio.removeEventListener("timeupdate", updateProgress);
            audio = null;
        }
    });
</script>

<style>
    .audio-player {
        display: flex;
        align-items: center;
        gap: 10px;
        width: 100%;
    }

    .play-button {
        font-size: 20px;
        cursor: pointer;
    }

    .progress-bar {
        flex-grow: 1;
        height: 10px;
        background-color: #ddd;
        border-radius: 5px;
        position: relative;
        cursor: pointer;
    }

    .progress {
        height: 100%;
        background-color: #007bff;
        border-radius: 5px;
        transition: width 0.1s;
    }

    .volume-slider {
        width: 100px;
    }
</style>

<div class="audio-player">
    <button class="play-button" on:click={togglePlay}>
        {isPlaying ? "Pause" : "Play"}
    </button>
    <div class="progress-bar" on:click={seek}>
        <div class="progress" style="width: {progress}%"></div>
    </div>
    <input
        class="volume-slider"
        type="range"
        min="0"
        max="1"
        step="0.01"
        bind:value={volume}
        on:input={changeVolume}
        title="Volume"
    />
</div>
