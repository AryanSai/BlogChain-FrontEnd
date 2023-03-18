<script>
     async function upload(){
       const data =  await fetch('http://127.0.0.1:5001/api/v0/cat?arg=QmZ6NnWBYT3C88Kzenn82Q4RshAfb2Lb23PBzKvVtP9dV6', {
    method: 'POST'});
    const json = await data.json()
    return json;
     }

 function sairam(){
     alert('sairam')
     upload().then((data)=>{
        console.log(data)
     });
    
}
</script>

<label for="myfile">Select a file:</label>
<input type="file" id="myfile" name="myfile"> 

<button on:click={sairam}>upload</button>
