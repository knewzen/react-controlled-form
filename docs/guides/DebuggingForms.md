# Debugging Forms

A common requirement is to seamlessly debug forms, the form data and everything at runtime with ease. Despite the basic practice of using the browser devTools, there are some neat tricks to do that:

## Redux devTools
Right, since the form data and state is fully handled by Redux under the hood, you can choose any Redux devTool to debug your form data and even use features like time traveling.<br>
Another helpful pattern is to provide the Redux store as a global window property by adding `window.store = store` just after your store setup. *(check the [examples](../introduction/Examples.md) on how this works)*.

## Displaying formatted JSON

If you've already checked one of the [examples](../introduction/Examples.md), you may have seen the simple yet helpful JSON representation of the form data. It shows the whole form data at any point of time and updates automatically as properties change. It is rendered using a combination of the [`withData`](../api/withData.md)-HOC and `JSON.stringify` with its third parameter.

#### Example
> It must be rendered inside the `<Form>`-component to show any effect.

```javascript
import React from 'react'
import { withData } from 'react-controlled-form'

const DataDisplay = ({ data }) => (
  <pre>{JSON.stringify(data, null, 2)}</pre>
)

export default withData()(DataDisplay)
```
