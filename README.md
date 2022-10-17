
# react-native-reactions
[![npm version](https://img.shields.io/badge/npm%20package-0.0.1-orange)](https://www.npmjs.org/package/react-native-country-code-select) [![Android](https://img.shields.io/badge/Platform-Android-green?logo=android)](https://www.android.com) [![iOS](https://img.shields.io/badge/Platform-iOS-green?logo=apple)](https://developer.apple.com/ios) [![MIT](https://img.shields.io/badge/License-MIT-green)](https://opensource.org/licenses/MIT)

---
This is a pure javascript and react-native-reanimated based library that provides two types of reactions including default and modal.

This library provides emoji reactions features like Instagram/WhatsApp or other social media, It is simple to use and fully customizable. It works on both android and iOS platforms.

---
## 🎬 Preview

---

| Default                                          | Modal                                        |
| ----------------------------------------------------- | -------------------------------------------------- |
| ![alt Default](./assets/absolute.gif) | ![alt Modal](./assets/modal.gif) |

---

## Quick Access

[Installation](#installation) | [Reactions](#reactions) | [Properties](#properties) | [Example](#example) | [License](#license) | 

# Installation

##### 1. Install library and react-native-reanimated

```bash
$ npm install react-native-reactions react-native-reanimated
# --- or ---
$ yarn add react-native-reactions react-native-reanimated
```

##### 2. Install cocoapods in the ios project

```bash
cd ios && pod install
```

> Note: Make sure to add Reanimated's babel plugin to your `babel.config.js`

```
module.exports = {
      ...
      plugins: [
          ...
          'react-native-reanimated/plugin',
      ],
  };
```

##### Know more about [react-native-reanimated](https://www.npmjs.com/package/react-native-reanimated)
---

# Reactions
- Reactions has two different types, default one and modal
- To avoid the zIndex/Overlap issue, you can use modal instead of the default.

#### 🎬 Preview
![Default Reaction](./assets/Reaction.png)
---
### Card Emoji Format

```
 const CardEmojiList = [
    {
        id: 0, emoji: '😇', title: 'like'
    },
    {
        id: 1, emoji: '🥰', title: 'love'
    },
    {
        id: 2, emoji: '🤗', title: 'care'
    },
    {
        id: 3, emoji: '😘', title: 'kiss'
    },
    {
        id: 4, emoji: '😂', title: 'laught'
    },
    {
        id: 5, emoji: '😎', title: 'cool'
    },
];

```
## Default Reactions
---
#### 🎬 Preview
![Default Absolute](./assets/absolute.gif)
---

#### Usage

---

##### App
```jsx

import React from 'react';
import { FlatList, SafeAreaView, StyleSheet } from 'react-native';
import { Card } from './component';

const PostItemList = [
    {
      id: 'bd7acbea-c1b1-46c2-aed5-3ad53abb28ba',
      title: 'First Item',
      image:'https://thumbs.dreamstime.com/b/free-shipment-sale-happy-little-kid-cardboard-box-cute-child-toddler-clients-receiving-carton-package-post-mail-parcel-157640927.jpg',
    },
    {
      id: '3ac68afc-c605-48d3-a4f8-fbd91aa97f63',
      title: 'Second Item',
      image:'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR1r0Aler0AJzpuvvL5i2bGK6tBp39fN3HKoQ&usqp=CAU',
    },
    {
      id: '58694a0f-3da1-471f-bd96-145571e29d72',
      title: 'Third Item',
      image:'https://i.pinimg.com/564x/48/db/a8/48dba86fb51376627e1e53b48c377bdc.jpg',
    },
  ];

const App = () => (
  <SafeAreaView style={styles.mainStyle}>
      <FlatList
        data={PostItemList}
        style={{ backgroundColor: '#c9cdd0' }}
        renderItem={({ index,item }) => <Card index={index} {...item} />}
        keyExtractor={item => item?.id}
      />
  </SafeAreaView>
);

export default App;

const styles = StyleSheet.create({
  mainStyle: {
    flex: 1,
  },
});
```
##### Card
```jsx

import { Image, StyleSheet, Text, View } from 'react-native'
import React, { useState } from 'react'
import { Reaction } from 'react-native-reactions'

interface EmojiItemProp {
  id: number;
  emoji: React.ReactNode | string | number;
  title: string;
}

interface CardProps extends CardItemsProps {
  index?: number;
  selectedEmoji?: EmojiItemProp
  setSelectedEmoji?: (e: EmojiItemProp | undefined) => void;
}
interface CardItemsProps {
  id?: string;
  image?: string;
  title?: string;
}

const Card = ({ index, ...item }: CardProps) => {
    const [selectedEmoji, setSelectedEmoji] = useState<EmojiItemProp>();

    return (
        <View style={styles.cardContainer}>
            <View style={styles.postImageContainer}>
                <Image
                    source={{ uri: item?.image }}
                    style={styles.postImage}
                />
            </View>
            <View style={styles.line} />
            <View style={styles.bottomContainer} >
                {/* Above we provied CardEmojiList */}
                <Reaction items={CardEmojiList} onTap={setSelectedEmoji}>
                    <Text>{selectedEmoji ? selectedEmoji?.emoji : 'Like'}</Text>
                </Reaction>
                <Text>Comment</Text>
                <Reaction items={CardEmojiList} onTap={setSelectedEmoji}>
                    <Text>Share</Text>
                </Reaction>
            </View>
        </View>
    )
}

export default Card

const styles = StyleSheet.create({
    cardContainer: {
        marginVertical: 5,
        backgroundColor: '#FFFFFF',
    },
    postImageContainer: {
        alignItems: 'center',
        zIndex: -1
    },
    postImage: {
        width: '100%',
        height: 200,
        zIndex: -1
    },
    line: {
        borderWidth: 0.3,
        borderColor: '#c9cdd0',
    },
    bottomContainer: {
        flexDirection: 'row',
        justifyContent: 'space-between',
        margin: 10,
        marginHorizontal: 20
    },

})

```

---


## Modal Reactions
- Basically, modal reactions use to avoid the zIndex issue on reactions.

> Note: Make sure to wrap your root component with ReactionProvider

```bash
import { ReactionProvider } from 'react-native-reactions';
export default const App = () => {
  return <ReactionProvider>{/* content */}</ReactionProvider>;
}
```

#### 🎬 Preview
![Default Modal](./assets/Modal.gif)
---

#### Usage

---
##### App.tsx
 Use the above [App](#app) example but the only change is to wrap the root component with ReactionProvider.
```jsx
import { ReactionProvider } from 'react-native-reactions';

 <ReactionProvider>
    <SafeAreaView style={styles.mainStyle}>
      <FlatList
        data={PostItemList}
        style={{ backgroundColor: '#c9cdd0' }}
        renderItem={({ index, item }) => <Card index={index} {...item} />}
        keyExtractor={item => item?.id}
      />
    </SafeAreaView>
  </ReactionProvider>
```

##### Card.tsx
 Use the above [Card](#card) example but the only change is to set type as modal.
```jsx
  <Reaction type='modal' items={CardEmojiList} onTap={setSelectedEmoji}>
    <Text>{selectedEmoji ? selectedEmoji?.emoji : 'Like'}</Text>
  </Reaction>
```
# Properties
---
## CardEmojiList

```
[
    {
        id:string,
        emoji: element | string | url,
        title: string
    }
]

```

| Prop              | Default                        | Type     | Description  |
| :---------------- | :----------------------------- | :------- | :----------- | 
|type               | default                        | string   | Different type of component like default and modal |
|items              | [CardEmojiList](#cardemojilist)| array    | Array of emoji reactions |
|disable            | false                          | boolean  | If true, disable all interactions for this component  |
|variant            | default                        | string   | Variants for touch like default, onPress and onLongPress         |
|onPress            | -                              | function | Called when the touch is released  |
|onLongPress        | -                              | function | Called when the long touch is released  |
|onTap              | -                              | function | Select onTap callback function that returns the selected emoji |
|cardStyle          | {}                             |ViewStyle | Card modal style|
|emojiStyle         | {}                             |TextStyle | Emoji style |
|emojiKey           | -                              |string    | EmojiKey will be the key name of array’s emoji field which is assign to items  |
|onShowDismissCard  | -                              |function  | return true or false when emojicard is open|
|isShowCardInCenter | false                          |boolean   | If true, Show card in center|
|iconSize           | 25                             |number    |Size of emoji. It should be in between 15 to 30.|
|titleStyle         | {}                             |TextStyle |Title style for emoji|
|titleBoxStyle      | {}                             |ViewStyle |Title box style|
|emojiContainerStyle| {}                             |ViewStyle |Emoji container style

---

# Example

A full working example project is here [Example](./example/src/App.tsx)

```sh
$ yarn
$ yarn example ios   // For ios
$ yarn example android   // For Android
```

# TODO

- [ ] Customize Emoji (Add new Emoji)
- [ ] Improve gesture and select emoji in a Single gesture event
- [ ] Landscape support

## Find this library useful? ❤️

Support it by joining [stargazers](https://github.com/SimformSolutionsPvtLtd/react-native-reactions/stargazers) for this repository.⭐

## Bugs / Feature requests / Feedbacks

For bugs, feature requests, and discussion please use [GitHub Issues](https://github.com/SimformSolutionsPvtLtd/react-native-reactions/issues/new?labels=bug&late=BUG_REPORT.md&title=%5BBUG%5D%3A), [GitHub New Feature](https://github.com/SimformSolutionsPvtLtd/react-native-reactions/issues/new?labels=enhancement&late=FEATURE_REQUEST.md&title=%5BFEATURE%5D%3A), [GitHub Feedback](https://github.com/SimformSolutionsPvtLtd/react-native-reactions/issues/new?labels=enhancement&late=FEATURE_REQUEST.md&title=%5BFEEDBACK%5D%3A)

## 🤝 How to Contribute

We'd love to have you improve this library or fix a problem 💪
Check out our [Contributing Guide](CONTRIBUTING.md) for ideas on contributing.

## License

- [MIT License](./LICENSE)




