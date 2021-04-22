# Git-github-markdown

1) the command line (explain one example per command). Define the basic comands of the prompt to:

## Create and delete directories
Create Directories
Directories (also known as folders in Windows) can also be created with a number of different techniques. The most simple way is with the ‘mkdir’ command, simply specify the name of the directory to create afterwards.

[root@rhel8 ~]# mkdir directory

This will create a new directory (or folder) called ‘directory’ in the current working directory. We can now enter it and use it to store content in.

[root@rhel8 ~]# cd directory/
[root@rhel8 directory]# pwd
/root/directory

We can also use the ‘mkdir’ command with the ‘-p’ flag to create parent directories if they do not already exist. In the example below, we can create the sub directory structure /root/directory/one/two/three in one step:

[root@rhel8 directory]# mkdir -p one/two/three
[root@rhel8 directory]# ls -lR
.:
total 0
drwxr-xr-x. 3 root root 16 May 14 00:56 one

./one:
total 0
drwxr-xr-x. 3 root root 18 May 14 00:56 two

./one/two:
total 0
drwxr-xr-x. 2 root root 6 May 14 00:56 three

./one/two/three:
total 0

The ‘-R’ flag from ‘ls’ will list all directories within recursively. 

Delete Directories
Deleting directories is a little different. The ‘rm’ command is still used, however look what happens if we try to delete a directory in the same manner as a file:

[root@rhel8 ~]# rm directory/
rm: cannot remove ‘directory/’: Is a directory

By default, to remove a directory we need to use the ‘-d’ flag, which is used to remove an empty directory.

[root@rhel8 ~]# rm -d directory/
rm: cannot remove ‘directory/’: Directory not empty

Note that I said empty! To demonstrate this, let’s create a fresh empty directory called ’empty’ and try again:

[root@rhel8 ~]# mkdir empty
[root@rhel8 ~]# rm -vd empty/
rm: remove directory ‘empty/’? y
removed directory: ‘empty/’

In the example above I’ve also used the ‘-v’ flag for verbose, this just outputs some extra information. In this case, the final line confirming that the directory has been removed is given due to the increased verbosity.

## navigate 

Show files
Command	ls	
Description List files 

C:\User\Example
$ ls
file1.txt DirectoryName1/

C:\User\Example
$ ls *.txt
file1.txt 

The directories with the ending "/" and the files with their respective extension are shown.


## compare 

## find files, folders and inside files

Command	find /dir/ -name name*
Description Find files starting with "name" in "dir"

C:\User\Example
$ ls
DirectoryName1/

C:\User\Example
$ touch file1.txt
 
C:\User\Example
$ ls
file1.txt DirectoryName1/

## create and edit text files

Formatting markdown files
Basic formatting
To bold a piece of text, place * around it; i.e. this is *bold*.


Headings
You can use the # symbol to create a heading i.e.

# Heading 1
## Heading 1.1
### Heading 1.1.1

 

Url Links
Wrap the word you want to hyperlink with square brackets, and follow with the linked url in regular brackets. Ie: [google](http://www.google.com)

 
Blockquote
To insert an aside (or blockquote), start a line with > character.

> Important Quotation

Horizontal line
To insert a full width horizontal line, you can type *** on any empty line.

***

Inserting images
Click on the list icon in the top right corner of your screen (just below your user name)

Select the Images tab

Search for your image in the grey field

Drag and drop your selected image into your file


## get the state of the computer

-$ lsb\_release -a

2) The git / github with examples:

## the initial configuration
Configure the Parser
Most of the time, the default configuration should be good enough. In those times you can just call the defaultTransform function on the parser class:

use Michelf\Markdown;
$html = Markdown::defaultTransform($text);
If you need some things to be different, instantiate your own parser and set a few variables:

use Michelf\Markdown;
$parser = new Markdown;
$parser->empty_element_suffix = ">";
$html = $parser->transform($text);
Configuration variables on the MarkdownExtra parser work the same way.

Markdown Configuration
empty_element_suffix = " />"
This is the string used to close tags for HTML elements with no content such as <br> and <hr>. The default value creates XML-style empty element tags which are also valid in HTML 5.

tab_width = 4
This is the width of a tab character on input. Changing this will affect how many spaces a tab character represents.

Note: Keep in mind that when the Markdown syntax spec says “four spaces or one tab”, it actually means “four spaces after tabs are expanded to spaces”. So setting tab_width to 8 will make the parser treat a tab character as two levels of indentation.

hard_wrap = false
Setting this to true will change line breaks into <br /> when the context allows it. When set to false, following the standard Markdown syntax these newlines are ignored unless they a preceded by two spaces.

no_markup = false
Setting this variable to true will prevent HTML tags in the input from being passed to the output.

Important: This is not a protection against malicious javascript being injected in a document. See why in Markdown and XSS.

no_entities = false
Setting this variable to true will prevent HTML entities (such as &lt;) from being passed verbatim in the output as it is the standard with Markdown. Instead, the HTML output will be &amp;tl; and once shown in shown the browser it will match perfectly what was written.

predef_urls = array()
You can predefine reference links by setting predef_urls to a list of urls where the key is the name of the reference. For instance:

$parser->predef_urls = array('ref' => 'https://michelf.ca/');
will make this Markdown input to create a link:

This is [my website][ref].
predef_titles = array()
Use predef_titles to set the title of the link references passed in predef_urls. As for predef_urls, the key is the reference name.

url_filter_func = null
This variable makes it possible to transform URLs for links and images in the document by specifying a function:

$parser->url_filter_func = function ($url) {
    return strtolower($url);
};

This can be used to fix unsecure URLs, transform local URLs into complete ones, etc.

header_id_func = null
Headers can have their id attribute auto-generated by setting this configuration variable to a function:

$parser->header_id_func = function ($text) {
    return preg_replace('/[^a-z0-9]/', '-', strtolower($text));
};

code_block_content_func = null
The content of code blocks can be transformed to HTML with a custom function. A simple version of this function will only change the special characters to HTML entities:

$parser->code_block_content_func = function ($code, $language) {
    return htmlspecialchars($code);
};

A more advanced version could use the $language parameter and perform syntax coloring. The result returned by this function is automatically wrapped in a <pre><code>.

code_span_content_func = null
The content of code code spans can be transformed to HTML with a custom function. A simple version of this function will only change the special characters to HTML entities:

$parser->code_span_content_func = function ($code) {
    return htmlspecialchars($code);
};

A more advanced version could infer the language of the code and perform syntax coloring. The result returned by this function is automatically wrapped in a <code>.

enhanced_ordered_list = false
Setting this to true allows ordered list to start with a number different from 1.

(Note: this is set to true by default for Markdown Extra.)

## starting a project from zero or cloning an existing repository

Cloning the repository
Once you've got the repository URL, open up the command line and navigate to the directory where you want the code to live. I have a folder called Projects located inside my home directory where I keep all the code I work on.

When you're ready to clone type the following command, making sure to replace the URL with the one you just copied from GitHub:

git clone git@github.com:robertlyall/shop.git
If successful, the output will look a little something like this:

Cloning into 'shop'...
remote: Enumerating objects: 27, done.
remote: Counting objects: 100% (27/27), done.
remote: Compressing objects: 100% (23/23), done.
remote: Total 27 (delta 2), reused 27 (delta 2), pack-reused 0
Receiving objects: 100% (27/27), 44.39 KiB | 478.00 KiB/s, done.
Resolving deltas: 100% (2/2), done.
By default, Git will create a folder with the same name as the repository, however you can change this by providing an extra argument to the git clone command.

For example, if you'd rather clone the project into a folder named shop-website, you would enter the following command:

git clone git@github.com:robertlyall/shop.git shop-website

## basic workflow commands to stage and commit 
Unstaging a Staged File
The next two sections demonstrate how to work with your staging area and working directory changes. The nice part is that the command you use to determine the state of those two areas also reminds you how to undo changes to them. For example, let’s say you’ve changed two files and want to commit them as two separate changes, but you accidentally type git add * and stage them both. How can you unstage one of the two? The git status command reminds you:

$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
    modified:   CONTRIBUTING.md
Right below the “Changes to be committed” text, it says use git reset HEAD <file>…​ to unstage. So, let’s use that advice to unstage the CONTRIBUTING.md file:

$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M	CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
The command is a bit strange, but it works. The CONTRIBUTING.md file is modified but once again unstaged.

Note
It’s true that git reset can be a dangerous command, especially if you provide the --hard flag. However, in the scenario described above, the file in your working directory is not touched, so it’s relatively safe.

For now this magic invocation is all you need to know about the git reset command. We’ll go into much more detail about what reset does and how to master it to do really interesting things in Reset Demystified.

## push to a remote repository

You can edit the file with notepad or any text editor.
add the file to the staging area with git add HelloWorld.md
commit the file to your local repository with git commit -m "edit md"
push the file to Github with git push origin master

## branches: create, delete, save/commit & merge 

Creating
git checkout -b $branchname
git push origin $branchname --set-upstream
Creates a new branch locally then pushes it.

Delete a local branch
git branch -d $branchname
Deletes the branch only if the changes have been pushed and merged with remote.

Get current sha1
git show-ref HEAD -s

Reset branch and remove all changes
git reset --hard

Undo commits to a specific commit
git reset --hard $commit_id

Now push safely to your branch
git push --force-with-lease

Or push brutally to your branch
git push --force

## gitflow. 
![image](https://user-images.githubusercontent.com/78174560/115681690-60984080-a31a-11eb-86fd-7115f439b757.png)

Gitflow es una especie de idea abstracta de un flujo de trabajo de Git. Esto quiere decir que ordena qué tipo de ramas se deben configurar y cómo fusionarlas. Explicaremos brevemente los objetivos de las ramas a continuación. El conjunto de herramientas de git-flow es una herramienta de línea de comandos propiamente dicha que tiene un proceso de instalación. El proceso de instalación de git-flow es sencillo. Los paquetes de git-flow están disponibles en varios sistemas operativos. En los sistemas OSX, puedes ejecutar brew install git-flow. En Windows, tendrás que descargar e instalar git-flow. Después de instalar git-flow, puedes utilizarlo en tu proyecto ejecutando git flow init. Git-flow es un envoltorio de Git. El comando git flow init es una extensión del comando predeterminado git init y no cambia nada de tu repositorio aparte de crear ramas para ti.
El primer paso es complementar la maestra predeterminada con una rama de desarrollo. Una forma sencilla de hacerlo es que un desarrollador cree una rama de desarrollo vacía localmente y la envíe al servidor:

git branch develop
git push -u origin develop
Esta rama contendrá el historial completo del proyecto, mientras que la maestra contendrá una versión abreviada. A continuación, otros desarrolladores deberían clonar el repositorio central y crear una rama de seguimiento para desarrollo.

Al utilizar la biblioteca de extensiones de git-flow, ejecutar git flow init en un repositorio existente creará la rama de desarrollo:

$ git flow init


Initialized empty Git repository in ~/project/.git/
No branches exist yet. Base branches must be created now.
Branch name for production releases: [master]
Branch name for "next release" development: [develop]


How to name your supporting branch prefixes?

Feature branches? [feature/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []


$ git branch
* develop
 master
 
