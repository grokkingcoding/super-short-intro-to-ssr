![Image for The Super Short SSR Guide](https://source.unsplash.com/M5tzZtFCOfs)

### About

- THis is a short guide to server-side rendering (SSR). In particular, we focus on how we achieve SSR with NextJS.

- If you would like to follow along with a real NextJs application running locally on your computer, you can git clone thw follow github repo on our github.

The github link is as follows:

- https://github.com/grokkingcoding/giphy-search-nextjs.git

### The Super Short SSR Guide

1. When considering whether you need SSR, you need to ask yourself a few questions.

   - Does the data I need on my webpage really chnage that often? If your answer is no, then you are better off not
     using SSR and using statically generated pages instead.

   - Is it critical for your application to use the most up-to-date data? Yes, then use SSR.

2. What is the difference between SSR and statically generated pages?

   - The key difference lies in how these pages are being built and served.

   - Statically generated pages are made during build time so this only happens once. Then the server will always use this version of the built pages to serve users on the web. It does not change with every request.

   - On the other hand, SSR will generate new pages (the HTML and the new data) with new data each time a request is made by the user.

3. To use SSR in NextJS is very simple. Instead of getting data with getStaticProps you use getServerSideProps to retrieve the data.

4. To do SSR with api data from another server, you can do something like this:

```

export async function getServerSideProps() {
let apiData = await fetch('https://mydomain/my-awesome-api')
apiData = await apiData.json()
return {props: {apiData: apiData}}
}

```

5. Below is a short code snippet of how you might use server side rendering to get data based on different route paths on your website.

- Here you see we used "context" to get the data from our route path. So the value "context.query.searchTerm" gets the variable in your route that you have specidified in your pages section.

```

export async function getServerSideProps(context) {

    const searchTerm = context.query.userName

```

- Here you pass the userName variable into your api url.

```

      let userData = await fetch(`https://myapi/v1/search?q=${userName}`)

      userData = await userData.json()

      return {
          props: {profile: userData.data}
          }
        }
```

6. So what happens after the data is fetched?

- To pass the data from getServerSideProps we pass initalData into our Profile component like this, you see right after the name of the component like this - export default function Profile (initalData).

- Note: remember to pass in a key for a list of items you are rendering with the array.map function. It is a good idea to use the index of each item in the array as this is always unique.

```

export default function Profile (initalData){

const router = useRouter()

return(
<>

<Head>
<title>Profile</title>
<link rel="icon" href="/favicon.ico" />
<link rel="stylesheet" href="/styles.css"/>
</Head>
    <h1>Welcome back: {router.query.userName}!</h1>

 <div className="giphy-search-results-grid">
        {
    initialData.profile.map((each, index) => {
        return(

        <div key={index}>
            <h3>{each.userName}</h3>
            <img src={each.userImageUrl} alt={each.imgTitle}/>
        </div>

        )
        })}
        </div>

</>)
}
```

- Why do we use initialData.profile.map? It is because when we fetched data before using getServerSideProps we structured the initialData like this.

```
      return {
          props: {profile: userData.data}
          }
        }
```

- Put it simple, initialData = props (which contains the data object) so when we do initialData.profile.map we are actually doing this:

props.profile.userData.data.map
