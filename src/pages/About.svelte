<script>
  import NavBar from "../components/NavBar.svelte";
</script>

<NavBar />

<center>
  <h2 style="text-align:center">Our Team</h2>

  <div class="flip-card">
    <div class="flip-card-inner">
      <div class="flip-card-front">
        <img
          src="src/images/aryan.png"
          alt="Pic"
          style="width:300px;height:300px;"
        />
      </div>
      <div class="flip-card-back">
        <h1>Aryan Sai</h1>
        <p class="title">Chief Blogger - BlogChain</p>
        <p class="title">
          Breaking the chains of convention with blockchain and code!
        </p>
      </div>
    </div>
  </div>
</center>
<div class="footer">
  <h2>Made with ❤️ in India 🇮🇳</h2>
</div>

<style>
  .flip-card {
    background-color: transparent;
    width: 300px;
    height: 300px;
    border-radius: 1000px;
    perspective: 1000px;
  }
  .title {
    color: white;
    font-size: 18px;
  }
  .flip-card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.6s;
    transform-style: preserve-3d;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  }

  .flip-card:hover .flip-card-inner {
    transform: rotateY(180deg);
  }

  .flip-card-front,
  .flip-card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
  }

  .flip-card-front {
    background-color: #bbb;
    color: black;
  }

  .flip-card-back {
    background-color: #000306;
    color: white;
    transform: rotateY(180deg);
  }

  .footer {
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
    padding: 15px;
    text-align: center;
    background: #000000;
  }
  h2 {
    color: rgb(255, 255, 255);
  }
</style>
