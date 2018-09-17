# easy-embeddable-polls

![Image of easy-embeddable-polls demo](https://i.imgur.com/0kh5gvt.png)

## Overview

`easy-embeddable-polls` provides you with Vue components for easily embedding polls into your website. Two components are offered out of the box:

- A basic `Poll` configurable by use of props. Outputs simple markup with semantic class names.
- A `RenderlessPoll` which follows the [renderless component pattern](https://adamwathan.me/renderless-components-in-vuejs/).
This option gives you complete control over the markup of the poll while still allowing `easy-embeddable-polls` to handle the business logic for you.

## Install

#### NPM

First, install `easy-embeddable-polls` via your preferred package manager:

```bash
$ npm install easy-embeddable-polls --save
```

Then register any relevant components in your JavaScript:

```js
import Vue from 'vue';
import { Poll, RenderlessPoll } from 'easy-embeddable-polls';

Vue.component('poll', Poll);
Vue.component('renderless-poll', RenderlessPoll);
```

Now you can use the components in your markup:

```html
<poll :choices="['Skittles', 'Starburst', 'Nerds']"></poll>
```

#### CDN

Simply include `vue` & `easy-embeddable-polls` - we recommend using [unpkg](https://unpkg.com/#/).

```html
<script src="https://unpkg.com/vue@latest"></script>
<script src="https://unpkg.com/easy-embeddable-polls@latest"></script>
```

> Note: You can point to a specific version of either package by replacing `@latest` with a specific version number, e.g. `easy-embeddable-polls@0.2.1`.

Then register any relevant components in your JavaScript:

```js
Vue.component('poll', easyEmbeddablePolls.Poll);
Vue.component('renderless-poll', easyEmbeddablePolls.RenderlessPoll);
```

Now you can use the components in your markup:

```html
<poll :choices="['Skittles', 'Starburst', 'Nerds']"></poll>
```

Here's an [example on JSBin](https://jsbin.com/zohebew/edit?html,css,js,output).

## Usage

### `Poll` Component

The basic `Poll` component offers a handful of props that allow you to customize your poll. These props are outlined below:

#### Props

| Name | Type | Default value | Description |
| :--- | :--- | :--- | :--- |
| allowCustomAnswer | Boolean | false | Gives users the option to enter a custom answer via a text field. |
| buttonText | String | "Submit Answer" | Text that will appear in the submit button. |
| choices | Array | `[]` | An array of answers users can choose in your poll. |
| customAnswerLabel | String | "Other" | The label that will appear for the custom answer option. |
| endpoint | String | undefined | A URL where your poll answers will be submitted to. Alternatively, if your form is submitting to a [FieldGoal](https://fieldgoal.io) form, the `fieldGoalFormKey` prop can be used. |
| errorMessage | String | "There was an error submitting your answer." | A message that will be displayed if there is an error submitting your poll. Supports HTML. |
| fieldGoalFormKey | String | undefined| Form key for a [FieldGoal](https://fieldgoal.io) form. If used, the `endpoint` prop will be overwritten with a FieldGoal URL. |
| multipleChoice | Boolean | false | Determines whether or not a user can choose choose multiple answers. |
| requestConfig | Object |  `{}`  | An [axios](https://github.com/axios/axios) configuration object that will be used on your poll submission request. |
| submitErrorHook | Function | Empty function | A callback that is run when an error is encountered after a poll is submitted. Receives an error object as a parameter. |
| submitSuccessHook | Function | Empty function | A callback that is run after your poll has been successfully submitted. Receives a response object as a parameter. |
| thankYouMessage | String | "Your answer has been submitted." | A message that will be displayed after a user submits your poll. Supports HTML. |
| title | String | undefined | A title that will appear at the top of your poll. |

#### Styling

The `Poll` component uses semantic CSS class names to give you control over the look and feel of your poll. You can see each class and how they are used in [our JSBin demo](https://jsbin.com/zohebew/edit?html,css,js,output).

### `RenderlessPoll` Component

A `RenderlessPoll` component is offered in addition to the `Poll` component for situations where you need to heavily customize the outputted markup of the poll.
The `RenderlessPoll` component follows the [renderless component pattern](https://adamwathan.me/renderless-components-in-vuejs/).
We will not dive into the concept of renderless components in this documentation, instead recommending you read the previously linked article to familiarize yourself.

Below is an example of an implementation of the `RenderlessPoll` component, including all slot props. It should be noted as well that the [`Poll` component](https://github.com/tightenco/easy-embeddable-polls/blob/master/src/components/Poll.vue) itself is an implementation of the `RenderlessPoll` component.

```js
<renderless-poll endpoint="http://www.example.com" :choices="['Banana Bread', 'Wheat Bread', 'Sourdough Bread']">
  <div slot-scope="{ allowCustomAnswer, buttonEvents, buttonText, choices, choiceAttrs, choiceEvents, customAnswerChoiceAttrs, customAnswerChoiceEvents, customAnswerChoiceSelected, customAnswerInputAttrs, customAnswerInputEvents, customAnswerLabel, error, errorMessage, inputType, submitted, thankYouMessage, title }">
    <div v-if="error">
      {{ errorMessage }}
    </div>
    <div v-else-if="submitted">
      {{ thankYouMessage }}
    </div>
    <div v-else>
      <span v-if="title">
        {{ title }}
      </span>
      <div
        v-for="choice in choices"
        :key="choice"
      >
        <input
          :id="choice"
          :value="choice"
          v-bind="choiceAttrs"
          v-on="choiceEvents"
        />
        <label :for="choice">{{ choice }}</label>
      </div>
      <div v-if="allowCustomAnswer">
        <input
          id="custom_answer_choice"
          v-bind="customAnswerChoiceAttrs"
          v-on="customAnswerChoiceEvents"
        />
        <label for="custom_answer_choice">{{ customAnswerLabel }}</label>
        <input
          type="text"
          v-if="customAnswerChoiceSelected"
          v-bind="customAnswerInputAttrs"
          v-on="customAnswerInputEvents"
        />
      </div>
      <button
        type="submit"
        v-on="buttonEvents"
      >
        {{ buttonText }}
      </button>
    </div>
  </div>
</renderless-poll>
```

### Request Payload

Both components work by sending a payload with a single `answer` key to your specified endpoint.
The value is either a string (if the user chose a single answer) or an array (if the user chose multiple answers).
Below are some examples:

```
{
  answer: 'Starburst'
}
```

```
{
  answer: [
    'Skittles',
    'Starburst',
  ]
}
```

## License

[MIT](https://github.com/tightenco/easy-embeddable-polls/blob/master/LICENSE.md)

