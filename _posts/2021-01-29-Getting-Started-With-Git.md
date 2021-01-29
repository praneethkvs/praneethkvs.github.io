---
layout: post
title: Getting Started with Git.
---

This is a brief introductory tutorial on getting started with github and creating a repository.   

Create a GitHub account by Signing up from the following url : "https://github.com/".  
Download git for Windows from the following url : "https://git-scm.com/downloads".

First thing we need to do after signing in to the website is to create a new repository using the green "New repository" button.
Type in the repository name, let's call it "firstrepo", leave everything else blank and hit "Create repository".
Check the "initialize with a readme if not checked".

We have now created a new repository in your github account, aah but there is nothing there, it's completely empty.
Well then let's dive in deep.

Now open "Git Bash" that you have just installed on your computer.
This should bring up a command terminal.
Let's get coding.

Now we introduce ourselves to git, Run the following Commands
```
git config --global user.name "your_username_here"
git config --global user.email "your_email_here"
```

Now that git knows who we are, we can continue.

We change our working directory to "C:/Users/Admin".
This is where we will store all or local repository files.   
```cd ~/``` 

Now we create a new directory, let's call it "myrepos" where we will store all our repositories.   
```mkdir myrepos```

Change directory to "myrepos" folder.   
```cd myrepos ```

Now that we are in the myrepos folder, we can start using some real "git" commands.
We need to get the remote repository that we created on github into this folder.   
```git clone "https://github.com/yourusername/firstrepo"```   
This is the url of the repository that we just created on github.

This creates a folder in myrepos with the name corresponding the repository name, so in this case you should see a folder named firstrepo.
Let's get into it:      
```cd firstrepo```


The next git command shows the status of your repository i.e; files and folders in the "firstrepo" directory.   
```git status ```
It should say "nothing to commit, working tree clean" as we haven't made any changes yet.

This let's you see all the files in the folder "firstrepo".   
```ls```
At this point you must be seeing a single file README.md

Now we can create a new file from bash directly or copy all the files that you want to upload into the repository and paste them into the "firstrepo" folder.
By running the "status" command again, we can now see all the files in the myrepo folder but they are labelled as untracked files, i.e githb is ignoring the files.   
```git status```

If you want to add a single file, This adds the file filename and now git recognizes the file and acknowledges it.   
```git add filename```

To add all the files.   
```git add *```

This command commits all the changes made to the git repository.
-m flag tells git to treat text following it as a message.
Make sure that you have a message or git will throw an error.   
```git commit -m "Add files"```

Lastly we need to update the changes made to our local repository and mirror these changes in the remote repository.	
For this we use the "push" command.    
```git push``` 
This might prompt you to login to git if it is the first time you opened it up.

The command line will output a few lines of code and if you do not get an error you are done.
Now go visit yor github repository page and Voila your files should be there.
You have just created your first GitHub repository. Rejoice. 



<b>Some useful git commands:   </b>
```
git ls-files #lists all available files in the repository.
git rm filename #deletes the file from the repository, Do not forget to push the changes after deleting files.
git add * #adds all files and folders n the repository in a single go
git add <folder>/* # adds all files within folder to git repository.
```
