# vue-confirm-input

Vue select component

#### Installation

```
npm i -D vue-confirm-input
```


#### Example

```
<confirm-input :text="'Value:'" :title="'Enter text'" :value="value"                
                :callback="text => myMethod(text)"/>
                
```

```
import ConfirmInput from 'vue-confirm-input'
```

```
components: {
    ConfirmInput
}

data: function () {
  return {
    value: 'Default text'
  }
},

methods: {
  myMethod: function (text) {
    console.log(text)
    this.value = text
  }
}
```
