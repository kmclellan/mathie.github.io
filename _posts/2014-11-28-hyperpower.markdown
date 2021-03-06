---
title: HYPERPOWER!
subtitle: Putting the caps lock key to work
excerpt: |
  Today, we'll figure out how to replace the caps lock key with the (far more
  useful) HYPER key on Mac OS X, and have a think about the different ways we
  can put it to use.
category: Ops
tags:
  - hyper
  - capslock
  - Alfred
  - Seil
  - Karabiner
---

I can't recall how I came across it, but recently I read a post on
[A useful Caps Lock key](http://brettterpstra.com/2012/12/08/a-useful-caps-lock-key/)
and I've actually started *using* a key that's sitting on the home row, so
tantalisingly close to my left pinky. So I thought I'd share my discovery, and
the slightly different tack I've taken, just in case it helps somebody else
reduce their RSI. I'm actively monitoring my workflow now, looking for
additional repetitive tasks I perform that could be bound to my new HYPERPOWER!
key. (Nine Inch Nails, from the Year Zero album, in case, like me, the word
'hyper' sparked something in your memory, but it took a while to make the
connection.) I'd love to hear your suggestions of what we could shorten. :)

It's a bit awkward to turn the caps lock key into something remarkably useful,
but it comes down to four things:

* Switching off its default behaviour, because who needs to WRITE IN ALL
  CAPITALS WITHOUT HOLDING DOWN THE SHIFT KEY? (I held down the shift key while
  I was writing that, and it's the hardest work my left pinkie has done all
  day.) This option is baked into Mac OS X, though it's a little tricky to
  find, if you don't already know where.

* Make the caps lock key perform two different operations, depending on how
  long you hold it down for. This pays homage to the vi(m) users, because we'll
  turn the caps lock key into an escape if you tap it, and only give it
  HYPERPOWER! if you hold it down for a wee while.

* If you hold down the caps lock key for a short while -- enough to find its
  key combination -- then we'll turn it into a different keypress (really, the
  equivalent of holding down multiple other keys) so that we can assign
  behaviour to the combination of it and other keypresses.

* Finally, we'll set up a pile of global shortcut keypresses (our new shortcut
  key, combined with others) to automate tasks that previously tired out our
  poor old fingers.

The bad news is that doing all this requires a couple of third party programs.
The worse news is that doing all this requires a couple of third party kernel
extensions. The good news is that they seem stable enough, are open source and
(I hope!) are well enough reviewed that they aren't logging all my keystrokes
to MI5. ;-) (Though I'm sure your Intelligence Agency could read these
keystrokes easily enough anyway...!)

## Switching off Caps Lock

I've been doing this bit for years. On every new Mac, on every fresh install of
Mac OS X, one of the first things I've done is just to disable the caps lock
key entirely. Why? It's annoyingly easy to press. And it's particularly
irritating when you accidentally fat-finger it before entering a (case
sensitive) password. On the plus side, Mac OS X now gives you a visual
indication when the caps lock is active in a password field, but still. Let's
disable it:

* Head to System Preferences;

* Go to the Keyboard preferences;

* Under the 'Keyboard' tab, click 'Modifier Keys...';

* In the Caps Lock drop-down, select 'No Action' and click 'OK'.

There we go, that's the caps lock key neutered. Hit it and note that the wee
light on the top left doesn't come on. Winning.

<figure class="thumbnail">
  <img src="/img/keyboard-preferences.png" alt="Switching off the Caps Lock key in system preferences." class="img-responsive">
  <figcaption class="caption">
    <p>
      Switching off the caps lock key in system preferences. Set the value for
      Caps Lock to "No Action" to completely disable its default behaviour.
    </p>
  </figcaption>
</figure>

## Putting the Caps Lock Key to Work

The next thing we need to do is remap the caps lock key so it sends Mac OS X a
different key code when it's pressed. For this, we'll use a wee utility called
[Seil](https://pqrs.org/osx/karabiner/seil.html.en) (it used to be called
PCKeyboardHack). Download that and install it, then run it to configure our
caps lock key.

Essentially, we're looking to remap it to another key on the keyboard, so that
we can assign some behaviour. However, it makes most sense to pick a key that
doesn't really exist on your own keyboard. On my laptop, the `F19` key (key
code `80`, discovered from the list of available keycodes at the bottom of the
Seil configuration window) doesn't exist, so that's a good bet. Specify your
own choice of key code, and make sure 'Change the caps lock key' is selected:

<figure class="thumbnail">
  <img src="/img/seil.png" alt="Changing the keycode for caps lock in Seil." class="img-responsive">
  <figcaption class="caption">
    <p>
      Changing the keycode for caps lock in Seil.
    </p>
  </figcaption>
</figure>

Seil is pretty much a single-purpose helper to remap the caps lock key. We'll
need its more powerful big brother,
[Karabiner](https://pqrs.org/osx/karabiner/index.html.en) (née
KeyRemap4MacBook) to assign more flexible behaviour. Download, install and run
it. There are tonnes of things you can do in here, which is to say that it
makes for a complicated UI! To assign behaviour to the caps lock key, head to
the 'Misc & Uninstall' tab, and click 'open private.xml'. Edit the file in your
favourite text editor to have the contents:

{% highlight xml %}
<?xml version="1.0"?>
<root>
	<item>
		<name>F19 to F19</name>
		<appendix>(F19 to Hyper (ctrl+shift+cmd+opt) + F19 Only, send escape)</appendix>
		<identifier>private.f192f19_escape</identifier>
		<autogen>
			--KeyOverlaidModifier--
			KeyCode::F19,
			KeyCode::COMMAND_L,
			ModifierFlag::OPTION_L | ModifierFlag::SHIFT_L | ModifierFlag::CONTROL_L,
			KeyCode::ESCAPE
		</autogen>
	</item>
</root>
{% endhighlight %}

Save that, go back to Karabiner, choose the "Change Key" tab and hit 'Reload
XML' at the top right. Make sure your new "F19 to F19" mapping is enabled.

So, what have we done here? The end result is two things:

* Pressing the caps lock key is now equivalent to pressing the F19 key, a key
  that doesn't really exist on my laptop keyboard.

* Pressing that F19 key has now got two overlaid behaviours:

  - a single press is the equivlant of hitting the escape key; and

  - holding it down is the equivalent of holding down the left command, option,
    shift and control keys.

The latter gives us our HYPERPOWER! key. There aren't any built in shortcuts in
Mac OS X that bind themselves to cmd-opt-shift-ctrl-anything, mostly because
it's such an awkward key combination to hold down that it doesn't count as a
shortcut! But mapping it to just holding down the caps lock key suddenly makes
it useful.

## Hyper Shortcut Keys

This is where I deviate from the other instructions I've been reading. They
recommend [Better Touch Tool](http://bettertouchtool.net/) or
[Keyboard Maestro](http://www.keyboardmaestro.com/main/) to perform all the key
mapping, both of which are excellent and powerful tools. However, I was happy
to have fewer moving parts, and I already use the awesome
[Alfred](http://www.alfredapp.com) to improve my productivity. In order to set
up a few new shortcut keys, I've set up a single Alfred Workflow, called "Hyper
Keys" which has a set of hotkey triggers joined up to a set of actions:

<figure class="thumbnail">
  <img src="/img/alfred-hyper-workflow.png" alt="My Alfred Hyper Key workflow." class="img-responsive">
  <figcaption class="caption">
    <p>
      My Alfred Hyper Key workflow.
    </p>
  </figcaption>
</figure>

So, what sorts of HYPERPOWER! shortcut keys have I come up with so far?

* Switching to applications I commonly use, launching them if necessary.
  HYPER-M jumps to Mail, HYPER-T switches to the Terminal, and HYPER-S switches
  to Safari.

* Opening particular common URLs in Safari. HYPER-5 will launch
  <http://localhost:5000/>, which is most often where the web app I'm currently
  developing will be listening politely for requests.

* Searching for the current text selection. When I've got a piece of text
  selected, HYPER-G will search for it in Google (using Safari), which HYPER-D
  will bring up Alfred's workflow for [Dash](http://kapeli.com/dash) to search
  developer documentation. This latter shortcut is probably the single most
  useful one I've set up.

* HYPER-L will lock the screen, which is nice and easy to hit just as I'm
  getting up from the keyboard.

I'm sure there are tonnes more things that I could shorten. In particular, I've
a feeling that I'd like to automate other command line tasks, or perhaps more
fine grained control of switching to the terminal (i.e. switching to a
particular pane in a certain tmux session), but I haven't quite sussed how to
do that. Perhaps I need Keyboard Maestro after all.

What could you do with a HYPERPOWER! key?