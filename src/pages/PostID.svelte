<script>
  import { pages } from "../store.js";
  import { fade } from "svelte/transition";
  import ReplyList from "../components/ReplyList.svelte";
  import ReplyListItem from "../components/ReplyListItem.svelte";
  let topic;
  const fetchPostbyid = async () => {
    let id = $pages.replace("PostID?id=", "");
    let res = await fetch(`https://csag-pop.herokuapp.com/api/posts/${id}`);
    let data = await res.json();
    if (res.ok) {
      topic = data.topic;
      return data;
    } else {
      let err = await res.text();
      throw new Error(err);
    }
  };
  let onReply = () => {
    promise = fetchPostbyid();
  };
  let promise = fetchPostbyid();
</script>

<style>

</style>

<svelte:head>
  {#if topic}
    <title>Post: {topic}</title>
  {/if}
</svelte:head>

<section>
  <!-- รอโหลดอยู่นิ -->
  {# promise}
    <p>Fetching posts . . .</p>
  {:then post}
    <!-- ละถ้าไม่มีใครโพสกระทู้ล่ะ -->
    {# post}
      <ReplyList
        topic={post.topic}
        name={post.name}
        content={post.content}
        on:ReplySuccess={onReply}>
        {#each post.comment as } <!-- ชื่อไรดี -->
          <ReplyListItem name={.name} reply={.reply} />
        {/each}
      </ReplyList>
    {/}
  {:catch}
    <div>Error</div>
  {/}
</section>
