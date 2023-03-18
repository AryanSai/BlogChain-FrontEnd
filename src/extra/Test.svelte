<script>
    import { ethers } from 'ethers';
    import { abi, address } from '/home/dmacs/Desktop/front_end/src/Greet.js';
   
    let connectedAccount = "";
    let contractMessage = "";

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
  
    async function displayMessage() {
      const contract = new ethers.Contract(address, abi, provider);
      contractMessage = await contract.message();
    }

    // async function updateMessage() {
    //   const contract = new ethers.Contract(address, abi, provider);
    //   const newmessage = await contract.setMessage("Hello Aryan");
    //   await newmessage.wait()
    // }
  </script>

<div>Connected Account: {connectedAccount}</div>
<div>Contract Message: {contractMessage}</div>

<button on:click={connectWallet}>Connect wallet</button>
<button on:click={displayMessage}>Read Message</button>