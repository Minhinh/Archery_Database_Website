# COS20031 archery-application

Archery Score Recording
Main Use Case
The most important use case for this application is the entry of a single score. Firstly, the round has to be chosen from a list and the archer who is scoring. This could look like:
Here, the round is chosen first from a list, then the archer is added. The default equipment is shown, which can be changed. Changing the default category is not necessary here.
After this setup, the archer is prompted to type in the correct number of ends. This could look like: where the listing requests the first end of five. The range is 50m on a 122cm face. The button with the pen on it could be any other control that opens an interface which lets a user type a score, such as this: The interface does not have to have a coloured keypad as in the picture. In fact, the existing keyboard or keypad of the device could be used. But the application should enforce the entry of the scores in the order of magnitude, large scores first:
X – 10 – 9 – 8 – 7 – 6 – 5 – 4 – 3 – 2 – 1 – M
where X counts as 10 and M (miss) counts as 0.
The application should know when no more ends are needed and communicate this to the user.

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
