<script>
  import { navigate } from "svelte-routing";
  let myaddress;
  let isConnected = false;
  async function connectWallet() {
    await window.ethereum.request({ method: "eth_requestAccounts" });
    if (window.ethereum && window.ethereum.selectedAddress) {
      myaddress = window.ethereum.selectedAddress;
      isConnected = true;
    } else {
      isConnected = false;
    }
    navigate("/Home");
  }
</script>

<center>
  <img src="src/images/cover.png" alt="Pic" style="width:100%;" />
  <br />
  <br />
  <h3>🙏 Welcome to BlogChain, the decentralised blogging platform. 🌎</h3>
  <br />
  <button class="button" on:click={connectWallet} href="/Home">
    <span>Connect Wallet 🦊</span>
  </button>
</center>

<style>
  /* Page Content */
  .button {
    border-radius: 4px;
    background-color: #080808;
    border: none;
    color: #ffffff;
    text-align: center;
    font-size: 18px;
    padding: 20px;
    width: 300px;
    transition: all 0.5s;
    cursor: pointer;
    margin: 5px;
  }

  .button span {
    cursor: pointer;
    display: inline-block;
    position: relative;
    transition: 0.5s;
  }

  .button span:after {
    content: "\00bb";
    position: absolute;
    opacity: 0;
    top: 0;
    right: -20px;
    transition: 0.5s;
  }

  .button:hover span {
    padding-right: 25px;
  }

  .button:hover span:after {
    opacity: 1;
    right: 0;
  }
</style>
