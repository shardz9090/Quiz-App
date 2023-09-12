<script>
  import Question from "./lib/Question.svelte";
  import {
    Navbar,
    NavBrand,
    NavLi,
    NavUl,
    NavHamburger,
  } from "flowbite-svelte";
  import {
    Footer,
    FooterCopyright,
    FooterLinkGroup,
    FooterLink,
  } from "flowbite-svelte";
  import "./app.css";
</script>

<svelte:head>
  <title>QUIZAPP</title>
</svelte:head>

<main class="flex flex-col min-h-screen">
  <div class="wholebody" />
  <div class="containerrr">
    <div class="navhead font-serif">
      <Navbar let:hidden let:toggle>
        <NavBrand href="/">
          <img
            src="https://flowbite.com/docs/images/logo.svg"
            class="mr-3 h-6 sm:h-9"
            alt="Flowbite Logo"
          />
          <span
            class="self-center whitespace-nowrap text-xl font-semibold dark:text-white"
          >
            <a href="/">QuizApp</a>
          </span>
        </NavBrand>
        <!-- <NavHamburger on:click={toggle} />
        <NavUl {hidden}>
          <NavLi href="/" active={true}>Home</NavLi>
        </NavUl> -->
      </Navbar>
    </div>

    <div class="slotbody md:mx-10 flex-grow">
      <Question />
    </div>
  </div>
  <div class="font-serif mt-auto">
    <Footer>
      <FooterCopyright href="/" by="Quizapp" year={2022} />
    </Footer>
  </div>
</main>

<style lang="postcss">
  :global(html) {
    background-color: theme(colors.blue.300);
  }
  main {
    background: linear-gradient(to right, #2e94f3, #fd7d74);
  }

  .containerrr {
    /* background-color: #000; */
    height: 100%;
    margin: 0;
    padding: 0;
    flex-grow: 1;
  }
  .slotbody {
    background: linear-gradient(to right, #2e94f3, #fd7d74);
    background-size: cover;
    background-position: center;
    /* min-height: 89vh; */
    /* height: 100%; */
  }
  @media screen and (max-width: 768px) {
    .slotbody {
      min-height: 78.5vh;
      background: linear-gradient(to right, #2e94f3, #fd7d74);

      background-size: cover;
      background-position: center;
      /* min-height: 79vh; */
      /* height: 100%; */
    }
    .containerrr {
      height: 100%;
      margin: 0;
      padding: 0;
      flex-grow: 1;
    }
  }
  .navhead {
    background-color: #000;
  }
  .navfoter {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    min-width: 100%;
  }
</style>
