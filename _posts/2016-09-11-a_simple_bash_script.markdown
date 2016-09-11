---
layout: post
title:  "a simple bash script"
date:   2016-09-11 19:48:43 +0000
---


it is yet another time again for me to write a blog.

i had no ideas. 
then i thought why don't i do something with bash. a couple of weeks ago a fellow student([fidel](https://unorientedobject.wordpress.com/)) asked me how to copy the content of existing file to another directory from bash. he needed to add .gitignore file to the lab he is currently working and he wanted to do it more efficently than creating a new file and copying and pasting the content. i didn't know how to do that back then. then we asked around([wu](https://irevived1.github.io/)) and found a better way to do it. you could use ```cat``` with ```>``` option. 

``` cat [existingfilename]>[newfilename] ``` or ```cat [existingfilename]>>[newfilename]```  if you'd like to append the content to an existing file. 


i thought we can do better than that. it was cool to use cat and all but you still need to write the path correctly, i never get it right at first try anyways.

another thing that was kinda distracting for me was adding pry to gemfile everytime in the labs where it is not included.

both of these things, i thought, could be achived with a bash script. i started googling how to write a bash script. and it was way easier to come up with correct syntax than i anticipated. it took me less than an hour to figure out how to do it. 


Usage

in your home directory open ```.bash_profile``` with your favourite editor. and add following code somewhere appropriate, mine is all the way at the end.



```
#adds pry to gemfile and creates or overwrites .gitignore file with the content from an existing .gitignore file from home directory
function cg {
  echo ------------------------------------------------------------------------
  echo ------------------------------------------------------------------------
  echo _____________________current .gitignore content_________________________
  cat .gitignore
  echo -------------------------end of file------------------------------------
  echo ------------------------------------------------------------------------
}

function apag {

echo ------------------------------------------------------------------------
echo ------------------------------------------------------------------------
if grep -q -r "pry" gemfile
then
  echo "                       gemfile includes pry!"
else
  echo "gem 'pry'" >> gemfile
  echo "                        pry added to gemfile"
fi
echo ------------------------------------------------------------------------
echo ------------------------------------------------------------------------
echo ""
echo ""
if what .gitignore > /dev/null 2>&1;
then
  cg
  echo "do you wish to overwrite .gitignore file?"

  select yn in "yes" "no"; do
    case $yn in
      yes ) cat ~/.gitignore > .gitignore; break;;
      no ) break;;
    esac
  done
else
  cat ~/.gitignore > .gitignore
  cg
fi
  echo done!
}

```

...and then run ```apag```...
short for addpryandgitignore. you can change it to anything you like. 

