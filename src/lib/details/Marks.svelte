<script>
  export let correct;
  export let sendlen;
  export let incorrect;
  export let timedout;
  export let skipped;
  export let obtainedmarks;

  function reloadPage() {
    window.location.reload();
  }
</script>

<div
  class="p-10 forque mx-40 bg-white rounded-xl shadow-lg flex items-center space-x-4"
>
  <div class="text-xl font-medium text-black">
    totalcorrect = {correct} / {sendlen} <br />
    totalincorrect = {incorrect} | skipped = {skipped} | marks = {obtainedmarks}
    | timed Out = {timedout} <br />
    <button
      on:click={reloadPage}
      class="text-white bg-gradient-to-br from-green-400 to-blue-600 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-green-200 dark:focus:ring-green-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
    >
      Play Again
    </button>
  </div>
</div>
