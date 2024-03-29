# Flexible cloze

**\*\*\*DISCONTINUATION NOTICE: From Anki 2.1.56 Flexible cloze has been superseeded by [Flexible cloze 2](https://ankiweb.net/shared/info/1889069832), no further updates will be made to this version of Flexible cloze\*\*\***

Flexible cloze is an [Anki addon](https://ankiweb.net/shared/info/1632356464) for a configurable cloze note type for keeping related information (and cards) on the same note ([Anki forum thread](https://forums.ankiweb.net/t/flexible-cloze-support-thread/14504)).

**Ideas for the functionality of this cloze variant blatantly stolen from trgkanki's Cloze (Hide all) [https://ankiweb.net/shared/info/1709973686] and RisingOrange's Enhanced Cloze (for Anki 2.1) [https://ankiweb.net/shared/info/1990296174] - both of which are excellent addons. However all code written from scratch (ok, I peeked at some other code).**

**ALL CREDIT FOR INNOVATION GOES TO TRGANKI AND RISINGORANGE**

![screenshot](https://github.com/TRIAEIOU/Flexible-cloze/raw/main/Screenshots/front-and-back.png){height=500px}

## General

- Active cloze: Cloze(s) with the current ordinal, i.e. the cloze(s) that should be answered. To change styling of these change or override "fcz-active" class in "Styling" of the card template.
- Inactive cloze: Cloze(s) that are not the current ordinal, i.e. the cloze(s) that should not be answered.To change styling of these change or override "fcz-inactive" class in "Styling" of the card template.
- Exposed cloze: Cloze(s) that when inactive (see above) will always be in "shown" state on the front side. Mark a cloze as exposed in the editor by making the first character an exclamation mark (e.g. `{{c1::!This will be displayed on the front when the cloze ordinal is not 1}}`).
  - Configurable expose character(s), default is _!_
  - Configurable position of the expose position to allow use with {{type:cloze:Text}}:
    - pre: **!**{{c1::content}}
    - begin (default): {{c1::**!**content}}
    - end: {{c1::content **!**}}
    - post: {{c1::content}}**!**
  - Configurable to reverse the expose status, i.e. all inactive clozes are exposed and those marked as "expose" will be hidden.
  - Example: `--expose: # pre reverse;`
  - To change styling of these change or override "fcz-expose" class in "Styling" of the card template.
- Scrolling: Configurable scrolling behaviour on card show/flip, when clicking a hidden cloze and when cycling to next/previous with edge taps or keyboard shortcuts:
  - `none`: no scroll
  - `min`: scrolls as little as possible to get active cloze(s) into view
  - `center`: centers the active clozes in the vindow.
  - `context`: scroll to center (above) if this means the line following the preceding cloze will be visible, otherwise scroll to the line following the preceding cloze (i.e. show the cloze context) - this may result in the current cloze being below the screen.
  - `chapter-context`: scroll to center (above) if this means the line following the preceding cloze will be visible, otherwise scroll to preceding `<hr>` or `<h1>`-`<h6>` or top of card (i.e. show all of the "chapter") - this may result in the current cloze being below the screen.
  - Scroll on initial show/flip: `context`, `center`, `min` or `none`.
  - Scroll on iterate (pressing next key etc.): `iterate-center`, `iterate-min` or `iterate-none`.
  - Scroll on click: `click-center`, `click-min` or `click-none`.
  - Example: `--scroll: context iterate-min click-min`;
- Iteration - cycling active, inactive or all clozes with keyboard shortcut or edge tap. Iteration behaviour can be configured as follows:
  - Which clozes should be cycled: `active`, `inactive` or `all`.
  - Hide the cloze "you are leaving" when iterating: `hide`
  - Loop iteration once you reach the first/last (otherwise you will stop): `loop`
  - Always start iteration from the top (otherwise iteration will "continue" from the last clicked item): `top`
  - Example: `--iterate: active top loop hide;`
- Non-active cloze and non-cloze field display behaviour can be configured to show per default as follows:
  - Inactive clozes: `inactive-front`, `inactive-back` (setting both these will make FCZ behave similar to core Anki clozes)
  - Addtional fields (including Information field): `additional-front`, `additional-back`
  - Information field (regardless of Additional fields): `info-front`, `info-back`
  - Example: `--show: info-front additional-back`
- Styling of different elements (e.g. "I want the answer to be displayed inline rather than in a block") can easily be configured in the "Styling" section of the card template.
- To facilitate end user modifications of the layout, style and function of the template updates will come with a dialog allowing the user to determine which parts to update. A temporary backup (will be overwritten on next update) of the current template will be created in the add-on folder.
- Changes will mainly be made inside "FCZ BEGIN/END" tags, which in turn is divided into functionality and styling allowing the user to avoid overwriting the styling part on update.
- Configuration is made in the "Styling" section of the card template under the ".fcz-config" heading:
  
  ``` CSS
  /* FLEXIBLE CLOZE CONFIGURATION======================================= */
  .fcz-config {
      --cloze-element: div;
      --inactive-prompt: ;
      --active-prompt: ;
      --expose: ! begin;
      --scroll: center iterate-min click-min;
      --iterate: active hide loop top;
      --key-next-cloze: j;
      --key-previous-cloze: h;
      --key-toggle-all: k;
      --show: info-front additional-back info-front;
  }
  ```

- Clicking an active cloze on the front side will cycle it between hint (if there is one) and show.
- Clicking an inactive cloze on the front side will cycle it through hide - hint - show.
- When writing notes that are "sequential" in nature (i.e. when the later clozes in the note requires knowing the earlier clozes) a suggestion is to configure Anki to present new cards in order (once they are no longer new they will not come in order but that is less important):
  - Using the V3 scheduler set the deck new card insertion order to `Sequential`.
  - Using an older scheduler use [show new siblings in order / no same day spacing(randomization) for new siblings](https://ankiweb.net/shared/info/268644742).
- There is an optional "show all" button (styleable in class .fcz-show-all-btn), note that:
  - The button is set to display: none in default configuration, you have to set it to display: inline on the Styling page of the cards dialog to get it to show.

  ``` CSS
  /* ANSWER SIDE STYLING ============================================== */
  /* Show all button/bar styling (and if visible or not) */
  .fcz-show-all-btn
  { display: inline;  background-color: #465A65; color: white; text-align: center; text-transform: uppercase;
  font-size: 15px; font-weight: bold; padding: 5px; border-bottom: 1px solid white; }
  ```

- If you are updating from a previous version of the template and choose to not "overwrite all" (overwriting any personal changes you have made) you will have to manually add the actual button on the front as this requires changing the HTML outside the FCZ BEGIN/END tags. Insert `<div id="fcz-show-all" class="fcz-show-all-btn" style="cursor: pointer;" onclick="fcz_toggle_all()">Show all</div>` just after the closing div-tag of the `<div id="fcz-additional" style="z-index: 2;">` (have a look in the add-on directory in fcz-front.html).
- <https://github.com/TRIAEIOU/Flexible-cloze>

## Regarding styling

As there have been a few questions regarding the default styling of the template not looking like "regular Anki clozes". You can have the clozes display however you want by adjusting the CSS on the "Styling" page of the "Cards" dialog. To achieve the "regular Anki cloze styling":
In the "FLEXIBLE CLOZE CONFIGURATION" section set:

``` CSS
--cloze-element: span;
--inactive-prompt: [...];
--active-prompt: [...];
```

Replace all the content under the "CLOZE STYLING" section with `.fcz-active { color: blue; font-weight: bold; }`. If by chance I am little off on the font-weight you can fine-tune it by starting at 400 which is normal font-weight and going upward (900 would be "very bold"). Similarly if by chance the blue nuance is off you can insert the correct RGB instead, e.g. #0000FF.

## Main difference from the earlier mentioned add-ons

There is effectively no add-on, it's all JavaScript (and HTML/CSS) and runs 100% "client side" (the only python is the update logic). This has a number of effects:

- The only thing the add-on does is insert a new note type (Flexible Cloze) with the relevant JS/HTML/CSS, once you have it (and don't want any updates) feel free to uninstall the add-on. Similarly you can share decks with anyone without any need for them to install anything since everything is in the note type.
- There are no special fields etc., the note type uses vanilla Anki clozes and parses them runtime.
- There are no complaints from Anki on missing clozes and no insertions into the notes of any type of markup or extra fields (so for anyone wanting to go to Anki's limit of 500 clozes, go ahead).
- Included is my note styling and configuration (the way it functions and which fields are present are more or less a complete rip-off from RisingOrange). However, you can edit the note type however you want if you know a little HTML and CSS.
- This allows for keeping related content on the same note facilitating note creation (no need to search through the deck to see if you already added a card with similar content). It can also help when reviewing as you can look at the other related clozes if you need to check something (e.g. "Well if it wasn't that, what was it?"). This is how I design my notes, hence the layout.
- I would recommend keeping any note type edits outside the FCZ BEGIN/END marks as content inside will be overwritten if the addon is updated (assuming you still have it installed). However if you want to keep the add-on for updates but want to muck about inside the begin/end tags I would suggest you duplicate the note type and rename your version to whatever (updates are made only on the appropriately named note type).
- If you, like me tend to do your reviews on a tablet the effect of having everything on the note makes it possible to edit the note on, for instance AnkiDroid, without any issues.
- Since all is on the note configurations (like how the clozes look etc.) are in the CSS there is no configuration from the add-on pane.
- Hardly an important difference but I use the flags for marking cards that needs to be corrected in different ways so there is a flag legend at the bottom (colors and text configurable) that takes up minimal space. Similarly there is a figure legend at the bottom (edit the front template to insert/change symbols).

## Similarities with the above two add-ons

- Almost identical fields, albeit styled differently, as Enhanced Cloze. But as earlier mentioned, you can rip out whatever you don't want or copy/paste the card contents to a new note type and design your own as long as:
- You have a div (or span) tag with id "fcz-content" somewhere as that is where the content is inserted runtime.
- You set the class "fcz-scroll-area" on the element where you want scrolling to occur.
- You have the {{FrontSide}} on the back side (to make the JS available).
- As with Enhanced Cloze you can navigate with keyboard shortcuts (h-j-k per default, configurable) or the edges of the screen.
- As with Enhanced Cloze you can expand individual clozes (active and inactive) on the front side as well as iterate through the active ones (e.g. all {{c:2}} clozes one by one) by pressing the side edges of the screen or keyboard shortcuts. This can be practical to learn lists or tables. You can have the cloze you iterate away from remain expanded or collapse (configurable) and also select whether all clozes, only active or inactive clozes should be iterated.
- As with Enhanced Cloze you can expand (and collapse) all clozes (active and inactive) by pressing the top of the screen. Additional fields (below the main Text field) can be expanded and collapsed by clicking them. It functions a little differently from Enhanced Cloze but the general idea is the same.

## Changelog

- 2021-11-13: Added selective update dialog inc. temporary backup of previous template. Added exposed cloze functionality (see above). Added option to iterate inactive clozes (see above). Added option to always start next/previous iteration from top (see above). Split FCZ BEGIN/END content into FUNCTIONALITY and STYLING for selective updates. Added symbol legend at bottom. Refactoring of code.
- 2021-11-15: Added functionality/options to initially show or hide inactive clozes (--show-all-on-back) and additional fields (--show-additional-on-back). Added a "show all button" (stylable under .fcz-show-all-btn-class).
- 2021-12-15: Custom scroll logic implemented, it should now automatically scroll to fit as many active clozes as possible when required (scrolling as little as possible but with a single line margin). Minor code refactoring.
- 2021-12-26: Adjustment of scroll logic to facilitate user custom card layout. If not using (or not updating to the latest version of) the "default" Flexible Cloze template the &lt;div&gt; that should be scrolled needs to have the "fcz-scroll-area" class (the scroll logic is applied to the first element with the fcz-scroll-area class).
- 2022-01-01: Scroll logic bug fix.
- 2022-01-06: 2022-01-05: Scroll logic bug fix (again), restructuring of HTML on "Front" to correct scroll bugs.
- 2022-02-11: Made exposed cloze character/string configurable (to allow character/string that does not cause TTS to kick in), made scroll behaviour configurable (as little as possible or center active clozes).
- 2022-02-13: Bug fixes, refactoring. Moved {{FrontSide}} on back of card inside FCZ functionality tag (required for current design).
- 2022-02-21: Refactor front side HTML for better compatibility with addons that inject HTML on the card, such as Remaining time for Anki 21 (update requires migration to new HTML model). Further enhancement of scrolling (await loading of images on image heavy cards). Added configurable position of expose character/string: pre/begin (default)/end/post cloze tags for better compatibility with type-in-the-answer (avoid having to type the expose character/string). Added configurable reverse-expose: all inactive clozes per default exposed (similar to "regular" Anki clozes), expose character/string hides them.
- 2022-03-06: Allow separate scroll behaviour on card show/flip, on cloze click and on next/previous iteration (--scroll, --scroll-iterate, --scroll-click: "none"/"min"/"center"). Replace --iterate-inactive option with --iterate: "active"/"inactive"/"all" to allow more fine grained customization.
- 2022-05-27: Configuration options and internal logic becoming a bit of a mess, reworked so to new configuration options which unfortunately requires updates to new format to adapt old styles to the new format (see instructions above for use).
- 2022-10-16: Add `context` scrolling option.
- 2022-12-12: Add `chapter-context` scrolling option, scroll caused code refactor.
- 2023-01-21: All development moved to [Flexible cloze 2](https://ankiweb.net/shared/info/1889069832).
