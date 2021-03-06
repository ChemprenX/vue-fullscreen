## Browser support
[Full Screen API](http://caniuse.com/fullscreen)

## Installation 
Install from GitHub via NPM
```bash
npm install vue-fullscreen
```
## Usage

To use `vue-fullscreen`, simply import it, and call `Vue.use()` to install.

```html
<template>
  <div id="app">
    <fullscreen :fullscreen.sync="fullscreen">
      Content
    </fullscreen>
    <button type="button" @click="toggle" >Fullscreen</button>
  </div>
</template>
<script>
  import fullscreen from 'vue-fullscreen'
  import Vue from 'vue'
  Vue.use(fullscreen)
  export default {
    methods: {
      toggle () {
        this.fullscreen = !this.fullscreen
      }
    },
    data() {
      return {
        fullscreen: false
      }
    }
  }
</script>
```

**Caution:** Because of the browser security function, you can only call these methods by a user gesture. (*e.g.* a click callback)


## Use as plugin
In your vue component, You can use `this.$fullscreen` to get the instance.

```html
<template>
  <div id="app">
    <div class="example">
      Content
    </div>
    <button type="button" @click="toggle" >Fullscreen</button>
  </div>
</template>
<script>
import fullscreen from 'vue-fullscreen'
import Vue from 'vue'
Vue.use(fullscreen)
export default {
  methods: {
    toggle () {
      this.$fullscreen.toggle(this.$el.querySelector('.example'), {
        wrap: false,
        callback: this.fullscreenChange
      })
    },
    fullscreenChange (fullscreen) {
      this.fullscreen = fullscreen
    }
  },
  data() {
    return {
      fullscreen: false
    }
  }
}
</script>
```



### Methods

#### toggle([target, options, force])

Toggle the fullscreen mode.

- **target**:
  - Type: `Element`
  - Default: `document.body`
  - The element target for fullscreen.
- **options** (optional):
  - Type: `Object`
  - The fullscreen options.
- **force** (optional):
  - Type: `Boolean`
  - Default: `undefined`
  - pass `true` to  force enter , `false` to exit fullscreen mode.



#### enter([target, options])

enter the fullscreen mode.

- **target**:
  - Type: `Element`
  - Default: `document.body`
  - The element target for fullscreen.
- **options** (optional):
  - Type: `Object`
  - The fullscreen options.



#### exit()

exit the fullscreen mode.


#### getState()

get the fullscreen state.

- Type: `Boolean`

**Caution:** The action is asynchronous, you can not get the expected state immediately following the calling method.     


### Options

### callback

- Type: `Function`
- Default: `null`

It will be called when the fullscreen mode changed.

### fullscreenClass

- Type: `String`
- Default: `fullscreen`

The class will be added to target element when fullscreen mode is on.

### wrap

- Type: `Boolean`
- Default: `true`

If `true`, the target element will be wrapped up in a background `div`, and you can set the background color.

### background

- Type: `String`
- Default: `#333`

The background style of wrapper, only available when fullscreen mode is on and `wrap` is true.






## Use as component

 You can simply import the component and register it locally.

```html
<template>
  <div id="app">
    <fullscreen ref="fullscreen" :fullscreen.sync="fullscreen">
      Content
    </fullscreen>
    <button type="button" @click="toggle" >Fullscreen</button>
  </div>
</template>
<script>
  import Fullscreen from "vue-fullscreen/src/component.vue"
  export default {
    components: {Fullscreen},
    methods: {
      toggle () {
        this.$refs['fullscreen'].toggle()
        // this.fullscreen = !this.fullscreen
      }
    },
    data() {
      return {
        fullscreen: false
      }
    }
  }
</script>
```



### Methods

#### toggle([force])

Toggle the fullscreen mode.You can pass `force` to force enter or exit fullscreen mode.

- **force** (optional):
  - Type: `Boolean`
  - Default: `undefined`
  - pass `true` to  force enter , `false` to exit fullscreen mode.

#### enter()

enter the fullscreen mode.

#### exit()

exit the fullscreen mode.

#### getState()

get the fullscreen state.

- Type: `Boolean`

**Caution:** The action is asynchronous, you can not get the expected state immediately following the calling method.




### Props

#### fullscreen

- Type: `Boolean`
- Default: `false`

Use `.sync` to synchronize the parent's value. You can change it to toggle fullscreen mode too.

**Caution:** Changing it may not work in Firefox and IE11, it may be that they handle async operation specially.

But in Firefox you can try to add `babel-polyfill` to the `vendor` in `webpack` like this:
```javascript
module.exports = {
  entry: {
    app: './example/main.js',
    vendor: ['babel-polyfill', 'vue']
  },
  ...
}
```
Then it works, though I don't know why. ╮(╯▽╰)╭

I have no idea how to fix it in IE11 yet.


#### fullscreenClass

- Type: `String`
- Default: `fullscreen`

The class will be added to the component when fullscreen mode is on.

#### background

- Type: `String`
- Default: `#333`

The background style of component, only available when fullscreen mode is on.

### Events

#### change

- **isFullscreen**:  The current fullscreen state.

This event fires when the fullscreen mode changed.

## No conflict

If you need to avoid name conflict, you can import it like this:

```html
<template>
  <div id="app">
    <fs ref="fullscreen" :fullscreen.sync="fullscreen">
      Content
    </fs>
    <div class="example">
      Content
    </div>
    <button type="button" @click="toggle" >Fullscreen</button>
  </div>
</template>
<script>
import Fullscreen from 'vue-fullscreen'
import Vue from 'vue'
Vue.use(Fullscreen, {name: 'fs'})
export default {
  methods: {
    toggle () {
      this.$refs['fullscreen'].toggle()
      this.$fs.toggle(this.$el.querySelector('.example'), {
        wrap: false,
        callback: this.fullscreenChange
      })
    },
    fullscreenChange (fullscreen) {
      this.fullscreen = fullscreen
    }
  },
  data() {
    return {
      fullscreen: false
    }
  }
}
</script>
```
