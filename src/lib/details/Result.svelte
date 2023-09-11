<script>
  import jQuery from "jquery";
  const jq = window.$;
  export let corrselans;
  export let resultans;
  let checklen = resultans.length;
  let qnum = 0;

  //   function buttoncolor(answer, qnum) {
  //     if (answer === corrselans[qnum]) {
  //       return "bg-black";
  //     }
  //   }
  function buttoncolor(answer, qnum) {
    if (answer === corrselans[qnum]) {
      return "bg-black";
    } else {
      return "bg-white";
    }
  }

  function changeques(index) {
    qnum = index;
  }

  //   function checkans(answer, qnum) {
  //     if (answer === resultans[qnum].correct_answer) {
  //       return "bg-green-400";
  //     }
  //   }
  function checkans(answer, qnum) {
    if (answer === resultans[qnum].correct_answer) {
      return "bg-green-400";
    } else {
      return "bg-red-400";
    }
  }
  jQuery(document).ready(function () {
    jQuery('input[class*="checked"]').prop("checked", true);
  });
</script>

<div class="resultdiv grid grid-cols-1 md:grid-cols-[7fr,3fr] gap-4 md:px-10">
  <div class="quesansdiv md:pt-2 md:px-20 space-y-4">
    <div class="qescont">
      <div
        class="p-4 h-full w-30 bg-yellow-100 mx-auto rounded-xl shadow-lg flex items-center space-x-9"
      >
        <div
          class="text-black font-medium text-2xl whitespace-normal overflow-hidden"
        >
          <u class="font-medium text-sm">Q no.{qnum + 1}</u>
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
            class="form-radio checked text-indigo-600 h-7 w-7 {buttoncolor(
              answer,
              qnum
            )}"
            name="answer"
            value={answer}
          />
          <p
            class="text-black {answer === corrselans[qnum]
              ? 'font-bold'
              : 'font-medium'}"
          >
            {@html answer}
          </p>
        </label>
      {/each}
    </div>
  </div>
  <div class="containerinfo bg-white p-4 rounded-lg shadow-lg">
    <div class="grid grid-cols-5 md:grid-cols-5 gap-1">
      {#each Array(checklen) as _, index}
        <button
          class="bg-grey-200 hover:bg-green-400 border-4 w-10 h-10 flex items-center justify-center rounded-full"
          on:click={() => changeques(index)}
        >
          {index + 1}
        </button>
      {/each}
    </div>
  </div>
</div>
