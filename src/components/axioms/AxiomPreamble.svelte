<script type="ts">
  import Typewriter from 'svelte-typewriter';
  export let onFinish;
</script>

<Typewriter cascade on:done={onFinish}>
  <p class="text-center pb-4"> Thank you all for agreeing to participate in this exciting new project. </p>

  <p class="text-center pb-4"> From all of us at <span class="text-delta-green">Green Hook Mysteries</span>, please keep safe and report back often.</p>
  
  <ul class="flex flex-col justify-center items-center pb-4">
    <p class="text-delta-green">Here are the events of the story you were working on:</p>

    <li> 6th March 2017 - Charles Dexter Ward disappears from a secure psychiatric hospital just outside Providence, Rhode Island.</li>
    <li> 31st May 2018 - Dr Jonathon Willett murders Lucy Hawthorne in her house in 20 Sheldon Avenue, Highgate, London </li>

  </ul>

  <p class="text-center"> Here are the notes you've shared with me so far: </p>

</Typewriter>
