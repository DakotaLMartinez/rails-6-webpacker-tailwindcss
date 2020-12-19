# Setting up TailwindCSS on Rails 6

To get started with tailwindcss in Rails 6, you'll want to run 

You'll need to make sure webpacker is installed and ready to go by running

```bash
rails webpacker:install
```

Of course, this is not going to work if you haven't already installed yarn. So if you haven't, you should [install yarn now](https://classic.yarnpkg.com/en/docs/install/).

Next, we want to add tailwindcss.

```bash
yarn add tailwindcss
```

In `app/javascript/css/application.scss` add the following:

```scss
// app/javascript/css/application.scss
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
```

Add this to `app/javascript/packs/application.js`

```js
// app/javascript/packs/application.js
// ...
require("css/application.scss")
```

We'll be using tailwind as a postCSS plugin, so let's look at the file called `postcss.config.js` in the root of our project and add a couple of requires to it for tailwindcss and autoprefixer.

```js
module.exports = {
  plugins: [
    require('postcss-import'),
    require('postcss-flexbugs-fixes'),
    require('tailwindcss')('./app/javascript/css/tailwind.js'),
    require('autoprefixer'),
    require('postcss-preset-env')({
      autoprefixer: {
        flexbox: 'no-2009'
      },
      stage: 3
    })
  ]
}
```

Now that we've done this, we need to actually create the `tailwind.js` file to allow configuration of tailwind:

```js
//app/javascript/css/tailwind.js
module.exports = {
  theme: {
    screens: {
      sm: '640px',
      md: '768px',
      lg: '1024px',
      xl: '1280px',
    },
    extend: {
      spacing: {
        '96': '24rem',
        '128': '32rem',
      }
    },
  },
  variants: {},
  plugins: [],
}

```

Now, let's check out our application layout and add make sure the these tags are there.

```html.erb
<%= stylesheet_pack_tag 'stylesheets', media: 'all', 'data-turbolinks-track': 'reload' %>
<%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
```


Resources

- [Rails 6 and Tailwind CSS](https://medium.com/@davidteren/rails-6-and-tailwindcss-getting-started-42ba59e45393)
- [How to Install Taildwind on Rails 6](https://dev.to/tcgumus/how-to-install-tailwind-css-on-rails-6-0-2h3f)
- [Tailwind CSS docs](https://tailwindcss.com/docs)