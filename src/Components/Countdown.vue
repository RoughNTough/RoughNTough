<template>
  <div v-if="remaining.total > 0" class="countdown">
    <span>{{ remaining.days }}D</span> :
    <span>{{ remaining.hours }}H</span> :
    <span>{{ remaining.minutes }}M</span> :
    <span>{{ remaining.seconds }}S</span>
  </div>
</template>

<script setup>
import { computed, onMounted, onUnmounted, ref } from "vue";

const props = defineProps({ target: { type: Date, required: true } });
const now = ref(new Date());

let timer;
onMounted(() => {
  timer = setInterval(() => (now.value = new Date()), 1000);
});
onUnmounted(() => clearInterval(timer));

const remaining = computed(() => {
  const diff = props.target - now.value;
  return {
    total:    diff,
    days:     Math.max(0, Math.floor(diff / 864e5)),
    hours:    Math.max(0, Math.floor((diff % 864e5) / 36e5)),
    minutes:  Math.max(0, Math.floor((diff % 36e5) / 6e4)),
    seconds:  Math.max(0, Math.floor((diff % 6e4) / 1e3)),
  };
});
</script>

<style scoped>
.countdown {
  font-size: 1.5rem;
  font-weight: 600;
  letter-spacing: 0.03em;
  margin-top: 1rem;
  display: inline-flex;
  gap: 0.3em;
}
</style>
