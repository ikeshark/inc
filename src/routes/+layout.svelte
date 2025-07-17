<script>
	import '../app.css';

	let { children } = $props();
</script>

<header>
  <h1 class="bold"><a href="/">inc inc inc</a></h1>
  <nav>
    <ul>
      <li>
        <a href="/about">
          about
        </a>
      </li>
      <li>
        <a href="/listen">
          listen
        </a>
      </li>
      <li>
        <a href="/icp">
          ice cream in panama
        </a>
      </li>
    </ul>
  </nav>
</header>

<style>
  header {
    background-color: #efefefab;
    padding: 0.5rem 1rem;
    display: flex;
    justify-content: space-between;
  }
  h1 {
    font-size: 1.75rem;
    font-weight: 900;
    line-height: 1;
    width: 4ch;
  }
  @media (min-width: 765px) {
    h1 {
      font-size: 2.25rem;
    }
  }
  ul {
    font-size: 1.1rem;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    height: 100%;
  }
  li {
    text-align: right;
  }
  nav a {
    letter-spacing: 0.2rem;
    font-weight: 600;
  }
  nav a:hover {
    text-decoration: underline;
  }
</style>
  
{@render children()}
