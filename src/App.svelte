<script lang="ts">
  import axios from 'axios';
 const API_KEY = "3061a017-f56f-4957-805b-f054c8102223"; // API KEY HERE
    const SD1XmodelsURL = "https://api.prodia.com/v1/sd/generate"
  let prompt = 'a usa girl'
  let image_url=''
  let costVnd = 0
    let jobId=''
    let verifyJobUrl=""
  async function GetImage(){
    image_url=''
    costVnd = 0
        const body={
  "model": "v1-5-pruned-emaonly.safetensors [d7049739]",
  "prompt": "puppies in a cloud, 4k",
  "negative_prompt": "badly drawn",
  "steps": 20,
  "cfg_scale": 7,
  "seed": -1,
  "upscale": false,
  "sampler": "DPM++ 2M Karras"
 }
    axios.post(SD1XmodelsURL, {body}
           )
  .then(function (response) {
    console.log(response.data);
    jobId=response.data.job
    console.log("jobid " + jobId)
    verifyJobUrl = "https://api.prodia.com/v1/job/" + jobId
 console.log(verifyJobUrl);
    axios.get(verifyJobUrl,{
    headers: {
        'X-Prodia-Key': API_KEY,
        'Accept': 'application/json',
        'Content-Type': 'application/json'
    }
 })
 .then((res) => {
    console.log(res.data.status)
    image_url= res.data.imageUrl
 
 })
 .catch((err) => console.error(err));
 
 
  })
  .catch(function (error) {
    console.log(error);
  });
 }
 
 </script>
 <h1>sdxl</h1>
 {#if costVnd != 0}
 <h1>Cost VND = {costVnd}</h1>
 {/if}
 <h1>stable-diffusion</h1>
 <input type="text" bind:value={prompt}>
 <button on:click={GetImage}>Send to get Image</button>
 
 {#if image_url != ''}
 <h1>{image_url}</h1>
 <img src={image_url} alt="theheai" />
 {/if}