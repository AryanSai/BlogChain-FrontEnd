<script>
  import { ethers } from 'ethers';
  import { abi as miracleAbi, address as miracleAddress } from '../Miracle';
  import { abi as miracleTokenAbi, address as miracleTokenAddress } from '../MiracleToken';
  import NavBar from "../components/NavBar.svelte";
  import ArticleCard from "../components/ArticleCard.svelte";
  import { onMount } from 'svelte';
  
  let data = []
  let len = 0
  let tokenBalance

  onMount(async () => {
    let articleCount = 0;
    const provider = new ethers.providers.Web3Provider(window.ethereum);

    // Get the signer from the provider
    const signer = provider.getSigner();

    // Get the address of the current user
    const myaddress = await signer.getAddress();
    //alert(myaddress)

    //get Token Balance
    const tokencontract = new ethers.Contract(miracleTokenAddress, miracleTokenAbi, provider);
    tokenBalance = await tokencontract.balanceOf(myaddress);

    const miraclecontract = new ethers.Contract(miracleAddress, miracleAbi, provider);
    //get article count
    articleCount = await miraclecontract.articleIdCounter();
    articleCount = parseInt(articleCount,10)
    //get articles
    let a=0;
    for(var x = 0;x<articleCount;x++){
      a = await miraclecontract.mapGetter(x);
      data.push(a)
    }
    len = data.length
	});
</script>

<NavBar/>

<center>
    <h3>Trending Articles</h3>
    <h4>Token Balance: {tokenBalance} MRCLTK</h4>
</center>

{#if len > 0}
{#each data as article}
  <ArticleCard title = {article.title} writer = {article.writer} id = {article.id}/>
  <br>
{/each}
{/if}
