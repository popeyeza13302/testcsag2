<script>
  let active = false;
  let topic = "";
  let content = "";
  import { user, pages } from "../store.js";
  const handlePostClick = () => {
    if (active && topic && content) {
      fetch("https://csag-pop.herokuapp.com/api/posts/", {
        method: "POST",
        body: JSON.stringify({
          name: $user.username,
          topic,
          content
        }),
        headers: {
          "content-type": "application/json"
        }
      })
        .then(res => res.text())
        .then(id => {
          $pages = `PostID?id=${id}`;
        });
    } else {
      active = true;
    }
  };
  const handleCancleClick = () => {
    active = false;
    topic = "";
    content = "";
  };
</script>

<style>
  textarea {
    resize: none;
  }
</style>

<button class="button" class:button-clear={} on:click={handlePostClick}>
  <!-- อยากให้ button-clear ใช้ตอนไม่ active -->
  Post !
</button>
{#if active}
  <button
    style="float:right"
    class="button-outline"
    on:click={handleCancleClick}>
    cancle
  </button>
  <input type="text" name="Topic" placeholder="Topic" bind:value={topic} />
  <textarea bind:value={content} placeholder="Content" />
{/if}
