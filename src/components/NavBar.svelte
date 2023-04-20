<center>
  <h1>BlogChain</h1>
</center>

<div class="navbar">
  <a class="{currentPage === 'Home' ? 'active' : ''}" href="/Home">🔥 Trending</a>
  <a class="{currentPage === 'NewArticle' ? 'active' : ''}" href="/NewArticle">🖊️ Write & Mint</a>
  <a class="{currentPage === 'MyArticles' ? 'active' : ''}" href="/MyArticles"> 🚀 My Portfolio</a>
  <a class="{currentPage === 'About' ? 'active' : ''}" href="/About"> 💡 Our Mission</a>
</div>

<script>
  export let currentPage;
</script>

<style>
  * {
    box-sizing: border-box;
  }
  
  h1 {
    color: rgb(0, 0, 0);
    text-shadow: 2px 2px rgb(164, 164, 164);
  }

  .navbar {
    width: 100%;
    background-color: #808080;
    overflow: auto;
    border-radius: 16px;
  }

  .navbar a {
    float: left;
    padding: 12px;
    color: white;
    text-decoration: none;
    font-size: 17px;
    width: 25%;
    text-align: center;
  }

  .navbar a.active {
    background-color: #000;
  }

  .navbar a:hover {
    background-color: #000;
  }
</style>
