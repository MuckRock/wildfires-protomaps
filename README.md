# wildfires-protomaps

Let's build a self-hosted version of [this wildfire map](https://felt.com/map/Wildfires-are-becoming-more-frequent-across-the-West-QBFznerjRpK6BWnGzY9BRYC) using Protomaps and Svelte.

Svelte is really just here for UI elements like a legend. This could pretty easily be vanilla javascript.

## Embed

Copy the following snippet into the CMS of your choice:

```html
<iframe
  src="https://muckrock.github.io/wildfires-protomaps/#4/39.82/-98.84"
  width="100%"
  height="450px"
  frameborder="0"
  title="Wildfires: 1980 - 2021"
></iframe>
```

## Fire data

The wilfire data is generated from our [gao-wildfire-exceptions](https://github.com/MuckRock/gao-wildfire-exceptions) repository.
