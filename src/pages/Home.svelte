<script>
  import { ethers } from "ethers";
  import { abi as blogAbi, address as blogAddress } from "../BlogChain";
  import {
    abi as blogTokenAbi,
    address as blogTokenAddress,
  } from "../BlogToken";
  import NavBar from "../components/NavBar.svelte";
  import ArticleCard from "../components/ArticleCard.svelte";
  import { onMount } from "svelte";

  let data = [];
  let len = 0;
  let tokenBalance;
  let myaddress;
  onMount(async () => {
    let articleCount = 0;
    const provider = new ethers.providers.Web3Provider(window.ethereum);

    // Get the signer from the provider
    const signer = provider.getSigner();

    // Get the address of the current user
    myaddress = await signer.getAddress();

    //get Token Balance
    const tokencontract = new ethers.Contract(
      blogTokenAddress,
      blogTokenAbi,
      provider
    );
    tokenBalance = await tokencontract.balanceOf(myaddress);
    const blogchaincontract = new ethers.Contract(
      blogAddress,
      blogAbi,
      provider
    );
    //get article count
    articleCount = await blogchaincontract.articleIdCounter();
    articleCount = parseInt(articleCount, 10);
    //get articles
    let a = 0;
    for (var x = 0; x < articleCount; x++) {
      a = await blogchaincontract.mapGetter(x);
      data.push(a);
    }
    len = data.length;
  });
  let currentPage = "Home";
</script>

<body>
  <NavBar {currentPage}/>

  <br />
  <div style="display: flex; justify-content: space-between;">
    <div class="chip">
      🔑 {myaddress}
    </div>
    <div class="chip">
      🪙 {tokenBalance} BLOG Tokens
    </div>
  </div>
  <br />

  <center>
    <h3>Trending Articles 🔥</h3>
  </center>

  {#if len > 0}
    {#each data as article}
      <ArticleCard
        title={article.title}
        writer={article.writer}
        id={article.id}
      />
      <br />
    {/each}
  {/if}
</body>

<style>
  .chip {
    display: inline-block;
    padding: 0 25px;
    height: 50px;
    font-size: 16px;
    line-height: 50px;
    border-radius: 16px;
    background-color: #000000;
    color: white;
  }
  h3 {
    color: rgb(0, 0, 0);
    text-shadow: 2px 2px rgb(164, 164, 164);
  }
</style>
