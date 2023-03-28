<script>
  import NavBar from '/home/dmacs/Desktop/FrontEnd/src/components/NavBar.svelte';
  import { ethers } from 'ethers';
  import { abi, address } from '/home/dmacs/Desktop/FrontEnd/src/Miracle.js';
  import { create } from 'ipfs-http-client';
  import { escape } from 'svelte/internal';
  
  let fcid
  const provider = new ethers.providers.Web3Provider(window.ethereum);
  
  async function ipfs(){
      const client = create({url:"http://127.0.0.1:5001/api/v0"})
      const title = document.getElementById("title").value
      const articleBody = onProcess()
      //alert(articleBody)🦊
      const cid = await client.add(escape('{"Title":"'+title+'","Text":"'+ articleBody +'"}'))
      const tokenuri = await client.add(escape('{"Title": "'+title+'","CID": '+ cid +'"}'))
      return cid
  }

  function onProcess(){
    var input = document.getElementById("articleBody").value;
    if (!input){
      // Null or undefined or bad input
      alert("Invalid input");
    }
    var output = "";
    // Replace line-breaks with "\n"
    output = input.replace(/(?:\r\n|\r|\n)/g, '\\n');
    return output;
}

  async function publish() {
    //connecting the wallet
    await window.ethereum.request({ method: 'eth_requestAccounts' })
    //getting the cid
    const title = document.getElementById("title").value

    //alert('publish function')
    ipfs().then(async (data)=>{
      fcid = data.path
      const signer = provider.getSigner()
      const contract = new ethers.Contract(address, abi, signer);
      await contract.publishArticle(fcid,title); 
    }); 
  }
</script>

<style> 
    textarea {
      width: 99%;
      height: 350px;
      padding: 20px 20px;
      box-sizing: border-box;
      border: 2px solid #ccc;
      border-radius: 6px;
      background-color: #f8f8f8;
      font-size: 16px;
      resize: vertical;
      margin-right: 10px;
      margin-left: 10px;
    }

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
</style>

<NavBar/>

<form>
    <br>
    <br>
    <center>
        <label for="lname">Title:</label> 
        
        <input type="text" id="title" name="Title">
       <p><label for="articleBody">Type your arcticle here: </label></p>
    </center>
    <textarea id="articleBody" name="articleBody" placeholder="Write a comment" rows="8" cols="152%"> </textarea>
</form>

<center>
  <button class="button" on:click={publish}><span>Publish</span></button>
</center>