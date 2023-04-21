<script>
  import { params } from "@roxi/routify";
  import { onMount } from "svelte";
  import { ethers } from "ethers";
  import {
    abi as blogchainAbi,
    address as blogchainAddress,
  } from "../../BlogChain.js";
  import {
    abi as blogTokenAbi,
    address as blogTokenAddress,
  } from "../../BlogToken.js";
  import NavBar from "../../components/NavBar.svelte";

  let title = "title";
  let body = "body";
  let writer = "writer";
  let tipAmount = 0;
  let data = 0;
  let uri = 0;
  // When the user scrolls the page, execute myFunction
  window.onscroll = function () {
    myFunction();
  };

  function myFunction() {
    var winScroll =
      document.body.scrollTop || document.documentElement.scrollTop;
    var height =
      document.documentElement.scrollHeight -
      document.documentElement.clientHeight;
    var scrolled = (winScroll / height) * 100;
    document.getElementById("myBar").style.width = scrolled + "%";
  }
  onMount(async () => {
    const articleId = $params.article_id;
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const blogchaincontract = new ethers.Contract(
      blogchainAddress,
      blogchainAbi,
      provider
    );
    data = await blogchaincontract.mapGetter(articleId);
    uri = await blogchaincontract.tokenURI(articleId);
    writer = data.writer;
    tipAmount = data.tipAmount;
    sairam();
  });

  async function get() {
    const data = await fetch("http://127.0.0.1:5001/api/v0/cat?arg=" + uri, {
      method: "POST",
    });
    const json = await data.json();
    return json;
  }

  function sairam() {
    get().then((data) => {
      title = data.Title;
      body = data.Text;
      body = body.replace(/(?:\r\n|\r|\n)/g, "<br>");
      area.innerHTML = body;
      showTips();
    });
  }

  const IsNumeric = (num) => /^-{0,1}\d*\.{0,1}\d+$/.test(num);

  async function tipWriter() {
    const articleId = $params.article_id;
    //connecting the wallet
    await window.ethereum.request({ method: "eth_requestAccounts" });
    const provider1 = new ethers.providers.Web3Provider(window.ethereum);
    const signer1 = provider1.getSigner();
    const blogchaincontract = new ethers.Contract(
      blogchainAddress,
      blogchainAbi,
      signer1
    );

    const tokencontract = new ethers.Contract(
      blogTokenAddress,
      blogTokenAbi,
      signer1
    );

    const tipAmount = document.getElementById("tipAmount").value;

    if (IsNumeric(tipAmount)) {
      //transfer the amount with token contract
      await tokencontract.transfer(writer, tipAmount);
      //modify the tip amount in mirale contract
      await blogchaincontract.tipWriter(articleId, tipAmount);
      showTips();
      on();
    } else {
      alert("Invalid Input!!");
    }
  }

  async function showTips() {
    const articleId = $params.article_id;
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const blogchaincontract = new ethers.Contract(
      blogchainAddress,
      blogchainAbi,
      provider
    );
    data = await blogchaincontract.mapGetter(articleId);
    tipAmount = data.tipAmount;
  }

  function on() {
    document.getElementById("overlay").style.display = "block";
  }

  function off() {
    document.getElementById("overlay").style.display = "none";
  }
</script>

<body>
  <div class="header">
    <NavBar />
    <br />
    <div class="progress-container">
      <div class="progress-bar" id="myBar" />
    </div>
  </div>

  <div class="content">
    <center>
      <br />
      <h2>{title}</h2>
      <h3>🔑 {writer}</h3>

      <p id="area">{body}</p>

      <br />
      <br />
      <h5>Tokens Earnt: {tipAmount} BLOG Tokens</h5>
      <input
        id="tipAmount"
        type="tipAmount"
        placeholder="Support the writer..."
        onkeypress="return (event.charCode !=8 && event.charCode ==0 || ( event.charCode == 46 || (event.charCode >= 48 && event.charCode <= 57)))"
      />

      <button class="button" on:click={tipWriter}><span>Tip</span></button>

      <div id="overlay" on:click={off}>
        <div id="text">Successfully tipped the writer!</div>
      </div>
    </center>
  </div>
</body>

<style>
  .header {
    position: fixed;
    top: 0;
    z-index: 1;
    width: 100%;
    background-color: #f1f1f1;
  }
  .content {
    padding: 100px 0;
    margin: 50px auto 0 auto;
    width: 80%;
  }
  .progress-container {
    width: 100%;
    height: 8px;
    background: #ffffff;
  }

  .progress-bar {
    height: 8px;
    border-radius: 20px;
    background: #000000;
    width: 0%;
  }

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

  input[type="tipAmount"] {
    padding: 15px;
    margin: 5px 0 22px 0;
    display: inline-block;
    border: none;
    background: #f1f1f1;
    border-radius: 6px;
  }

  input[type="tipAmount"]:focus {
    background-color: #ddd;
    outline: none;
    border-radius: 6px;
  }
</style>
