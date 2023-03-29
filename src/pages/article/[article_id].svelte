<script>
  import { params } from "@roxi/routify"
  import { onMount } from 'svelte';
  import { ethers } from 'ethers';
  import { abi as blogchainAbi, address as blogchainAddress } from '../../BlogChain.js';
  import { abi as blogTokenAbi, address as blogTokenAddress } from '../../BlogToken.js';
  import NavBar from "../../components/NavBar.svelte";
  
  let title = "title"
  let body="body"
  let writer ="writer"
  let tipAmount = 0
  let data = 0
  let uri = 0

  onMount(async () => {
    const articleId = $params.article_id;
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const blogchaincontract = new ethers.Contract(blogchainAddress, blogchainAbi, provider);
    data = await blogchaincontract.mapGetter(articleId);
    uri = await blogchaincontract.tokenURI(articleId);
    writer = data.writer
    tipAmount = data.tipAmount
    sairam()
	});

  async function get(){
    const data =  await fetch('http://127.0.0.1:5001/api/v0/cat?arg='+uri,{method: 'POST'});
    const json = await data.json()
    return json;
  }

  function sairam(){
    get().then((data)=>{
    title = data.Title
    body = data.Text;
    body = body.replace(/(?:\r\n|\r|\n)/g, "<br>");
    area.innerHTML = body;
    showTips()
    });
  }

  const IsNumeric = (num) => /^-{0,1}\d*\.{0,1}\d+$/.test(num);

  async function tipWriter(){
    const articleId = $params.article_id;
    //connecting the wallet
    await window.ethereum.request({ method: 'eth_requestAccounts' })
    const provider1 = new ethers.providers.Web3Provider(window.ethereum);
    const signer1 = provider1.getSigner()
    const blogchaincontract = new ethers.Contract(blogchainAddress, blogchainAbi, signer1);

    const tokencontract = new ethers.Contract(blogTokenAddress, blogTokenAbi, signer1);

    const tipAmount = document.getElementById("tipAmount").value

    if(IsNumeric(tipAmount)){
      //transfer the amount with token contract
      await tokencontract.transfer(writer,tipAmount)
      //modify the tip amount in mirale contract
      await blogchaincontract.tipWriter(articleId,tipAmount);
      showTips()
      alert("Successfully transferred the tip amount!!")  
    }else{
      alert("Invalid Input!!")
    }
  }

  async function showTips(){
    const articleId = $params.article_id;
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const blogchaincontract = new ethers.Contract(blogchainAddress, blogchainAbi, provider);
    data = await blogchaincontract.mapGetter(articleId);
    tipAmount = data.tipAmount
  }
</script>

<style>
  .button {
      border-radius: 4px;
      background-color: #080808;
      border: none;
      color: #FFFFFF;
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
      content: '\00bb';
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

    input[type=tipAmount] {
      padding: 15px;
      margin: 5px 0 22px 0;
      display: inline-block;
      border: none;
      background: #f1f1f1;
      border-radius: 6px;
    }

    input[type=tipAmount]:focus {
      background-color: #ddd;
      outline: none;
      border-radius: 6px;
    }
</style>

<NavBar/>

<center>
  <br>
  <br>
  <h1>{title}</h1>
  <h4>By {writer}</h4>
  <p id="area">{body}</p>
  
  <br>
  <br>
  <h5>Tips Earnt: {tipAmount} BLOG Tokens</h5>
  <h5>Do you like to support the writer?</h5>
  <input id="tipAmount" type="tipAmount" placeholder="Enter the amount" onkeypress="return (event.charCode !=8 && event.charCode ==0 || ( event.charCode == 46 || (event.charCode >= 48 && event.charCode <= 57)))">
  
  <button class="button" on:click={tipWriter}><span>Tip</span></button>
</center>