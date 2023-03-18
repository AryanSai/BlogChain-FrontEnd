<script>
  import { ethers } from 'ethers';
  import { abi, address } from '../Miracle';
  import NavBar from "../components/NavBar.svelte";
  import ArticleCard from "../components/ArticleCard.svelte";
  import { onMount } from 'svelte';
  
  let data = []
  let len = 0
  onMount(async () => {
    let articleCount = 0;
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    const contract = new ethers.Contract(address, abi, provider);
    //get article count
    articleCount = await contract.articleIdCounter();
    articleCount = parseInt(articleCount,10)
    //get articles
    let a=0;
    for(var x = 0;x<articleCount;x++){
      a = await contract.mapGetter(x);
      data.push(a)
    }
    len = data.length
	});
</script>

<NavBar/>

<center>
    <h3>Trending Articles</h3>
</center>

{#if len > 0}
{#each data as article}
  <ArticleCard title = {article.title} writer = {article.writer} id = {article.id}/>
  <br>
{/each}
{/if}
