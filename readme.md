![alt text](https://github.com/Hackerswiki/wiki/blob/master/hackerswiki.png "Hackers Wiki")
# Hackers.wiki


Welcome fella's. 

We are currently in alpha, the wiki is maintained through github. 

Let's go!

## Inner workings

The wiki is basically a frontend for the git repository. The repository holds everything from wise knowledge to total bullshit. 

The current flow to contribute is fork -> change -> pullrequest on github. Be creative with the index, create directories where needed. 

The wiki front-end currently only parses Markdown: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
Code is also more than welcome. Use coding standards; or else Torvalds will strangle you in your sleep.

## Security of the wiki

The wiki is hosted on a VPS with nothing else on it. The wiki uses a read-only deployment key to pull the git every minute. So hacking the wiki is useless and just a waste of your own time. 

## License 

If you care about sharing knowledge consider pasting the following license into your wiki:

> This work is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License.
