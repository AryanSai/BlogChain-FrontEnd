<script>
    import { create } from 'ipfs-http-client';
     async function upload(){
         const client = create({url:"http://127.0.0.1:5001/api/v0"})
         const cid = await client.add('{"Title":"Trial post","Text":"adasdsaaadfdfda"}')
         return cid
     }

 function sairam(){
     alert('sairam')
     upload().then((data)=>{
        const cid = data.path
         alert(cid)
     });
    
}
</script>

<label for="myfile">Select a file:</label>
<input type="file" id="myfile" name="myfile"> 

<button on:click={sairam}>upload</button>
