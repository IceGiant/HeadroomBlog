Title:
That Time I Discovered Unicode has a Formatting Stack While Fixing a Localization Bug

Permalink:
arabic-localization

Tagged: right to left languages, software engineering, text processing, ui, Unicode 

This story happened back in 2015. It feels so long ago, back when Sublime Text was my primary text editor outside an IDE rather than VS Code (or VS Codium these days when I don't want the telemetry). 

For those unaware, RTL languages (Arabic and Hebrew are what I worked with), can cause all kinds of annoyances and problems in getting the UI to look nice and be comprehensible to those who use those languages. Everything you're used to having on the left side of the UI switches to being on the right side and vice versa along with text reading from the right side of the line.

Back when this happened, there had been a number of RTL (Right to Left) issues where localization to RTL languages was causing a problem with the old Windows Open File dialog. When opening that window for a user to select a file, you pass it a filter string, which looks something like this:

`Extension type (*.ext)|*.ext`

When the direction was set to RTL, it looked more like this:

`txe.*|(txe.*) epyt noisnetxE`

The substring after the `|` is what's used to filter which files show in the UI. If you have a Unicode "Right To Left override" character to make the RTL languages readable for native readers, it breaks the format and you don't get any filtering.

When trying to figure the issue out, one of the technical translators recommended something that led down an unexpected rabbit hole. She pointed me toward an editor specifically for dealing with Unicode that had a useful ability to insert and render invisible control characters, including a Right to Left Override character. (Back then, text editors weren't as good at dealing with Unicode as they are now) Seeing that override, it followed that there was a similar Left to Right Override, which was the case. I then spotted a name for a character whose acronym collides with a well known document format: PDF, or Pop Directional Formatting. I then went and read the official spec for that character to gain a deeper understanding.

Anyone who's dealt with computing and knows what a stack is probably just reacted like I did on discovering that: Unicode has a directional formatting stack where you can push and pop elements!? That character allowed me to properly fix the issue. Now, someone might think: does it really make a difference that the text direction is perfect when people who read Hebrew or Arabic probably have to get used to reading it backward for dealing with software that doesn't support the language as well? Yes and no. It matters less for Hebrew because each character is distinct like in printed English, but Arabic is a script you could compare to cursive, and since the characters are linked together, words would look very different when the characters are reversed. Also, I'm just a stickler for getting the UI right. 

Because of how other text editors rendered the text at the time, the end result looked more like this when the text wasn't in the localization editor: 

`<LeftToRight override char> <RightToLeft override char> <Pop Directional Format char> RTL reading Arabic text (*.ext)|*.ext`

For editors that displayed the directional formatting correctly, it looked more like this:

`<LeftToRight override char> <RightToLeft override char> RTL Arabic text <Pop Directional Format char> (*.ext)|*.ext`

With this as the end result:

`شهادة الجهاز *.pfx |(*.pfx)`

If you copy/paste that result text into an editor, put your cursor somewhere near the end of the string and tap the left arrow on your keyboard, seeing where the cursor goes.You'll get a better idea of how the text is read vs how Windows processes the text. I hope you enjoyed learning this obscure bit of information about text display and processing. 

PS,
If you were wondering whether I made the title sound like one of those overly long anime/manga titles on purpose, wonder no more. 🙂