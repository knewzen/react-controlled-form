# Form Validation

Validation is an important part for most forms. There are two ways to perform validation.

* [Global Validation](#global-validation)
* [Per-Field Validation](#per-field-validation)
  * [On User Input](#on-user-input)
  * [On Blur](#on-blur)
  * [On Submit](#on-submit)

## Global Validation
We are able to validate the whole form by passing a [`validate`](../api/Form.md#props) function to our wrapping [`<Form>`](../api/Form.md) component.<br>
This is primarily useful for special cross-validation cases where multiple field values are validated together.


#### Example
```javascript
import { Form } from 'react-controlled-form'

// e.g. if we do not ship to Berlin, Germany
function validate({ data }) {
  return data.country.value !== 'Germany' ||
    data.city.value !== 'Berlin'
}

export default () => (
  <Form formId="address" validate={validate}>
    /* form fields */
  </Form>
)
```

## Per-Field Validation
Although we are already able to validate the whole form, it is often much simpler and more intuitive to allocate validation with each field.<br>

### On User Input
The most common way is to validate on user input.

#### Example
```javascript
import { asField } from 'react-controlled-form'

// only allow real positive numbers
function validate(value) {
  return value.match(/^\d+$/) !== null && parseInt(value) > 0
}

const Age = ({ updateField, value, isValid }) => {
  function onInput(e) {
    const nextValue = e.target.value

    updateField({
      isValid: validate(nextValue),
      nextValue
    })
  }

  return (
    <input
      value={value}
      onInput={onInput}
      style={{
        // simple way to indicate field validation
        color: isValid ? 'green' : 'red'
      }}
    />
  )
}

export default asField(Age)
```

---

### On Blur
Another very common way is to trigger validation as soon as the user leaves the input field. This can be done using the built-in HTML `onBlur` event.
We will utilize the `isTouched` indicator to only show visible validation on blur. As react-controlled-form automatically sets `isTouched` to `true` as on `updateField`, we have to force the opposite.

> In addition, you could also use the `onFocus`-event to set `isTouched` to `false` again.

#### Example
```javascript
import { asField } from 'react-controlled-form'

function validate(value) {
  return value.match(/^\d+$/) !== null && parseInt(value) > 0
}

const Age = ({ updateField, value, isValid, isTouched }) => {
  function onInput(e) {
    const nextValue = e.target.value

    updateField({
      isValid: validate(nextValue),
      // forcing isTouched: false
      isTouched: false,
      value: nextValue
    })
  }

  function onBlur() {
    updateField({
      isTouched: true
    })
  }

  return (
    <input
      value={value}
      onBlur={onBlur}
      onInput={onInput}
      style={{
        // if its touched show the validation color
        // if not just remain black
        color: isTouched ? (isValid ? 'green' : 'red') : 'black'
      }}
    />
  )
}

export default asField(Age)
```

---
### On Submit
The final common validation pattern probably is on Form submit.

#### Example
> Coming soon.
