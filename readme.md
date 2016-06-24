# Hackers.wiki


Welcome fella's. 

We are currently in alpha, the wiki is maintained through github. The current flow to contribute is fork -> change -> pull request. Be creative with the index, create directories where needed. 

Let's go!


## Proposal for inner workings

See this as a community project.
My (simplect's) proposal for the wiki:

The wiki is basically a frontend for a git repository. The repository holds everything from wise knowledge to total bullshit. 

The development branch will be public for everyone to add .md's, code files etc. Then there is a rating system. Highly rated files will be added to a trending development. When the file is significantly trending because of real human voting one of the maintainers will merge it to the master branch.

In order to ease merging and keep structure the development branch will be dropped every week and a new development branch will be checkedout.


## Current state

### Working
- Github synchronisation.

### Todo
- Dev/Master branch separation?
- Rating system
- Trending list
- ???

## Security of the wiki

The wiki is hosted on a VPS with nothing else on it. The wiki uses a read-only deployment key to pull the git every minute. So hacking the wiki is useless and just a waste of your own time. 

## License 

If you care about sharing knowledge consider pasting the following license into your wiki:
This work is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License.
