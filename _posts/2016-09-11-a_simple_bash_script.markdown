---
layout: post
title:  "a simple bash script"
date:   2016-09-11 15:48:44 -0400
---



It is yet another time again for me to write a blog.

I had no ideas. 
Then I thought why don't I do something with bash. A couple of weeks ago a fellow student([Fidel](https://unorientedobject.wordpress.com/)) asked me how to copy the content of existing file to another directory from bash. He needed to add .gitignore file to the lab he is currently working and he wanted to do it more efficently than creating a new file and copying and pasting the content. I didn't know how to do that back then. Then we asked around([Wu](https://irevived1.github.io/)) and found a better way to do it. You could use ```cat``` with ```>``` option. 

``` cat [existingfilename]>[newfilename] ``` or ```cat [existingfilename]>>[newfilename]```  if you'd like to append the content to an existing file. 


I thought we can do better than that. It was cool to use cat and all but you still need to write the path correctly, I never get it right at first try anyways.

Another thing that was kinda distracting for me was adding pry to gemfile everytime in the labs where it is not included.

Both of these things, I thought, could be achived with a bash script. I started googling how to write a bash script. And it was way easier to come up with correct syntax than I anticipated. It took me less than an hour to figure out how to do it. 


Usage

In your home directory open ```.bash_profile``` with your favourite editor. And add following code somewhere appropriate, mine is all the way at the end.



```
#adds pry to Gemfile and creates or overwrites .gitignore file with the content from an existing .gitignore file from home directory
function cg {
  echo ------------------------------------------------------------------------
  echo ------------------------------------------------------------------------
  echo _____________________CURRENT .gitignore CONTENT_________________________
  cat .gitignore
  echo -------------------------END OF FILE------------------------------------
  echo ------------------------------------------------------------------------
}

function apag {

echo ------------------------------------------------------------------------
echo ------------------------------------------------------------------------
if grep -q -R "pry" Gemfile
then
  echo "                       Gemfile includes Pry!"
else
  echo "gem 'pry'" >> Gemfile
  echo "                        Pry added to Gemfile"
fi
echo ------------------------------------------------------------------------
echo ------------------------------------------------------------------------
echo ""
echo ""
if what .gitignore > /dev/null 2>&1;
then
  cg
  echo "Do you wish to overwrite .gitignore file?"

  select yn in "Yes" "No"; do
    case $yn in
      Yes ) cat ~/.gitignore > .gitignore; break;;
      No ) break;;
    esac
  done
else
  cat ~/.gitignore > .gitignore
  cg
fi
  echo Done!
}

```

...and then run ```apag```...
Short for AddPryAndGitignore. You can change it to anything you like. 

