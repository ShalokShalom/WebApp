### Colors

A button can have different colors

```
<div>
  <p>
    <hc-button>Default Button</hc-button>
    <hc-button color="primary">Primary Button</hc-button>
    <hc-button color="dark">Dark Button</hc-button>
    <hc-button color="white">White Button</hc-button>
  </p>
  <p>
    <hc-button color="success">Success Button</hc-button>
    <hc-button color="info">Info Button</hc-button>
    <hc-button color="danger">Danger Button</hc-button>
    <hc-button color="warning">Warning Button</hc-button>
  </p>
</div>
```

### Circle

A button can be circular

```
<div>
  <hc-button color="primary" size="xsmall" circle>
    <hc-icon icon="plus" />
  </hc-button>
  <hc-button color="primary" size="small" circle>
    <hc-icon icon="plus" />
  </hc-button>
  <hc-button color="primary" circle>
    <hc-icon icon="plus" />
  </hc-button>
  <hc-button color="primary" size="medium" circle>
    <hc-icon icon="plus" />
  </hc-button>
  <hc-button color="primary" size="large" circle>
    <hc-icon icon="plus" />
  </hc-button>
</div>
```

### Loading

A button can appear in loading state

```
<hc-button color="primary" :loading="true">Loading Button</hc-button>
```

### Types

A button can render with different types: button (default), nuxt, link

```
<p>
  <hc-button type="nuxt" color="primary" to="/">Nuxt Link</hc-button>
  <hc-button type="link" color="primary">Normal Link</hc-button>
</p>
```