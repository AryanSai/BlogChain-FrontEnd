<script>
  export let title;
  export let writer;
  export let id;

  import { goto } from "@roxi/routify";
  function gotoArticle() {
    $goto("../article/" + id);
  }
</script>

<div id="card" on:click={gotoArticle}>
  <center>
    <div class="row" id="rectangle">
      <div class="article">
        <div class="container">
          <h4><b>{title}</b></h4>
          <p>🔑 {writer}</p>
        </div>
      </div>
    </div>
  </center>
</div>

<style>
  .article:center {
    margin: auto;
    width: 50%;
    padding: 10px;
  }

  .article {
    box-shadow: 4px 4px 4px 4px rgba(0, 0, 0, 0.2);
    transition: 0.6s;
    width: 60%;
    padding-left: 10px;
    border-radius: 0.5rem;
    color: rgb(0, 0, 0);
    background-color: rgb(255, 255, 255);
  }

  .article:hover {
    box-shadow: 12px 16px 16px 12px rgba(0, 0, 0, 0.2);
  }

  .container {
    padding: 8px 16px;
  }

  #card:hover {
    cursor: pointer;
  }
</style>
