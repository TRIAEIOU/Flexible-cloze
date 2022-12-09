# Flexible cloze 2.0

Reimplementation of [Flexible cloze](https://ankiweb.net/shared/info/1632356464) Anki addon to handle nested clozes avalable from Anki 2.15.XYZ as well as remove lesser used features for easier maintenance. FCZ2 is a configurable cloze note type for keeping related information (and cards) on the same note.

**Ideas for the functionality of this cloze variant blatantly stolen from trgkanki's Cloze (Hide all) [https://ankiweb.net/shared/info/1709973686] and RisingOrange's Enhanced Cloze (for Anki 2.1) [https://ankiweb.net/shared/info/1990296174] - both of which are excellent addons. However all code written from scratch (ok, I peeked at some other code).**

**ALL CREDIT FOR INNOVATION GOES TO TRGANKI AND RISINGORANGE**

<img src="https://github.com/TRIAEIOU/Flexible-cloze/blob/main/Screenshots/front-and-back.png" height="500">

## General

- Active cloze: Cloze(s) with the current ordinal, i.e. the cloze(s) that should be answered. To change styling of these change or override `.cloze` class in "Styling" of the card template.
- Inactive cloze: Cloze(s) that are not the current ordinal, i.e. the cloze(s) that should not be answered.To change styling of these change or override `.cloze-inactive` class in "Styling" of the card template.
- Exposed cloze: Cloze(s) that when inactive (see above) will always be in "shown" state. Mark a cloze as exposed in the editor by making the first character an exclamation mark (e.g. `{{c1::!This will always be displayed when the cloze ordinal is not 1}}`).
  - Configurable expose character(s), default is `!`
  - Configurable position of the expose position to allow use with {{type:cloze:Text}}:
    - pre: **!**{{c1::content}}
    - begin (default): {{c1::**!**content}}
    - end: {{c1::content **!**}}
    - post: {{c1::content}}**!**
  - Configurable to reverse the expose status, i.e. all inactive clozes are exposed and those marked as "expose" will be hidden.
  - Example: `--expose: # pre reverse;`
- Cloze prompts can be congigured:
  - Prompt format/text for clozes without hint. Example: `--prompt: "[...]";`
  - Prompt format/text for clozes with hint where `%h` will be replaced by the hint text. Example: `--hint: "[%h]";`
- Scrolling: Configurable scrolling behaviour on card show/flip, when clicking a hidden cloze and when cycling to next/previous with edge taps or keyboard shortcuts:
  - none: no scroll
  - min: scrolls as little as possible to get active cloze(s) into view
  - center: centers the active clozes in the vindow.
  - Scroll on initial display of front (question) side: `front-none`, `front-min`, `front-center`, `front-context`
  - Scroll on initial display of back (answer side): `back-none`, `back-min`, `back-center`, `back-context`
  - Scroll on iterate (pressing next key etc.):`iterate-none`, `iterate-min`, `iterate-center`, `iterate-context`
  - Scroll on click: `click-none`, `click-min`, `click-center`, `click-context`
  - Example: `--scroll: front-context back-min iterate-min click-min`;
- Iteration - cycling active clozes with keyboard shortcut or edge tap. Iteration behaviour can be configured as follows:
  - Hide the cloze "you are leaving" when iterating: `hide`
  - Loop iteration once you reach the first/last (otherwise you will stop): `loop`
  - Always start iteration from the top (otherwise iteration will "continue" from the last clicked item): `top`
  - Example: `--iterate: top loop hide;`
- Cloze initial display behaviour can be configured to show per default as follows:
  - Inactive clozes: `inactive-front`, `inactive-back` (setting both these will make FCZ2 behave similar to core Anki clozes)
  - Addtional fields (including Information field): `additional-front`, `additional-back`
  - Information field (regardless of Additional fields): `info-front`, `info-back`
  - Example: `--show: info-front additional-back`
- Styling of different elements (e.g. "I want the answer to be displayed inline rather than in a block") can easily be configured in the "Styling" section of the card template.
- To facilitate end user modifications of the layout, style and function of the template updates will come with a dialog allowing the user to determine which parts to update. A temporary backup (will be overwritten on next update) of the current template will be created in the add-on folder.
- Changes will mainly be made inside "FCZ BEGIN/END" tags, which in turn is divided into functionality and styling allowing the user to avoid overwriting the styling part on update.
- Configuration is made in the "Styling" section of the card template under the ".fcz-config" heading:

``` CSS
<pre><code>/* FLEXIBLE CLOZE CONFIGURATION======================================= */
.fcz-config {
    --prompt: "[...]";
    --hint: "[%h]";
    --expose: ! begin;
    --scroll: front-context back-min iterate-min click-min;
    --iterate: hide loop top;
    --key-next-cloze: j;
    --key-previous-cloze: h;
    --key-toggle-all: k;
    --show: additional-back info-front info-back;
}
```

- Clicking an active cloze on the front side will cycle it between hint (if there is one) and show.
- Clicking an inactive cloze on the front side will cycle it between hide and show (no hint).
- A suggestion is using deck options `Insertion order: sequential (oldest cards first)`, `Bury new siblings: off` and `Bury review siblings: off` when writing notes that are "sequential" in nature. I.e. when the later clozes in the note requires knowing the earlier clozes. This makes the clozes be presented in ascending order on new cards (which is when it needs to be ordered). Once learned they will not be ordered (or even on the same day depending on how well/fast you learn the different clozes) but then you already "know the preceding clozes".
- There is an optional "show all" button (styleable in class .fcz-show-all-btn). Note that he button is set to `display: none` in default configuration, you have to set it to `display: inline` on the Styling page of the cards dialog to get it to show:

``` CSS
/* SHOW ALL BUTTON ============================================== */
/* Show all button/bar styling (and if visible or not) */
#fcz-show-all-btn
{ display: inline;  background-color: #465A65; color: white; text-align: center; text-transform: uppercase;
font-size: 15px; font-weight: bold; padding: 5px; border-bottom: 1px solid white; }
```

## Regarding styling

The default styling of the template does not look like "regular Anki clozes". You can have the clozes display however you want by adjusting the CSS on the "Styling" page of the "Cards" dialog. To achieve the "regular Anki cloze styling":

In the `FLEXIBLE CLOZE CONFIGURATION` section set:

``` CSS
--prompt: "[...]";
--hint: "[%h]";
```

In the `CLOZE STYLING` section replace all the content with:

``` CSS
span.cloze {color: blue; font-weight: bold;}
```

## Main difference from the earlier mentioned add-ons

There is effectively no add-on, it's all JavaScript (and HTML/CSS) and runs 100% "client side" (the only python is the update logic). The logic requires an Anki version based on the 2.15.XYZ back end (i.e. Anki desktop 2.15.XYZ, AnkiDroid 2.XYZ or AnkiMobile 2.XYZ).

- The only thing the add-on does is insert a new note type (Flexible Cloze) with the relevant JS/HTML/CSS, once you have it (and don't want any updates) feel free to uninstall the add-on. Similarly you can share decks with anyone without any need for them to install anything since everything is in the note type.
- Included is my note styling and configuration (the way it functions and which fields are present are more or less a complete rip-off from RisingOrange). However, you can edit the note type however you want if you know a little HTML and CSS.
- This allows for keeping related content on the same note facilitating note creation (no need to search through the deck to see if you already added a card with similar content). It can also help when reviewing as you can look at the other related clozes if you need to check something (e.g. "Well if it wasn't that, what was it?"). This is how I design my notes, hence the layout.
- I would recommend keeping any note type edits outside the FCZ BEGIN/END marks as content inside will be overwritten if the addon is updated (assuming you still have it installed). However if you want to keep the add-on for updates but want to muck about inside the begin/end tags I would suggest you duplicate the note type and rename your version to whatever (updates are made only on the appropriately named note type).
- Since all is on the note configurations (like how the clozes look etc.) are in the CSS there is no configuration from the add-on pane.
- Hardly an important difference but I use the flags for marking cards that needs to be corrected in different ways so there is a flag legend at the bottom (colors and text configurable) that takes up minimal space. Similarly there is a figure legend at the bottom (edit the front template to insert/change symbols).

## Similarities with the above two add-ons

- Almost identical fields, albeit styled differently, as Enhanced Cloze. But as earlier mentioned, you can rip out whatever you don't want or copy/paste the card contents to a new note type and design your own as long as:
- You have a div (or span) tag with id "fcz-content" somewhere as that is where the content is inserted runtime.
- You set the class "fcz-scroll-area" on the element where you want scrolling to occur.
- You have the {{FrontSide}} on the back side (to make the JS available).
- As with Enhanced Cloze you can navigate with keyboard shortcuts (h-j-k per default, configurable) or the edges of the screen.
- As with Enhanced Cloze you can expand individual clozes (active and inactive) on the front side as well as iterate through the active ones (e.g. all {{c:2}} clozes one by one) by pressing the side edges of the screen or keyboard shortcuts. This can be practical to learn lists or tables. You can have the cloze you iterate away from remain expanded or collapse (configurable).
- As with Enhanced Cloze you can expand (and collapse) all clozes (active and inactive) by pressing the top of the screen. Additional fields (below the main Text field) can be expanded and collapsed by clicking them. It functions a little differently from Enhanced Cloze but the general idea is the same.
