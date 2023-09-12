<script>
  import jQuery from "jquery";
  export let corrselans;
  export let resultans;
  let checklen = resultans.length;
  let qnum = 0;

  export let correct;
  export let sendlen;
  export let incorrect;
  export let timedout;
  export let skipped;
  export let obtainedmarks;

  function reloadPage() {
    window.location.reload();
  }

  jQuery(document).ready(function () {
    jQuery('input[class*="checked"]').prop("checked", true);
    jQuery('input[class*="nocheck"]').prop("checked", false);
  });

  function buttoncolor(answer, qnum) {
    if (corrselans[qnum] === "noanswer") {
      return "nocheck";
    } else {
      if (answer === corrselans[qnum]) {
        return "checked";
      } else {
        return "nocheck";
      }
    }
  }

  function changeques(index) {
    qnum = index;
    jQuery(document).ready(function () {
      jQuery('input[class*="checked"]').prop("checked", true);
      jQuery('input[class*="nocheck"]').prop("checked", false);
    });
  }

  function checkans(answer, qnum) {
    if (answer === resultans[qnum].correct_answer) {
      return "bg-green-400";
    } else {
      return "bg-red-400";
    }
  }
</script>

<div class="md:grid md:ml-28 mt-8 mb-6 md:grid-cols-[6fr,4fr] gap-1">
  <div class="infoques space-y-4">
    <div
      class="infodiv p-4 bg-white rounded-xl shadow-lg flex items-center space-x-4"
    >
      <div class="text-xl font-medium text-black">
        Total Questions = <span class="text-yellow-400 font-medium"
          >{sendlen}</span
        >
        <br />
        Total Correct =
        <span class="text-green-400 font-medium">{correct}</span>
        | Total Incorrect =
        <span class="text-red-400 font-medium">{incorrect}</span>
        | Skipped = <span class="text-red-400 font-medium">{skipped}</span>
        | Timed Out = <span class="text-red-400 font-medium">{timedout}</span>
        <br />

        Marks = <span class="text-green-400 font-medium">{obtainedmarks}</span>
        <br />
        <button
          on:click={reloadPage}
          class="text-white bg-gradient-to-br from-green-400 to-blue-600 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-green-200 dark:focus:ring-green-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
        >
          Play Again
        </button>
      </div>
    </div>
    <div class="buttondiv">
      <div class="quesansdiv md:pt-2 space-y-4">
        <div class="qescont">
          <div
            class="p-4 h-full w-30 bg-yellow-100 mx-auto rounded-xl shadow-lg flex items-center space-x-9"
          >
            <div
              class="text-black font-medium text-2xl whitespace-normal overflow-hidden"
            >
              <u class="font-medium text-lg">Q no.{qnum + 1}</u>
              {@html resultans[qnum].question}
            </div>
          </div>
        </div>

        <div class="grid grid-cols-1 gap-5 md:grid-cols-2 md:gap-10">
          {#each [resultans[qnum].correct_answer, ...resultans[qnum].incorrect_answers] as answer}
            <label
              class="border-4 p-2 w-full max-w-sm mx-auto items-center {checkans(
                answer,
                qnum
              )} rounded-xl shadow-lg flex items-center space-x-4"
            >
              <input
                disabled
                type="radio"
                class="form-radio text-indigo-600 h-7 w-7 {buttoncolor(
                  answer,
                  qnum
                )}"
                name="answer"
                value={answer}
              />
              <p
                class="text-black {answer === corrselans[qnum]
                  ? 'font-bold'
                  : 'font-thin'}"
              >
                {@html answer}
              </p>
            </label>
          {/each}
        </div>
      </div>
    </div>
  </div>
  <div
    class="resultdiv h-48 items-center grid grid-cols-1 md:grid-cols-[7fr,3fr] gap-4"
  >
    <div class="containerinfo bg-white p-4 rounded-lg shadow-lg">
      <div class="grid grid-cols-5 md:grid-cols-5 gap-1">
        {#each Array(sendlen) as _, index}
          <button
            class="hover:bg-green-400 border-4 w-10 h-10 flex items-center justify-center rounded-full"
            on:click={() => changeques(index)}
          >
            {index + 1}
          </button>
        {/each}
      </div>
    </div>
  </div>
</div>
