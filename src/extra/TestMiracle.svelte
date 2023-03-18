<script>
    import { ethers } from 'ethers';
    import { abi, address } from '/home/dmacs/Desktop/front_end/src/Miracle.js';
   
    let connectedAccount = "";
    let articleCount=0
    const provider = new ethers.providers.Web3Provider(window.ethereum);
  
    async function connectWallet() {
      if (window.ethereum) {
        await window.ethereum.request({ method: 'eth_requestAccounts' })
        const signer = provider.getSigner();
        connectedAccount = await signer.getAddress();
      } else {
        alert('You need to install metamask');
      }
    }
  
    async function upload() {
      const signer = provider.getSigner()
      const contract = new ethers.Contract(address, abi, signer);
      await contract.publishArticle("QmXttLj4omjdiPBMbBhEU8FJ4a3FPtsooyvB5WeZx9ZFch","FirstPost");
    }

    async function displayMessage() {
      const contract = new ethers.Contract(address, abi, provider);
      articleCount = await contract.articleIdCounter();
    }
  </script>

<div>Connected Account: {connectedAccount}</div>
<div>Contract Message: {articleCount}</div>

<button on:click={connectWallet}>Connect wallet</button>
<button on:click={upload}>upload</button>
<button on:click={displayMessage}>count</button>
