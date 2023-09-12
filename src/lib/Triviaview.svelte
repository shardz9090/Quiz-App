<script>
  import Info from "./details/Info.svelte";
  import { tweened } from "svelte/motion";
  import { Alert } from "flowbite-svelte";
  import Result from "./details/Result.svelte";

  export let listque;
  export let sendlen;

  // console.log(sendlen);
  let check = sendlen;
  let qnum = 0;
  let qno = 0;
  let original = 5;
  let timedout = 0;
  $: timer = tweened(original);
  let totalTime = 0;
  let skipped = 0;
  let correct = 0;
  let incorrect = 0;
  let showConfirmation = false;
  let selectedAnswer = null;
  let skipChecked = false;

  let showTimeoutAlert = false;
  let obtainedmarks = 0;
  let giveans = [];
  let noans = [];
  let resultans = [];
  let corrselans = [];
  let skiper = [];

  $: {
    if ($timer < 0) {
      handleTimerComplete();
    }
    if ($timer === 0) {
      handleTimerComplete();
    }

    if (check === 0) {
      $timer = 1000;
    }
  }
  // ------ don't need to modify code below
  setInterval(() => {
    if ($timer > 0) $timer--;
  }, 1000);

  $: seconds = Math.floor($timer);

  //handle timer
  function resetTimer() {
    timer.set(original);
    selectedAnswer = null;
  }

  //next
  function handleNext() {
    if (!skipChecked && !selectedAnswer) {
      alert("Select any button");
      return;
    }
    if (skipChecked) {
      selectedAnswer = null;
      showConfirmation = true;
    } else {
      qnum = qno;
      giveans.push(qnum);
      resultans.push(listque[qno]);

      const sel_ans = document.querySelector('input[name="answer"]:checked');
      const sel_value = sel_ans.value;
      corrselans.push(sel_value);
      if (sel_value === listque[qno].correct_answer) {
        correct += 1;
        addmarks($timer);
      } else {
        incorrect += 1;
      }
      if (qno < listque.length - 1) {
        qno++;
        resetTimer();
      }
      totalTime += original - seconds;
      check--;
    }
  }
  //handle the timer reaching 0
  function handleTimerComplete() {
    corrselans.push("noanswer");
    resultans.push(listque[qno]);

    qnum = qno;
    noans.push(qnum);
    showTimeoutAlert = true;
    if (qno < listque.length - 1) {
      qno++;
      resetTimer();
    }
    setTimeout(() => {
      showTimeoutAlert = false;
    }, 3000);
    totalTime += original - seconds;
    timedout += 1;
    check--;
  }
  // skip
  function handleConfirmSkip() {
    qnum = qno;
    noans.push(qnum);
    corrselans.push("noanswer");
    resultans.push(listque[qnum]);
    console.log(resultans);
    skiper.push(qnum);

    if (qno < listque.length - 1) {
      qno++;
      skipped++;
      resetTimer();
    }
    totalTime += original - seconds;
    check--;
    closeConfirmation();
  }
  //modal
  function closeConfirmation() {
    skipChecked = !skipChecked;
    showConfirmation = false;
  }

  function getColorClass(timer) {
    if (timer >= 45) {
      return "bg-green-500";
    } else if (timer >= 25) {
      return "bg-yellow-200";
    } else if (timer >= 5) {
      return "bg-orange-300";
    } else {
      return "bg-red-500";
    }
  }

  $: colorClass = getColorClass($timer);

  //marks add
  function addmarks(timer) {
    if (timer >= 45) {
      obtainedmarks += 10;
    } else if (timer >= 15 && timer < 45) {
      obtainedmarks += 7;
    } else if (timer >= 5 && timer < 15) {
      obtainedmarks += 5;
    } else if (timer > 0 && timer < 5) {
      obtainedmarks += 3;
    }
  }
</script>

<div class=" questions-cat mx-2 md:mx-2">
  {#if listque.length > 0}
    {#if check == 0}
      <Result
        {corrselans}
        {resultans}
        {obtainedmarks}
        {sendlen}
        {correct}
        {incorrect}
        {skipped}
        {timedout}
      />
    {:else}
      <div class="alertcontainer h-10 w-1/2 mx-20 p-1">
        <div class="mx-1">
          {#if showTimeoutAlert}
            <Alert>
              <span class="font-medium">Timedout!</span>
              Previous Question was timed out.
            </Alert>
          {/if}
        </div>
      </div>
      <div
        class="grid grid-cols-1 gridans md:grid-cols-[7fr,3fr] gap-4 md:px-10"
      >
        <div class="bg-inherit">
          <div class="ques_container md:pt-2 md:px-20 space-y-4">
            <div class="qescont">
              <div
                class="p-4 h-full w-30 bg-yellow-100 mx-auto rounded-xl shadow-lg flex items-center space-x-9"
              >
                <div
                  class="text-black font-medium text-2xl whitespace-normal overflow-hidden"
                >
                  <u class="font-medium text-sm">Q no.{sendlen - check + 1})</u>
                  {@html listque[qno].question}
                </div>
              </div>
            </div>
            <div>
              <h1 class="text-2xl font-semibold text-gray-800">
                <span class="inline-block relative">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-6 w-6 text-gray-700"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"
                    />
                  </svg>
                  {#if $timer <= 5}
                    <span
                      class="animate-ping absolute top-1 right-0.5 block h-3 w-3 rounded-full ring-2 ring-green-800 bg-green-800"
                    />
                  {/if}
                </span>
                <span class="secs text-yellow-200">{seconds}</span> sec!
              </h1>
              <div
                class="w-full bg-gray-200 rounded-full h-2.5 dark:bg-gray-700"
              >
                <div
                  class="{colorClass} h-2.5 rounded-full"
                  style="width: {100 * ($timer / original)}%"
                />
              </div>
            </div>
            <div class="grid grid-cols-1 gap-5 md:grid-cols-2 md:gap-10">
              {#each [listque[qno].correct_answer, ...listque[qno].incorrect_answers] as answer}
                <label
                  class="border-4 p-2 w-full max-w-sm mx-auto items-center bg-green-400 rounded-xl shadow-lg flex items-center space-x-4 hover:bg-yellow-100"
                >
                  <input
                    type="radio"
                    class="form-radio text-indigo-600 h-7 w-7 bg-white"
                    name="answer"
                    value={answer}
                    bind:group={selectedAnswer}
                  />
                  <p class="text-black font-medium">{@html answer}</p>
                </label>
              {/each}
            </div>
          </div>
          <div
            class="button_container flex justify-center space-x-10 items-center my-5"
          >
            <div class="flex items-center mb-4">
              <input
                id="skip-checkbox"
                type="checkbox"
                value=""
                class="w-4 h-4 text-red-600 bg-gray-100 border-gray-300 rounded focus:ring-red-500 dark:focus:ring-red-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600"
                bind:checked={skipChecked}
              />
              <label
                for="red-checkbox"
                class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300"
                >Skip</label
              >
            </div>
            {#if showConfirmation}
              <div class="fixed inset-0 flex items-center justify-center z-50">
                <div class="bg-white w-96 p-6 rounded-lg shadow-lg">
                  <p>Are you sure you want to skip?</p>
                  <div class="mt-4 flex justify-end">
                    <button
                      class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full mr-4"
                      on:click={handleConfirmSkip}
                    >
                      OK
                    </button>
                    <button
                      class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-full"
                      on:click={closeConfirmation}
                    >
                      Cancel
                    </button>
                  </div>
                </div>
              </div>
            {:else if qno < listque.length - 1}
              <button
                on:click={handleNext}
                class="text-white bg-gradient-to-br from-purple-600 to-blue-500 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-blue-300 dark:focus:ring-blue-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
              >
                Next
              </button>
            {:else}
              <button
                on:click={handleNext}
                class="text-white bg-gradient-to-br from-pink-500 to-orange-400 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-pink-200 dark:focus:ring-pink-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
              >
                Finish
              </button>
            {/if}
          </div>
        </div>
        {#if qno <= listque.length - 1}
          <div class="containerinfo bg-white p-4 rounded-lg shadow-lg">
            <Info
              {skipped}
              {totalTime}
              {timedout}
              category={listque[qno].category}
              difficulty={listque[qno].difficulty}
            />

            <div class="grid grid-cols-5 md:grid-cols-6 gap-1">
              {#each Array(sendlen) as _, index}
                <div
                  class="border-4 w-10 h-10 flex items-center justify-center rounded-full"
                  style={index <= qno && giveans.includes(index)
                    ? "background-color: #68d391"
                    : index <= qno && noans.includes(index)
                    ? "background-color: #fc8181"
                    : "background-color: #c9c8c3"}
                >
                  {index + 1}
                </div>
              {/each}
            </div>
            <br />
            <hr />
            <div class="grid grid-cols-[2fr,8fr]">
              <div
                class="border-4 bg-red-400 w-10 h-10 flex items-center justify-center rounded-full"
              />
              <span class="font-medium">Timed out or Skipped Question</span>
              <div
                class="border-4 bg-green-400 w-10 h-10 flex items-center justify-center rounded-full"
              />
              <span class="font-medium">Completed Question</span>
              <div
                class="border-4 w-10 h-10 flex items-center justify-center rounded-full"
                style="background-color: #c9c8c3"
              />
              <span class="font-medium">Incomplete Question</span>
            </div>
          </div>
        {/if}
      </div>
    {/if}
  {/if}
</div>
