<script>
  import NavBar from "../components/NavBar.svelte";
  let currentPage = "About";
</script>

<NavBar {currentPage} />

<center>
  <br />
  <h4>
    Blogchain's mission is to revolutionize the world of content creation and
    distribution by empowering writers to take ownership of their work and
    providing them with a new revenue stream through NFTs and a native token. We
    aim to create a platform that is transparent, secure, and verifiable, using
    the Ethereum blockchain to ensure that the ownership of content is fully
    controlled by the users. Our decentralized approach fosters engagement and
    collaboration between writers and readers, creating a sense of community
    that is rare in the traditional model. We believe that by using NFTs and a
    native token, we can create an economy within the DApp that incentivizes
    writers to create high-quality content and readers to support their favorite
    writers. Our ultimate goal is to disrupt the traditional model of content
    creation and distribution and provide a new paradigm for writers to earn a
    living from their craft.
  </h4>
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
  <h2>Built with ❤️ in India 🇮🇳</h2>
</div>

<style>
  .flip-card {
    background-color: transparent;
    width: 300px;
    height: 300px;
    border-radius: 1000px;
    perspective: 1000px;
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
    color: white;
    width: 100%;
    padding: 15px;
    text-align: center;
    background: #000000;
  }
  h2 {
    color: rgb(255, 255, 255);
  }
</style>
