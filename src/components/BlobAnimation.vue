<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from "vue";

const props = defineProps({
  state: {
    type: String,
    required: true,
    validator: (value) => ["dormant", "listening", "speaking"].includes(value),
  },
});

// Create refs for each video element
const dormantVideo = ref(null);
const listeningVideo = ref(null);
const speakingVideo = ref(null);
const blobClip = ref(null);

const updateState = (newState) => {
  if (!blobClip.value) return;

  // Reset clip animation
  blobClip.value.style.animation = "none";
  blobClip.value.offsetHeight; // Trigger reflow

  // Pause all videos and fade out
  [dormantVideo.value, listeningVideo.value, speakingVideo.value].forEach(
    (video) => {
      if (!video) return;
      video.pause();
      video.style.opacity = "0";
    },
  );

  // Set new state
  switch (newState) {
    case "dormant":
      blobClip.value.style.width = "100px";
      blobClip.value.style.height = "100px";
      blobClip.value.style.transform = "translateY(100px)";
      blobClip.value.style.animation = "dormant 4s ease-in-out infinite";
      dormantVideo.value?.play();
      dormantVideo.value.style.opacity = "1";
      break;
    case "listening":
      blobClip.value.style.width = "225px";
      blobClip.value.style.height = "225px";
      blobClip.value.style.transform = "translateY(0)";
      blobClip.value.style.animation =
        "listening-shape 3s ease-in-out infinite";
      listeningVideo.value?.play();
      listeningVideo.value.style.opacity = "1";
      break;
    case "speaking":
      blobClip.value.style.width = "300px";
      blobClip.value.style.height = "300px";
      blobClip.value.style.transform = "translateY(0)";
      blobClip.value.style.animation =
        "speaking-shape 2s cubic-bezier(0.4, 0, 0.2, 1) infinite";
      speakingVideo.value?.play();
      speakingVideo.value.style.opacity = "1";
      break;
  }
};

// Watch for state changes
watch(
  () => props.state,
  (newState) => {
    updateState(newState);
  },
);

onMounted(() => {
  // Initialize videos
  [dormantVideo.value, listeningVideo.value, speakingVideo.value].forEach(
    (video) => {
      video?.load();
      video?.play().then(() => video.pause());
    },
  );

  // Set initial state
  updateState(props.state);
});

// Cleanup
onBeforeUnmount(() => {
  [dormantVideo.value, listeningVideo.value, speakingVideo.value].forEach(
    (video) => {
      video?.pause();
    },
  );
});
</script>

<template>
  <div class="blob-container">
    <div ref="blobClip" class="blob-clip">
      <video ref="dormantVideo" class="video-layer" loop muted playsinline>
        <source
          :src="'https://clients.tuimedia.com/matt/vidblob/dormant3.webm'"
          type="video/webm"
        />
      </video>
      <video ref="listeningVideo" class="video-layer" loop muted playsinline>
        <source
          :src="'https://clients.tuimedia.com/matt/vidblob/listening3.webm'"
          type="video/webm"
        />
      </video>
      <video ref="speakingVideo" class="video-layer" loop muted playsinline>
        <source
          :src="'https://clients.tuimedia.com/matt/vidblob/speaking3.webm'"
          type="video/webm"
        />
      </video>
    </div>
  </div>
</template>
