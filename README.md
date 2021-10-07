# ScoutingCore
ScoutingCore is an open source modular scouting app framework developed by Team 3061 Huskie Robotics. ScoutingCore provides a simple platform upon which a team can build a scouting app with little to no prior experience.

ScoutingCore is built with HTML, JS, CSS, and Node.js and operates with a MongoDB database.


## Features
- An intuitive customizable interface for building your scouting app adaptable to any FRC game.
- Easy to use platform for data entry throughout matches.
- Analysis page to display statistics and charts about matches and teams.
- Admin view for live scouter management at competition.
- Easy scouter tracking and analytics through Google authentication or a form.


## Config Info

### client.json
`client.json` includes all of the information that encompasses a scouting app's basic frontend and data collection.

- `gameId`: A unique identifier for the current year's game. This is separate from version. 
- `version`: A string identifying the scouting frontend's version.
- `timing`
    - `totalTime`: The total amount of time in a game in ms.
    - `timeTransitions`: An object with keys being numbers (in the JSON as a string) denoting the start of a time-based layer transition (see `layers`). The display text is then displayed in any match-control buttons.
- `layout`: the layout of a scouting app's main grid.
    - `gridRows`: The number of rows in the scouting app's main grid.
    - `gridColumns`: The number of columns in the scouting app's main grid.
    - `layers`: A layer is an array of objects representing each button of the scouting app visible in a layer. By default, the layer at index 0 of layers is shown, but this can be changed through `timing`, or by `executables` triggered by a button press.
        - `id`: A unique identifier string for the button that represents a scouting action
        - `displayText`: The text to be displayed inside of the button (optional)
        - `gridArea`: An array of four strings representing each element of a CSS grid-area attribute that will be applied to the button (ex. `0 / 1 / 1 / 2` becomes `["0","1","1","2"]`)
        - `class`: A CSS class to be applied to the button.
        - `type`: A string representing the type of scouting action the button represents.
            1. `"action"`: When pressed, add a robot action to the action queue (eg. score, climb) and run any executables if applicable.
            2. `"undo"`: When pressed, remove the last action from the action queue and run any executables if applicable.
            3. `"none"`: When pressed, run any executables if applicable without modifying the action queue.
            4. `"match-control"`: Is either a start button, timer, or end button depending on the time remaining in the match.
        - `executables`: An array of objects representing custom tasks executed when the button is pressed beyond those specified by their type.
            - `type`: Name of an executable function.
            - `args`: A static array of arguments passed into the executable function.

## Pages
Pages are the modular component of the scouting frontend. Every view that a scouter sees is a page. Pages are stored in `/src/scouting/views/pages/` as .ejs files. By default, the initial page is `landing.ejs`, but can be changed in `/src/scouting/scouting.js`.

JS and CSS files for these pages can be found in `/src/scouting/public/css/` or `/src/scouting/public/js/`. Make sure to include your JS and CSS files inside your page individually. 


### Page JS Best Practices
`/src/scouting/public/js/internal.js` contains JS functions or code that will be included in every page. 

Minimize your use of  global functions and variables. These should only be used if a function or variable is shared between multiple pages.


### Page CSS Best Practices

`/src/scouting/public/css/global.css` contains global CSS variables and styles that will be available to use in all pages.

`/src/scouting/public/css/internal.css` contains CSS styles integral to the proper functioning of the app.

When selecting elements in your page-specific CSS files, make sure to begin your selector with `#[pagename]` where `[pagename]` is your page's name.

(ex: `#landing .action` to select elements with the `action` class in the `landing.ejs` page)

### Page JS Utilities

Use the function `switchPage` in pages (located in `internal.js`) to switch pages.