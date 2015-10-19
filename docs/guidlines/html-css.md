### Html
We usally use bootstrap3 html. That's are current standard.

### Css
We try to use .less for all css coding here on ezy. So if you need to change an legacy project.
You should add a new .less file with the same name and copy the .css file to that. So we can start getting .less in.

Also we should always avoid in-line css and use css files for all CSS.

#### Naming

#### Capitalizing
We will always name our classes with lowercase.

for example:
`.my-button { ... }` 

#### Semantic naming

We should name our classes on what they do and not a color or what css it contains. For example  
 `.confirm-button` is fine , but not `.blue-button`

#### LESS
LESS files should be loaded in a modular way where the partial files is prefixed with a underscore.  
example : 
`@import "_animations.less";`  
This LESS file will not compile into a css file (since we are loading it in a modular way the css file is not needed).

By loading LESS files in this way we get more structure and are able to reuse the files needed in each part of the page.  


Another advantage that comes with this is that in Visual Studio 2015 the Web Essentials tool has removed the LESS compilation and moved it into an [extension](https://visualstudiogallery.msdn.microsoft.com/3b329021-cd7a-4a01-86fc-714c2d05bb6c). This extension is using a json file for configuration where you need to add ALL the less files that are going to be compiled on build. Since we are only interested in compiling one file, the one we load all the partial ones into, then we need to add only this one in the json configuration file. 







