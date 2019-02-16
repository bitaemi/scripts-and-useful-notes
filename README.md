# Use Jade or Pug

npm i jade // to use later to transpile jade into html or js
jade -P jadetemplate.jade //transpile jade into html
jade --client --no-debug jadetemplate.jade //transpile jade into js

- use the | to mix more tags

// -- I'm a comm that dont shows in html
//  I'm a comm and I'll show in html

- varMe = 'I am a var'
p I am a paragraph containing the #{varMe}

...

# code-academy-notes
Code Academy 2017 lectures notes
