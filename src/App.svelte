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

<main>
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
        <NavHamburger on:click={toggle} />
        <NavUl {hidden}>
          <NavLi href="/" active={true}>Home</NavLi>
        </NavUl>
      </Navbar>
    </div>

    <div class="slotbody bg-black">
      <Question />
    </div>
  </div>
  <div class="empty" />
  <div class="navfoter font-serif">
    <Footer>
      <FooterCopyright href="/" by="Quizapp" year={2022} />
      <!-- <FooterLinkGroup
        class="flex flex-wrap items-center mt-3 text-sm font-medium text-gray-500 dark:text-gray-400 sm:mt-0"
      >
        <FooterLink>
          <a href="/" class="mr-4 hover:underline md:mr-6">Home</a>
        </FooterLink>
      </FooterLinkGroup> -->
    </Footer>
  </div>
</main>

<style lang="postcss">
  :global(html) {
    background-color: theme(colors.blue.300);
  }

  .containerr {
    /* background-color: #000; */
  }
  .slotbody {
    background: linear-gradient(to right, #2e94f3, #ff8822);
    background-size: cover;
    background-position: center;
    min-height: 89vh;
  }
  @media screen and (max-width: 768px) {
    .slotbody {
      min-height: 78.5vh;
      background: linear-gradient(to right, #2e94f3, #ff8822);

      background-size: cover;
      background-position: center;
      min-height: 79vh;
    }
  }
  .navhead {
    background-color: #000;
  }
  .navfoter {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    min-width: 100%;
  }
  .empty {
    background: linear-gradient(to right, #2e94f3, #ff8822);

    min-height: 10vh;
  }
</style>
