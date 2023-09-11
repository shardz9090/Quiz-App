<script>
  export let totalTime;
  export let category;
  export let difficulty;
  export let timedout;
  export let skipped;
</script>

<div class="mb-4">
  <span class="text-gray-500 whitespace-nowrap dark:text-gray-400">
    <span class="font-bold">Total Time:</span>
    <span class="text-green-400 font-medium">{totalTime}</span> |
    <span class="font-bold">TimedOut:</span>
    <span class="text-green-400 font-medium">{timedout}</span> | <br />
    <span class="font-bold">Questions Skipped :</span>
    <span class="text-red-400 font-medium">{skipped}</span>
  </span>
</div>

<div class="mt-4">
  <p class="text-gray-700 font-normal">
    <span class="font-medium">Category:</span>
    <span class="font-bold">{category}</span>
  </p>
  <p class="text-gray-700 font-normal">
    <span class="font-medium">Difficulty:</span>
    <span class="font-bold">{difficulty}</span>
  </p>
</div>
