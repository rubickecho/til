# Next.js Dynamic Routes get params

***Copy Form Next.js Website***
> A Dynamic Segment can be created by warpping a folder's name is square brackets: `[folderName]`.For example, `[id]` or `[slug]`.

# Example

```shell
./
├── _app.js
├── index.js
└── posts
    └── [id].js
```

In `[id].js`, we can use `getStaticProps` get route params.

```js
export async function getStaticProps({ params }) {
  // get params.id
  const id = params.id;

  // eg. fetch data
  const data = fetch(`<API_PATH>/${id}`)
  return {
    props: {
      data
    }
  }
}
```

or use hook `useRouter`

```js
import { useRouter } from 'next/router'

export default function Page() {
    const router = useRouter();
    const id = router.query.id;

    return (
        <div>ID: { id }</div>
    )
}
```