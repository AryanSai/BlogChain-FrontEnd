<script>
  import NavBar from "/home/dmacs/Desktop/FrontEnd/src/components/NavBar.svelte";
  import { ethers } from "ethers";
  import { abi, address } from "../BlogChain.js";
  import { create } from "ipfs-http-client";
  import { escape } from "svelte/internal";

  let fcid;
  const provider = new ethers.providers.Web3Provider(window.ethereum);

  async function ipfs() {
    const client = create({ url: "http://127.0.0.1:5001/api/v0" });
    const title = document.getElementById("title").value;
    const articleBody = onProcess();
    //alert(articleBody)
    const cid = await client.add(
      escape('{"Title":"' + title + '","Text":"' + articleBody + '"}')
    );
    return cid;
  }

  function onProcess() {
    var input = document.getElementById("articleBody").value;
    if (!input) {
      // Null or undefined or bad input
      alert("Invalid input");
    }
    var output = "";
    // Replace line-breaks with "\n"
    output = input.replace(/(?:\r\n|\r|\n)/g, "\\n");
    return output;
  }

  async function publish() {
    //connecting the wallet
    await window.ethereum.request({ method: "eth_requestAccounts" });
    //getting the cid
    const title = document.getElementById("title").value;

    ipfs().then(async (data) => {
      fcid = data.path;
      const signer = provider.getSigner();
      const contract = new ethers.Contract(address, abi, signer);
      await contract.publishArticle(fcid, title);
      on();
    });
  }
  let currentPage = "NewArticle";
  function on() {
    document.getElementById("overlay").style.display = "block";
  }

  function off() {
    document.getElementById("overlay").style.display = "none";
  }
</script>

<NavBar {currentPage} />

<form>
  <br />
  <br />
  <center>
    <input
      id="title"
      class="textbox"
      type="text"
      placeholder="A magical title!"
    />
    <br />
    <br />
    <textarea
      id="articleBody"
      class="textarea"
      placeholder="This is where the magic happens..."
    />
  </center>
</form>

<center>
  <button class="button" on:click={publish}><span>Mint</span></button>
</center>

<div id="overlay" on:click={off}>
  <div id="text">To the Moon! Successfully Minted!</div>
</div>

<style>
  #overlay {
    position: fixed;
    display: none;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 2;
    cursor: pointer;
  }

  #text {
    position: absolute;
    top: 50%;
    left: 50%;
    font-size: 50px;
    color: white;
    transform: translate(-50%, -50%);
    -ms-transform: translate(-50%, -50%);
  }
  .button {
    border-radius: 4px;
    background-color: #080808;
    border: none;
    color: #ffffff;
    text-align: center;
    font-size: 18px;
    padding: 20px;
    width: 200px;
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

  .textarea {
    background-color: #fff;
    color: #000000;
    padding: 2rem;
    border: none;
    border-radius: 0.5rem;
    font-size: 1.0rem;
    line-height: 1.2;
    width: 98%;
    height: 18rem;
    font-family: sans-serif;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.4);
    transition: box-shadow 0.3s ease-in-out;
    resize: none;
  }

  .textarea:focus {
    outline: none;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.4), 0 0 0 2px #ffd300;
  }
  .textbox {
    background-color: #fff;
    color: #000000;
    padding: 1.5rem;
    border: none;
    border-radius: 0.5rem;
    font-size: 1rem;
    width: 30%;
    font-family: "Roboto", sans-serif;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.4);
    transition: box-shadow 0.3s ease-in-out;
  }

  .textbox:focus {
    outline: none;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.4), 0 0 0 2px #ffd300;
  }
</style>
