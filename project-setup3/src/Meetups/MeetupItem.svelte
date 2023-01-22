<script>
    export let meetup;
    export let {title, subTitle, imageUrl, description, address, contactEmail} = meetup;
</script>

<article>
    <header>
        <h1> {meetup.title}</h1>
        <h2> {meetup.subTitle}</h2>
        <p>{address}</p>
    </header>
     <div class="image">
        <img src="{imageUrl}" alt="{title}">
     </div>
     <div class="content">
        <p>{description}</p>
     </div>

     <footer> 
        <a href="mailTo:{contactEmail}">Contact</a>
        <button>Show Details</button>
        <button>Favorite</button>
     </footer>
</article>