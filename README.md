# arXiv Reference Management using Bear.app

I am totally in love with [Bear.app](https://bear.app).
It's the first notes app that really works for me.
It's the first notes app I use consistantly throughout my life.
And using Bear I actually find the information I am looking for right when I need it.

When I didn't find a good reference management software, it was therefore natural to use Bear for it.
In the beginning I just created a note for each paper manually.
Over time, this felt extremely repetitive because in my field (machine learning),
almost all the papers can be found on [arXiv](https://arxiv.org) and thus I always copied the same information
from arXiv to Bear.

I am a big fan of automatizing repetitive tasks.
So I built a simple bookmarklet that automatically creates a new note in Bear and adds the correct title, authors, tags and a link back to arXiv.

In the following I explain how you can create your own bookmarklet.

## 1. Adjusting Template

First, you should adapt the following template to your needs.
You should only change the first line.
It builds the content of the note:

* a line break (`\n`)
* a tag (e.g. `#phd/publication`)
* two line breaks (`\n\n/`)
* the author list, in italic (`/${document.querySelector('.authors').innerText}/`)
* two line breaks (`\n\n/`)
* a link back to arXiv (`[${document.title.replace("[","").replace("]","")}](${window.location.href})`)
* two line breaks (`\n\n/`)
* a headline (`## Notes`)
* a line break (`\n`)
* a new list (`* `)

If you are fine with my choices, you probably only need to change the tag to reflect your tag structure.

```js
var text = `\n#phd/publication\n\n/${document.querySelector('.authors').innerText}/\n\n[${document.title.replace("[","").replace("]","")}](${window.location.href})\n\n## Notes\n* `;
var url = 'bear:/' + `/x-callback-url/search?term=${encodeURIComponent(document.querySelector('.title').innerText)}`;
(function (text) {
  var node = document.createElement('textarea');
  var selection = document.getSelection();

  node.textContent = text;
  document.body.appendChild(node);

  selection.removeAllRanges();
  node.select();
  document.execCommand('copy');

  selection.removeAllRanges();
  document.body.removeChild(node);
})(text);
window.location = url;
```

## 2. Creating the Bookmarklet

Copy the template with all your changes to a bookmarklet generator.
I recommend [msdlr's JS inject](http://mcdlr.com/js-inject/).
I've tried some others but not all of them worked for me.

## 3. Defining a Keyboard Shortcut

I use the Safari browser.
This step (and the whole bookmarklet) have not been tried with other browsers.
To specify a keyboard shortcut for this bookmarklet, follow these steps:

1. Select Safari
2. Select the *View* menu and click *Show Favorites Bar* (if it's not already visible)
3. Drag the bookmarklet into the favorites bar
4. Go to *Safari* menu and select settings
5. Select the *Tabs* tab
6. Deselect that ⌘1 to ⌘9 switch tabs (if not already deselected)
7. Select the *View* menu and click *Hide Favorites Bar*

## 4. Running the Bookmarklet

Open a paper on arXiv ([example](https://arxiv.org/abs/1712.04248)). Do not open the PDF.
You can now activate the bookmarklet using ⌘1 (if the bookmarklet is not the first position in your favorites bar, use a the appropriate number).

When you execute it, it will ask you whether it can open Bear. Simply press *Enter* or *Return* on your keyboard.
It will open Bear for you and search for the paper title.
This let's you check whether you already have written down some notes for this paper.
If not, you can simply presss ⌘N to create a new note with the correct title.
Finally, you can press ⌘V to paste the note content created from the template above and automatically added to your clipboard.
No you can write down some notes for the paper.
And you can use all the other awesome features of Bear, such as cross-referencing other notes.

## Summary: How to Use the Bookmarklet

1. open arXiv
2. ⌘1
3. Enter or Return
4. ⌘N
5. ⌘V

I wish a single step would be sufficient, but the whole process is actually really fast once you get used to it.
