# Next.js tips <!-- omit in toc -->

We build Next.js applications with Tailwind CSS as our CSS authoring framework.
Some examples here may contain Tailwind CSS specific tips, but for the sake of
practicality, we will include everything here.

- [Page containers](#page-containers)
- [Custom fonts](#custom-fonts)
- [Blocks](#blocks)
- [`Money.jsx` component](#moneyjsx-component)
- [`LocalDate.jsx` component](#localdatejsx-component)
- [Login](#login)
- [Authenticated pages](#authenticated-pages)
- [Authenticated API routes](#authenticated-api-routes)

# Page containers

In some cases wherein a mock-up contains a specific max-width, you can
instead modify Tailwind's existing `container` class.

In your `tailwind.config.js`

```js
module.exports = {
  theme: {
    container: {
      center: true,
      padding: '2em',
      screens: {
        lg: '1024px',
        xl: '1280px',
      },
    },
  },
  variants: {
    extend: {},
  },
  plugins: [],
}

```

Then in your JSX file:

```jsx
<div className='container'>
  ...
</div>
```

# Custom fonts

If a font name has spaces in it, you will need to wrap it in quotes.


```js
module.exports = {
  theme: {
    extend: {
      fontFamily: {
        sans: ['"Voyage Regular"', '"Playfair Display"'],
      },
    },
  },
  variants: {
    extend: {},
  },
  plugins: [],
}

```

# Blocks

A block is a "row" of element. Most commonly found in landing pages.

Always use `grid` over `flex` when implementing a layout.

- `block`
  - `container`
    - `image`
    - `texts`

```jsx
<div className='bg-white'>
  <div className='container grid grid-cols-2 gap-8'>
    <div>Image</div>
    <div>Texts</div>
  </div>
</div>
```


# `Money.jsx` component

Requires the `warpgate.js` library.

```jsx
<Money currency='BTC' amount={1.00000000} />
```

To customize the look, just modify your `globals.css`. Here's an example:

```css
/* styles/globals.css */

.money {
  @apply space-x-1;
}

.money-currency {
  @apply opacity-50;
}

.money-amount { 
  @apply font-medium;
}
```

# `LocalDate.jsx` component

Use this for representing date time from Warpgate API since we
don't store timezones in our database.

Requires the `warpgate.js` library.

```jsx
<LocalDate date={new Date()} format='MMMM DD, YYYY' />
<LocalDate date={new Date()} format='fromNow' />
```

# Login

This operation will enable the user to be authenticated given the right credentials.
The JWT token is securely stored via http-cookie.

**DO NOT** store JWT in Local Storage because your app will be [vulnerable to XSS attacks](https://medium.com/redteam/stealing-jwts-in-localstorage-via-xss-6048d91378a0).

Requires the `warpgate.js` library.


```js
export default apiResponse(req, async () => {
  const input = _.pick(req.body, ['username', 'password'])
  await login(input)
  return {}
})
```

# Authenticated pages

If you require the user to be authenticated (through HTTP cookies), you need
to wrap your `getServerSideProps` with a wrapper.

```jsx
export const getServerSideProps = requiresUser()
```

Or if you need to retrieve some data before rendering the page:

```jsx
export const getServerSideProps = requiresUser(async (ctx, resource) => {
  const orders = listOrders({
    filter: {
      userId: resource.userId
      status: ctx.req.query.status || 'pending',
    },
  })

  return {
    props: {
      orders
    }
  }
})
```

Note that `getServerSideProps` will be rendered server-side each time the page loads 
which means that the page will be slower than usual to load due to not being cached.

# Authenticated API routes

Requires the `auth.js` library.

If you require an API endpoint to be authenticated:

```js
export default requiresApiUser(async (req, res, resource) => {
  const data = await warpgateQuery(QUERY, { userId })
  res.json(data)
})
```

The `requiresApiUser` wrapper will return two (2) types of error status codes:

- `401` if the user isn't authenticated.
- `400` if the callback throws an error.

No need to wrap your callback in a try/catch statement. if the Warpgate API call throws
an error, it will be passed on to the response json body.
