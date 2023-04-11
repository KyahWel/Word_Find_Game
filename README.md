
# WordFind

Word Find is a classic eLearning game that asks learners to recall keywords and search for them in a grid. For instance, You can include definitions or mention related keywords to give learners clues about the word/s they should be looking for. If you’re introducing new concepts or terms to them, this is the ideal game to include after a lesson.

## Build Setup


```bash

# install dependencies
$ npm  install

# serve with hot reload at localhost:3000
$ npm  run  dev

# build for production and launch server

$ npm  run  build
$ npm  run  start

```

# Main Game Component

You must call the MainGame component in the index.vue with the list of words and timer length as its props.

```HTML

<MainGame  v-if="playInit" :words="words" :time="time" />

```

You can initialize the props value in the script section in the index.vue

```Javascript
export  default {

data() {
	return {
		time: 180, //in seconds
		words: ['test', 'banana', 'cherry', 'date', 'fig', 'grape', 'honeydew', 'kiwi', 'lemon'] // words to search
	}
}
```
With that, you can now use the MainGame component and play the Word Find game. Enjoy!!

# ©  Copyright 
* **William Cris Hod** , Technological University of the Philippines Manila (williamcris.hod02@gmail.com)
* **Franco De Guzman** , Technological University of the Philippines Manila (deguzmanfranco06@gmail.com)
* **Austin Charles Burog** , Technological University of the Philippines Manila (austinburog017@gmail.com)

