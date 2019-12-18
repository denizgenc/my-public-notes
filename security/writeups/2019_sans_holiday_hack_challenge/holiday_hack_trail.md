Easy
----
Just change the distance value to somewhere near 7900.

Medium
------
Go into the medium version of the trail, then right-click and inspect element inside the frame
(clicking one of the buttons helps).

Find the `statusContainer` div, and then change the value in the `distance` input to 8000. Press the
"Go" button to win.

Hard
----
The above should still apply, but now the `hash` input in the `statusContainer` div is actually
being calculated, and if the hash of the values is incorrect, you are "thrown off the trail" or
whatever.

I wonder if it's MD5 or something.
