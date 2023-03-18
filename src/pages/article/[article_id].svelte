<script>
  import { params } from "@roxi/routify"
  import { onMount } from 'svelte';
  import { ethers } from 'ethers';
  import { abi, address } from '/home/dmacs/Desktop/FrontEnd/src/Miracle';
  import NavBar from "../../components/NavBar.svelte";

  let final_cid;
  let title = "title"
  let body="body"
  let writer ="writer"
  let tipAmount = 0

  async function get(){
    const data =  await fetch('http://127.0.0.1:5001/api/v0/cat?arg='+final_cid,{method: 'POST'});
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

  let data = 0
  onMount(async () => {
    const articleId = $params.article_id;
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const contract = new ethers.Contract(address, abi, provider);
    data = await contract.mapGetter(articleId);
    final_cid = data.cid
    writer = data.writer
    tipAmount = data.tipAmount
    sairam()
	});

  const IsNumeric = (num) => /^-{0,1}\d*\.{0,1}\d+$/.test(num);

  async function tipWriter(){
    const articleId = $params.article_id;
    //connecting the wallet
    await window.ethereum.request({ method: 'eth_requestAccounts' })
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const signer = provider.getSigner()
    const contract = new ethers.Contract(address, abi, signer);
    const tipAmount = document.getElementById("tipAmount").value
    if(IsNumeric(tipAmount)){
      
      await contract.tipWriter(articleId,{value: ethers.utils.parseEther(tipAmount)});
      showTips()
      alert("Successful!!")  
    }else{
      alert("Invalid Input!!")
    }
  }

  async function showTips(){
    const articleId = $params.article_id;
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const contract = new ethers.Contract(address, abi, provider);
    data = await contract.mapGetter(articleId);
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
  <h5>Tips Earnt: {tipAmount/10**18} ETH </h5>
  <h5>Do you like to support the writer?</h5>
  <input id="tipAmount" type="tipAmount" placeholder="Enter the amount" onkeypress="return (event.charCode !=8 && event.charCode ==0 || ( event.charCode == 46 || (event.charCode >= 48 && event.charCode <= 57)))">
  
  <button class="button" on:click={tipWriter}><span>Tip</span></button>


</center>